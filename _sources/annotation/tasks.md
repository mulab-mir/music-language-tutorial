# Music Description Tasks
Let's now look at the range of music description tasks, going from those that give the "simplest" form of output (a categorical label) to those that produce more complex dialogue-based outputs. Through this, we also review some of the history of deep learning-based AMU systems.

## From labels to captions

Traditionally, music description in MIR has consisted of classification-based systems that learn to predict a single (or multiple) label(s) based on an audio input, 
with each label corresponding to a specific, pre-assigned descriptor. This is the case with tasks like music classification tasks on categories such as genre {cite}`tzanetakis2002musical`, instrument {cite}`herrera2003automatic`, mood {cite}`kim2010music`, key [TODO], tempo [TODO], and more.

Classification produces categorical labels chosen from a predefined set which forms a fixed, and often small-size, vocabulary: $f(x) = \mathbf{p} \in [0, 1]^L$. Therefore this type of description is limited in its expressivity, as it cannot adapt to new concepts, or model the relationship between labels.


```{figure} ./img/tags.png
---
name: tagging
---

```

For this reason, in recent years many have started incorporating natural language in music description systems, developing models that map audio inputs 
to full sentences. Enjoying the benefits of natural language, these systems can produce descriptions that are more nuanced, expressive and human-like. Given our focus on natural language, we mostly look at this type of music description in the rest of this tutorial.

```{figure} ./img/caption.png
---
name: captioning
---

```

### Music Captioning
In music audio captioning, the goal is to generate natural language outputs describing an audio input. We can think of it as a type of conditional language modelling, where we seek to predict the next token in a sequence, based not only on prior text tokens, but also on the audio:

$$
P(y|a) = \prod_{t=1}^{n} P(y_t | y_1, y_2, \ldots, y_{t-1}, a)
$$

Music captioning can be performed at the sub-track, track or multi-track level, depending on whether the audio input is a segment from a longer track (typically fixed-sized), a whole variable-length track, or a sequence of multiple tracks (i.e. a playlist). In the latter case, we usually refer to this type description task by *playlist captioning*. Instead, when using *music captioning* we usually mean captioning of either a clip or full track {cite}`manco2021muscaps`.

## From captions to dialogues
Some of them, in even more recent developments, they can answer questions or engage in multi-turn conversations about audio inputs. 

Largely influenced by dialogue-based LLM applications, 

(Audio, text) --> text

### Music Question Answering 

{cite} @

### Music Dialogue Generation

## References

```{bibliography}
:filter: docname in docnames
```
