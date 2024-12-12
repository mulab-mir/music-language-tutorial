(description_models)=
# Models

Deep learning models for music description via natural language typically fit into one of two designs:

- Encoder-decoder
- Multimodal Autoregressive Models

In {numref}`description_models_table` below we give an overview of music description models from 2016 to today.  * denotes taks that don't fall under the music description umbrella but are still addressed by the model.

```{table} Music description models.
:name: description_models_table
| Model | Type | Task(s) | Weights | Training dataset | 
| ------- |  ------ |    ---- | ---- | ---- | 
| Choi *et al.* {cite}`manco2021muscaps` | Encoder-decoder | Playlist captioning | ❌ | Private | 
| MusCaps {cite}`manco2021muscaps` | Encoder-decoder | Captioning, retrieval* | ❌ | Private | 
| LP-MusicCaps {cite}`manco2021muscaps` | Encoder-decoder | Captioning | ✅ |  | 
| MusCaps {cite}`manco2021muscaps` | Encoder-decoder | Captioning, retrieval* | ❌ | Private | 
| BLAP {cite}`lanzendorfer_blap_2024` | Encoder-decoder |  |  |  |
| MuLLama {cite}`liu_music_2024` | Adapted LLM |  |  |  |
| MusiLingo {cite}`deng_musilingo_2024` | Adapted LLM |  |  |  |
| M2UGen{cite}`hussain2023m` | Adapted LLM |  |  |  |
| LLark {cite}`gardner2023llark` | Adapted LLM |  |  |  |
```

## Encoder-Decoder Models
This is the modelling framework of the earliest DL music captioning models.
Encoder-decoder models first emerged in the context of sequence-to-sequence tasks (e.g. machine translation). It is easy to see many tasks can be cast as sequence-to-sequence, so encoder-decoder models found wide use in image captioning first, and audio captioning shortly after, including music.

As the name suggests, models of this type are composed of two main modules: an *encoder* and a *decoder*. Although there are several variations, in the simplest design of these models, the encoder is resposible for processing the
input sequence (i.e. audio input) into an intermediate representation (the context $c$)

$$
c = f_{\text{encoder}}(X)
$$

and the decoder then "unrolls" this representation 
into a target sequence (e.g. text describing the audio input), typically computing the probability distribution over possible tokens at each step in the sequence, conditioned on the context $c$:

$$
P_{\theta}(Y | c) = \prod_{t=1}^{n} P(y_t | y_1, y_2, \ldots, y_{t-1}, c).
$$

### Architectures
When it comes to the design of the encoder and decoder components, the general philosophy is to adopt state-of-the-art architectures for the respective modalities, balancing our requirements around possible domain-specific restrictions (e.g. the need to capture features at different timescales in music signals), with the computational and data budget we have at our disposal. This is to say that there are many possible designs for encoder-decoder music captioners in theory, but most follow standard choices. Let's review some below.

The first example of encoder-decoder model for music description appeared in work by Choi *et al.* {cite}`choi2016towards`. While this did not yet produce well-formed sentences, a later model by Manco *et al.*, MusCaps {cite}`manco2021muscaps`, consolidated the use of a similar architecture for track-level music captioning. These early iterations of encoder-decoder music captioners employed CNN-based audio encoders alongside 
RNN-based language decoders. More recent iterations of this framework typically make use of a Transformer-based language decoder (e.g. based on Transformer decoders such as GPT-2 {cite}`gabbolini-etal-2022-data` or BART {cite}`doh2023lp`), alongside CNNs {cite}`gabbolini-etal-2022-data` or Transformer audio encoders {cite}`srivatsan2024retrieval`, and sometimes a hybrid of both {cite}`doh2023lp`.

```{figure} ./img/encoder_decoder.png
---
name: encoder_decoder
width: 600px
align: center
---

```

### Conditioning and Fusion
Now that we've discussed how to choose our encoder and our decoder, how do we connect the two in order to join the two modalities we are translating between? Of course there are standard ways to pass the encoder output to the decoder, but since we are dealing with different modalities, this requires a bit more care than usual. The type of mechanism employed to condition language generation on the audio input is in fact one of the key differentiating aspects between different encoder-decoder models. In practice, this choice is tied to the network architecture of our encoder and decoder modules. In the simplest of cases, the encoder outputs a single, fixed-sized embedding for the entire input sequence, which we'll call $\boldsymbol{a}$, and the decoder, e.g. an RNN, is initialised with this embedding. More precisely, the RNN initial state $\boldsymbol{h}_0$ is set to the encoder output, or a (non-)linear projection thereof:

$$
\boldsymbol{h}_0 = \boldsymbol{a}.
$$

In most cases, however, we deal with more sophisticated architectures, and conditioning is realised through **fusion** of audio and text representations. 
Earlier models with RNN-based text decoders employ a range of fusion mechanisms, such as feature concatenation or cross-modal attention {cite}`manco2021muscaps`. Concatenation as a modality fusion mechanism in RNNs typically consists of concatenating an audio embedding (e.g. the output of the encoder module $\boldsymbol{a}$) to the input $\boldsymbol{x}$, so that an RNN state $\boldsymbol{h}$ depends on $[\boldsymbol{a}; \boldsymbol{x}]$, or to the previous state vector $[\boldsymbol{a}; \boldsymbol{h}_{t-1}]$, and sometimes to both. In this case, we assume that the encoder produces a single audio embedding. 

If our encoder produces instead a sequence of audio embeddings, and we wish to retain the sequential nature of the conditioning signal, an alternative way to achieve fusion is through **cross-attention**. In this case, instead of concatenating the same audio embedding at every time step $t$, we can compute attention scores $\beta_{t i}$ to suitably weigh each item in the audio sequence $\boldsymbol{a}_i$ differently at each time step $t$:

$$
\hat{\boldsymbol{a}}_t=\sum_{i=1}^L \beta_{t i} \boldsymbol{a}_i, 
$$

where the attention scores are given by

$$
\beta_{t i}=\frac{\exp \left(e_{t i}\right)}{\sum_{k=1}^L \exp \left(e_{t k}\right)}.
$$

The exact computation of $e_{t i}$ depends on the score function used. For example, we can have:

$$
e_{t i}=\boldsymbol{w}_{a t t}^{\top} \tanh \left(\boldsymbol{W}^{a t t} [\boldsymbol{a}; \boldsymbol{h}_{t-1}]\right),
$$

where $\boldsymbol{w}_{a t t}$ and $\boldsymbol{W}^{a t t}$ are learnable parameters.

Similar types of attention-based fusion can also be used in Transformer-based architectures {cite}`gabbolini-etal-2022-data` {cite}`doh2023lp`. In this setting, instead of the cross-attention shown above, fusion can also be directly embedded within the Transformer blocks by modifying their self-attention mechanism to depend on both text and audio embeddings, though exact implementations of co-attentional Transformer layers vary between models:

$$
\boldsymbol{A}\left(\boldsymbol{q}^{\text{text}}_{i}, \boldsymbol{K}^{\text{audio}}, \boldsymbol{V}^{\text{audio}}\right)=\operatorname{softmax}\left(\frac{\boldsymbol{q}^{\text{text}}_{i} K^{\text{audio}}}{\sqrt{d_{k}}}\right) \boldsymbol{V}^{\text{audio}}
$$


```{figure} ./img/lp_musiccaps.png
---
name: lp_musiccaps
width: 500px
align: center
---
```

In addition to the type of mechanism used, depending on the level at which modalities are combined, it is also common to distinguish between *early* (i.e. at the input level), *intermediate* (at the level of latent representations produced by an intermediate step in the overall processing pipeline) or *late* fusion (i.e. at the output level). We note that the terms *early, intermediate* and *late* fusion do not have an unequivocal definition and are used slightly differently in different works.


## Multimodal Autoregressive Models
The success of Large Language Models (LLMs) has largely influenced the development of music description in recent years. As a consequence, today's state-of-the-art models rely on LLMs in one form or another. Typically, this means that music description systems closely mimic text-only autoregressive modelling via Transformers, but within this framework there are two main routes we can take. The first, and most common, is to adapt text-only LLMs so that they become multimodal by augmenting them with additional modelling components. A second option is to instead treat audio and text as sequences of tokens from the start, devising tokenization techniques and training on multiple modalities without additional modality-specific components. The line between these two approaches is not always clear. In the next section, we attempt to better define the salient characteristics of LLMs adapted to music-language inputs, and sketch out the newer trend towards natively multimodal models and its potential in music description.

Overall, a common thread in this line of work is the attempt to unify multimodal tasks by reframing all as text generation. When trained on music data, multimodal LLMs can therefore leverage their text-based interface to enable a variety of music understanding tasks by simply allowing users to query via text and obtain information about a given audio input. This is the machanism that enables the conversation-based music description tasks we have seen in the [Tasks](description_tasks) section.

### Adapted LLMs
One modelling paradigm that has become particularly popular in audio description, including music, is that of adapted (multimodal) LLMs. At the core of this approach is a pre-trained text-only LLM, which is adapted to take in inputs of different modalities
such as audio. This is achieved via an *adapter* module, a light-weight neural network trained to map embeddings produced by an audio feature extractor (usually pre-trained and then frozen) to the input space of the LLM. As a result of this adaptation process, the LLM can then receive audio embeddings alongside text embeddings. 

```{figure} ./img/adapted.png
---
name: adapted_llm
width: 600px
align: center
---
```

🚧

Alongside music-specialised multimodal LLMs, a LLM with general-audio understanding capabilities can similarly perform music description tasks such as captioning and MQA. Among these we count:
* SALMONN {cite}`tang_salmonn_2023`
* Pengi {cite}`deshmukh_pengi_2023`
* Qwen-Audio `chu_qwen-audio_2023`
* LTU
* [Audio-LLM: Activating the Capabilities of Large Language Models to Comprehend Audio Data](https://link.springer.com/chapter/10.1007/978-981-97-4399-5_13)

We don't discuss these in detail, but their high-level design is similar to the music-specialised models we've seen in this section.

#### Adapter Modules

#### Training 
From the perspective of training, similarly to the text-only setting, training adapted LLMs is usually broken into several stages. After pre-training and finetuning of the text-only part, the remaining components undergo a series of multimodal training stages, while the backbone LLM is either kept frozen or further finetuned. These steps are usually a mixture of multi-task pre-training and supervised finetuning, often including instruction tuning, all carried out on pairs of audio and text data. 

##### Instruction Tuning 

### Natively Multimodal AR Models
Other autoregressive Transformer models for music description share a similar core modelling mechanism to adapted LLM. But one key difference is that, while adapted LLMs require modality-specific encoders, usually pre-trained separately, natively multimodal LLMs forgo this in favour of a unified tokenization scheme that treats audio tokens much like text tokens from the start. 
This paradigm is sometimes referred to as mixed-modal early-fusion modelling.

It's worth noting that, at this time, this type of model is a promising direction for music description, rather than a fully established paradigm. Currently, no music-specialised multimodal AR Transformers exist, but some general-purpose models include music-domain data in their training and evaluation. This is in line with the overall trend of developing large-scale models that tackle all domains, but it remains to be seen what the impact of this modalling paradigm will be on music description in the years to come. Among current examples of this type of model that include music description we have:
* AnyGPT {cite}`doh2023lp`
* 

## References

```{bibliography}
:filter: docname in docnames
```
