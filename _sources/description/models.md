(description_models)=
# Music Description Models
Deep learning models for music description via natural language typically fit into one of two designs:

- Encoder-decoder
- Multimodal (adapted) LLM

## Encoder-Decoder Models
This is the modelling framework of the earliest DL music captioning models.
Encoder-decoder models first emerged in the context of sequence-to-sequence tasks, where they were first applied to the machine translation task and later adapted to image and audio captioning.

At a high-level, models of this type are composed of two main modules, an encoder and a decoder. Although there are several variations, in the simplest design of these models, the encoder is resposible for processing the
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
Beyond architectural choices for the core modules, a key aspect that differentiates different encoder-decoder models is the type of mechanism employed to condition language generation on the audio input. This is typically tied to the architecture used in the encoder and decoder modules. In the simplest of cases, the encoder outputs a single, fixed-sized embedding for the entire input sequence, which we'll call $\boldsymbol{a}$, and the decoder, e.g. an RNN, is initialised with this embedding. More precisely, the RNN initial state $\boldsymbol{h}_0$ is set to the encoder output, or a (non-)linear projection thereof:

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
\boldsymbol{A}\left(\boldsymbol{q}^{\alpha}_{i}, \boldsymbol{K}^{\beta}, \boldsymbol{V}^{\beta}\right)=\operatorname{softmax}\left(\frac{\boldsymbol{q}^{\alpha}_{i} K^{\beta}}{\sqrt{d_{k}}}\right) \boldsymbol{V}^{\beta}
$$


```{figure} ./img/lp_musiccaps.png
---
name: lp_musiccaps
width: 500px
align: center
---

```

In addition to the type of mechanism used, depending on the level at which modalities are combined, it is also common to distinguish between *early* (i.e. at the input level), *intermediate* (at the level of latent representations produced by an intermediate step in the overall processing pipeline) or *late* fusion (i.e. at the output level). We note that the terms *early, intermediate* and *late* fusion do not have an unequivocal definition and are used slightly differently in different works.

## Multimodal Autoregressive Transformers
The success of Large Language Models (LLMs) has largely influenced the development of music description in recent years. As a consequence, today's state-of-the-art models rely on LLMs in one form or another. Typically, this means that music description systems closely mimic text-only autoregressive modelling via Transformers, but within this framework we can distinguish two main variants: adapted text-only LLMs and natively multimodal LLMs. 

### Adapted LLMs
One modelling paradigm that has become particularly popular is that of adapted (multimodal) LLMs. At the core of this approach is a pre-trained text-only LLM, which is adapted to take in inputs of different modalities
such as audio. This is achieved via an *adapter* module, a light-weight neural network trained to map embeddings produced by an audio feature extractor (usually pre-trained and then frozen) to the input space of the LLM. As a result of this adaptation process, the LLM can then receive audio embeddings alongside text embeddings. 

Let's look at some examples of adapter modules in the literature.

🚧.


{cite}`gao_music_2023` {cite}`liu_music_2024` {cite}`deng_musilingo_2024` {cite}`hussain2023m` {cite}`gardner2023llark`

There are several techniques to pass audio-text embeddings 

### Natively Multimodal Autoregressive Transformers
Other autoregressive Transformer models for music descriptions share a similar core modelling mechanism to adapted LLM. But one key difference is that, while adapted LLMs require modality-specific encoders, usually pre-trained separately, natively multimodal LLMs forgo this in favour of a unified tokenization scheme that treats audio tokens much like text tokens from the start.

🚧

### Instruction Tuning

## References

```{bibliography}
:filter: docname in docnames
```
