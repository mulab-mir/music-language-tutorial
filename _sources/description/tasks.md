# Music Description Tasks
As we've established, music description encompasses multiple different tasks.
Let's now look at each of these in more detail, going from those that give the simplest form of output (a categorical label) to those that produce more complex, natural language-based outputs. Through this, we also review some of the history of deep learning-based AMU systems.

## From labels to captions

Traditionally, music description in MIR has been addressed through supervised classification-based systems that learn 
to predict a single (or multiple) label(s) based on an audio input, with each label corresponding to a specific, pre-assigned descriptor.
For example, we can train classifiers to describe music with respect to categories such as genre {cite}`tzanetakis2002musical`, instrument {cite}`herrera2003automatic`, mood {cite}`kim2010music`, and more.
Sometimes, multi-label classification (*auto-tagging*) is instead used to assign *tags* that cover more than one category, typically encompassing genre, mood, instrument and era {cite}`choi2017convolutional` {cite}`lee2017multilevel` {cite}`won2021transformer`.

A classifier $f$ maps the audio to categorical labels chosen from a predefined, often small-size, set of $K$ labels, $f: \mathcal{X} \rightarrow \{0, \dots, 1-K\}$. Therefore this type of description is limited in its expressivity, as it cannot adapt to new concepts, or model the relationship between labels. 

```{figure} ./img/tags.png
---
name: tagging
---

```

For this reason, in recent years, research on music description has shifted towards incorporating natural language, developing models that map audio inputs 
to full sentences, $g: \mathcal{X} \rightarrow \mathcal{V}^*$. In this case, $\mathcal{V}$ is the vocabulary, the set of tokens (e.g. words or sub-words tokens) and $\mathcal{V}^*$ denotes all possible sequences that can be formed with elements of $\mathcal{V}$. Enjoying the benefits of natural language, these systems can produce descriptions that are more nuanced, expressive and human-like. Given our focus on natural language, we mostly look at this type of music description in the rest of this tutorial.

```{figure} ./img/caption.png
---
name: captioning
---

```

### Music Captioning
In music audio captioning, the goal is to generate natural language outputs describing an audio input. We can think of it as a type of conditional language modelling, where we seek to predict the next token $y_t$ in a sequence $Y$ of length $L$, based not only on prior text tokens $y_{i<t}$, but also on the audio $a$:

$$
P(Y|a) = \prod_{t=1}^{L} P(y_t | y_1, y_2, \ldots, y_{t-1}, a).
$$

Music captioning can be performed at the sub-track, track or multi-track level, depending on whether the audio input is a segment from a longer track (typically fixed-sized), a whole variable-length track, or a sequence of multiple tracks (i.e. a playlist). In the latter case, we usually refer to this type of description task by *playlist captioning* {cite}`gabbolini-etal-2022-data`. Instead, when using the terms *music captioning* we usually mean captioning of either a clip or full track {cite}`manco2021muscaps` {cite}`manco2021muscaps` {cite}`wu2024futga`. 

Most work on music captioning so far has focused on describing global characteristics of the content, especially those that emerge at relatively long timescales (~10-30s), such as style and genre. In many cases, this type of caption does not contain references to time-localised events, temporal order, structure, or other types of temporally aware description, and can only convey a high-level, coarse summary of a musical piece. Temporal evolution within music audio is however a crucial aspect. For this reason, more recent works have proposed to extend the captioning task to also allow for fine-grained music descriptions {cite}`wu2024futga`.

## From captions to dialogues
Hand in hand with more flexible and fine-grained music captioning, the latest research efforts on music description have also started exploring models that can not only caption audio, but also answer questions or engage in multi-turn conversations about music inputs.
These models have been largely influenced by dialogue-based LLM applications.

(Audio, text) --> text

🚧

### Music Question Answering 

{cite}`gao_music_2023` {cite}`liu_music_2024` {cite}`deng_musilingo_2024` {cite}`hussain2023m` {cite}`gardner2023llark`

General audio with music: {cite}`tang_salmonn_2023` {cite}`deshmukh_pengi_2023` {cite}`chu_qwen-audio_2023`
🚧

### Music Dialogue Generation

{cite}

🚧

## References

```{bibliography}
:filter: docname in docnames
<!-- :labelprefix: A
:keyprefix: a- -->
```
