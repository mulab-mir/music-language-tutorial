# Music Description Models
Deep learning models for music description via natural language typically fit into one of two designs:

- Encoder-decoder
- Multimodal (adapted) LLM

## Encoder-Decoder Models
This is the modelling framework of the earliest DL music captioning models.
Encoder-decoder models first emerged in the context of sequence-to-sequence tasks, where they were first applied to the machine translation task and later adapted to image and audio captioning.

At a high-level, models of this type are composed of two main modules, an encoder and a decoder. The encoder is resposible for processing the
input sequence (i.e. audio input) into an intermediate representation (the context $c$) and the decoder then "unrolls" this representation 
into a target sequence (e.g. text describing the audio input). 
$$
c = f_{\text{encoder}}(X)
$$

$$
P(Y | c) = \prod_{t=1}^{n} P(y_t | y_1, y_2, \ldots, y_{t-1}, c)
$$

The first example of encoder-decoder model for music description appeared in work by Choi *et al.* {cite}`choi2016towards`. While this did not yet produce well-formed sentences, a later model by Manco *et al* {cite}`manco2021muscaps` later consolidated the use of a similar architecture for track-level music captioning. These early iterations of encoder-decoder music captioners employed CNN-based audio encoders alongside 
RNN-based language decoders. More recent iterations of this framework typically make use of a Transformer-based language decoder, alongside CNNs 🚧 or Transformer audio encoders 🚧, and sometimes a hybrid of both {cite}`doh2023lp`.


Beyond architectural choices, a key aspect that differentiates different encoder-decoder models is the type of mechanism employed to fuse audio 
and text representations. The choice of this fusion mechanism is typically tied to the encoder architecture used. 
For example, in models that employ Transformer-based architectures, fusion often happens via cross-attention 🚧. Instead, earlier models with RNN-based text decoders employed a wider range of fusion mechanisms, such as feature concatenation or cross-modal attention, at various levels of the processing [pipeline], from early to late stages.

## Multimodal Autoregressive Transformers
The success of Large Language Models (LLMs) has largely influenced the development of music description in recent years. As a consequence, today's state-of-the-art models rely on LLMs in one form or another. Typically, this means that music description systems closely mimic text-only autoregressive modelling via Transformers, but within this framework we can distinguish two main variants: adapted text-only LLMs and natively multimodal LLMs. 

### Adapting LLMs to audio-text inputs
One modelling paradigm that has become particularly popular is that of adapted (multimodal) LLMs. At the core of this approach is a pre-trained text-only LLM, which is adapted to take in inputs of different modalities
such as audio. This is achieved via an *adapter* module, a light-weight neural network trained to map embeddings produced by an audio feature extractor (usually pre-trained and then frozen) to the input space of the LLM. As a result of this adaptation process, the LLM can then receive audio embeddings alongside text embeddings. 

Let's look at some examples of adapter modules in the literature.

🚧.


{cite}`gao_music_2023` {cite}`liu_music_2024` {cite}`deng_musilingo_2024` {cite}`hussain2023m` {cite}`gardner2023llark`

There are several techniques to pass audio-text embeddings 

## Other Modelling Paradigms
Other autoregressive Transformer models for music descriptions share a similar core modelling mechanism to adapted LLM. But one key difference is that, while adapted LLMs require modality-specific encoders, usually pre-trained separately, natively multimodal LLMs forgo this in favour of a unified tokenization scheme that treats audio tokens much like text tokens from the start.

🚧

## References

```{bibliography}
:filter: docname in docnames
:labelprefix: C
:keyprefix: c-
```
