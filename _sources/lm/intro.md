# Introduction

This section provides a high-level introduction to the research topics surrounding language models.

Language models using neural networks have been hugely successful in the recent years, and it’s been influential in many other fields of research not limited to natural language processing, including music information retrieval and generation as we’ll see in the later chapters of this tutorial.
This chapter is intended to be a 30,000 feet overview contextualizing language model research and how they can be used in a broad set of applications including music.
It is not intended to be too deep into any mathematical or technical details, but it tried to cover recent developments and latest challenges in the area.

### What are language models?

In the most general sense, a language model is a probability distribution defined over natural languages, so $P$ of some text:

$$P(\mathrm{some~~text})$$

It’s often defined as a conditional probability distribution, because we are usually interested in the probability of texts at a certain situation, that we can change or control when we want.

$$P(\underbrace{\textrm{some text}}_{\textrm{output}} | \underbrace{\textrm{condition}}_{input})$$

So in that sense, in this conditional probability distribution of some text given the condition, the condition is the input, and the text can be considered as the output of this language model.

This is a really flexible framework, because the input and output can be basically anything.
They can be question and answer, and we have a question-answering models like T5.
If the output is a sub-segment of a text and the condition is its surrounding texts, it’s a masked language model that can fill in the blanks, like BERT.
In autoregressive language models like GPT, the condition is any prefix and the output is its continuation.
More specifically, in conversational AI models like ChatGPT, the prefixes and continuations are formatted so that the output can be a conversational response to the provided chat history.

| output |  input | |
|:------:|:-------:|----|
| answer | question | sequence-to-sequence models (e.g. T5) |
| substring | surroundings | masked language models (e.g. BERT) |
| continuation | prefix | autoregressive language models (e.g. GPT) |
| chat response | chat history | conversational AI (e.g. ChatGPT) |

So those were the inputs and outputs. What about the model part that we simply called $P$ ?
For the model part as well, the definition of language models do not limit us on any specific implementation.
The model is usually defined using a set of parameters, denoted with subscript $\theta$:

$$P_{\theta}(\textrm{some text} | \textrm{condition})$$

Until neural networks started to really work, $n$-gram models have been the standard approach to language modeling, which is based on the distribution of $n$ consecutive words.
More recently, language models based on recurrent neural networks such as LSTMs or Transformers have been proven to be more effective methods for capturing long-range dependencies and better understanding natural language.

As for the parmaeters, in $n$-gram models, the parameters are simply the counts of $n$-grams appearing in the training corpus.
Whereas in neural-network-based language models, the parameters are learned using gradient descent.

| architecture $P$ | parameters $\theta$ |
|:----:|:----:|
| $n$-grams | counting |
| RNNs | gradient-based optimization |
| Transformers | gradient-based optimization |

This covers what language models are at the most abstract level.
In the next section, we dig one step further into how they are implemented in practice.
