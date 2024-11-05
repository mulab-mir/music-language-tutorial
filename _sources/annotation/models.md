# Music Description Models
Deep learning models for music description via natural language typically fit into one of two designs:

- Encoder-decoder
- Multimodal (adapted) LLM

## Encoder-Decoder Models
The encoder-decoder modelling framework emerged in the context of sequence-to-sequence tasks, and it was first applied to machine translation.
At a high-level, models of this type are composed of two main modules, an encoder and a decoder. The encoder is resposible for processing the
input sequence (i.e. audio input) into an intermediate representation (the context $c$) and the decoder then "unrolls" this representation 
into a target sequence (e.g. text describing the audio input). The first example of encoder-decoder model for music description appeared in work
by Choi *et al.* {cite}`choi2016towards`. These early iterations of encoder-decoder music captioners employed CNN-based audio encoders alongside 
RNN-based language decoders {cite}`choi2016towards` {cite}`manco2021muscaps`. More recent iterations of this framework typically make use of a 
Transformer-based language decoder, alongside CNNs [TODO] or Transformer audio encoders [TODO], and sometimes a hybrid of both [TODO].

$$
c = f_{\text{encoder}}(X)
$$

$$
P(Y | c) = \prod_{t=1}^{n} P(y_t | y_1, y_2, \ldots, y_{t-1}, c)
$$

Beyond architectural choices, a key aspect that differentiates different encoder-decoder models is the type of mechanism employed to fuse audio 
and text representations. The choice of this fusion mechanism is typically tied to the encoder architecture used. 
For example, in models that employ Transformer-based architectures, fusion often happens via cross-attention [TODO]. Instead, earlier models with RNN-based text decoders employed a wider range of fusion mechanisms, such as feature concatenation or cross-modal attention, at various levels of the processing [pipeline], from early to late stages.

## Multimodal Autoregressive Transformers

### Adapting LLMs to audio-text inputs
The success of Large Language Models (LLMs) has largely influenced the development of music description in recent years. As a consequence, 
today's state-of-the-art models rely on LLMs in one form or another. One modelling paradigm that has become particularly popular is that of
adapted (multimodal) LLMs. At the core of this approach is a pre-trained text-only LLM, which is adapted to take in inputs of different modalities
such as audio. This is achieved via an *adapter* module, a light-weight neural network trained to map embeddings produced by an audio feature extractor (usually pre-trained and then frozen) to the input space of the LLM. As a result of this adaptation process, the LLM can then receive audio embeddings alongside text embeddings. 

Let's look at some examples of adapter modules in the literature.

[TODO].

There are several techniques to pass audio-text embeddings 

## Other Modelling Paradigms
[TODO]

## References

```{bibliography}
:filter: docname in docnames
```
