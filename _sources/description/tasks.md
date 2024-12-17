(description_tasks)=
# Tasks
Music description encompasses several different tasks.
Let's now look at each of these in more detail, going from those that give the simplest form of output (a categorical label) to those that produce more complex, natural language-based outputs. Through this, we also review some of the history of deep learning-based AMU systems.

## Music Classification
Traditionally, music description in MIR has been addressed through supervised classification-based systems that learn 
to predict a single (or multiple) label(s) based on an audio input, with each label corresponding to a specific, pre-assigned descriptor.
For example, we can train classifiers to describe music with respect to categories such as genre {cite}`tzanetakis2002musical`, instrument {cite}`herrera2003automatic`, mood {cite}`kim2010music`, and more.
Sometimes, multi-label classification (*auto-tagging*) is instead used to assign *tags* that cover more than one category, typically encompassing genre, mood, instrument and era {cite}`choi2017convolutional` {cite}`lee2017multilevel` {cite}`won2021transformer`.

A classifier $f$ maps the audio to categorical labels chosen from a predefined, often small-size, set of $K$ labels, $f: \mathcal{X} \rightarrow \{0, \dots, 1-K\}$. Therefore this type of description is limited in its expressivity, as it cannot adapt to new concepts, or model the relationship between labels. 

```{figure} ./img/tags.png
---
name: tagging
width: 600px
align: center
---

```

```{note}
If you would like to know more about music classification, there is an [ISMIR tutorial](https://music-classification.github.io/tutorial/landing-page.html) from 2021 covering this topic.
```

## Music Captioning
For this reason, in recent years, research on music description has shifted towards incorporating natural language, developing models that map audio inputs 
to full sentences, $g: \mathcal{X} \rightarrow \mathcal{V}^*$. In this case, $\mathcal{V}$ is the vocabulary and $\mathcal{V}^*$ denotes all possible sequences that can be formed with elements of $\mathcal{V}$. Enjoying the benefits of natural language, these systems can produce descriptions that are more nuanced, expressive and human-like.

The primary example of language-based music description task is *music captioning*, in which the goal is to generate natural language outputs describing an audio input:

```{figure} ./img/caption.png
---
name: captioning
width: 600px
align: center
---

```
We can think of it as a type of conditional language modelling, where we seek to predict the next token $y_t$ in a sequence $Y$ of length $L$, based not only on prior text tokens $y_{i<t}$, but also on the audio $a$:

$$
P(Y|a) = \prod_{t=1}^{L} P(y_t | y_1, y_2, \ldots, y_{t-1}, a).
$$

### Types of music captioning

Music captioning can be performed at the sub-track, track or multi-track level, depending on whether the audio input is a segment from a longer track (typically fixed-sized), a whole variable-length track, or a sequence of multiple tracks (i.e. a playlist). In the latter case, we usually refer to this type of description task by *playlist captioning* {cite}`gabbolini-etal-2022-data`. Instead, when using the terms *music captioning* we usually mean captioning of either a clip or full track {cite}`manco2021muscaps` {cite}`manco2021muscaps` {cite}`wu2024futga`. In a variation of the music captioning task, lyrics may be considered alongside audio as additional input data to base the description on, though this is only explored in one prior study {cite}`he2023alcap`. 

Music captioning is usually focused on describing global characteristics of the content, especially those that emerge at relatively long timescales (~10-30s), such as style and genre. In many cases, this type of caption does not contain references to time-localised events, temporal order, structure, or other types of temporally aware description, and can only convey a high-level, coarse summary of a musical piece. Temporal evolution within audio signals is however a crucial aspect to music. For this reason, more recent variants of this task focus on producing more fine-grained captions that capture structural characteristics  {cite}`wu2024futga`.

```{figure} ./img/finegrained.png
---
name: finegrained_captioning
width: 600px
align: center
---
```

## Music Question Answering
Hand in hand with more flexible and fine-grained music captioning, recent developments on music description have started going beyond static captioning towards more interactive, dialogue-based description. A primary example of this is *music question answering* (MQA), where the goal is to obtain a language output that answers a question about a given piece of music:

```{figure} ./img/mqa.png
---
name: finegrained_captioning
width: 600px
align: center
---

```

The MQA task is relatively recent, and the first example we find in the literature comes from Gao *et al.* (2023) {cite}`gao_music_2023`. In their work, the authors present a dataset of `(music, question, answer)` tuples and propose a baseline model trained to predict the answer from the music-question input, alongside items in an *aesthetic knowledge base*. 
However the MQA task has only become more established with the development of multimodal AR models such as LLark {cite}`gardner2023llark`, MusiLingo {cite}`deng_musilingo_2024` and MuLLaMa, which we discuss in more detail in the [Models](description_models) section.


## Conversational Music Description
Finally, an even more generalised form of music description can be achieved through *conversational music description* or *music dialogue generation*, in which the goal is to obtain suitable language outputs in response to both language inputs and accompanying music audio, in a multi-turn fashion:

```{figure} ./img/dialogue.png
---
name: dialogue
width: 600px
align: center
---
```

A key difference between dialogue-based description and one-off captioning is that, instead of an `audio --> text` mapping, we are now dealing with an `(audio, text) --> text` mapping. This is reflected in the different model designs typically considered for these tasks (see [Models](description_models)). Differently from simple MQA, in music dialogue generation, responses are expected to be based on the entire dialogue history instead of only considering the current input. 

In terms of real-world applications, the advantages of dialogue-based description are clear: instead of being constrained to a one-shot caption or answer, it allows users to provide text inputs to further instruct the model on what kind of information should be included, or how the text output itself should be structured. In short, these tasks make for a much more flexible approach which better reflects real-world use. One drawback is that they are harder to evaluate (see [Evaluation](description_evaluation))!

## References

```{bibliography}
:filter: docname in docnames
```
