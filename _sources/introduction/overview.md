# Background

```{figure} ../img/understanding.png
---
name: overview
---
```

The journey of Music and Language Models started with two basic human desires: to **understand music** deeply and to **listen to the music** we want whenever we want, whether it's existing artist music or creative new music. These fundamental needs have driven the development of technologies that connect music and language. This is because language is the most fundamental communication channel we use, and through this language, we aim to communicate with machines.

## Early Stage of Music Annotation and Retrieval

The first approach was Supervised Classification. This method involved developing models that could predict appropriate Natural Language Labels (Fixed-Vocabulary) for given audio inputs. These labels could cover a wide range of musical attributes including genre, mood, style, instruments, usage, theme, key, tempo, and more {cite}`sordo2007annotating`. The advantage of Supervised Classification was that it automated the annotation process. As music databases grew richer with these annotations, in the retrieval phase, we could use cascading filters to find desired music more easily  {cite}`eck2007automatic` {cite}`lamere2008social`. The research on supervised classification evolved over time. In the early 2000s, with advancements in pattern recognition methodologies, the focus was primarily on feature engineering {cite}`fu2010survey`. As we entered the 2010s, with the rise of deep learning, the emphasis shifted towards model engineering {cite}`nam2018deep`.

```{note}
If you're particularly interested in this area, please refer to the following tutorial:

- [ISMIR2008-Tutoiral: SOCIAL TAGGING AND MUSIC INFORMATION RETRIEVAL](https://www.slideshare.net/slideshow/social-tags-and-music-information-retrieval-part-i-presentation)
- [ISMIR2019-Tutoiral: Waveform-based music processing with deep learning](https://zenodo.org/records/3529714)
- [ISMIR2021-Tutoiral: Music Classification: Beyond Supervised Learning, Towards Real-world Applications](https://music-classification.github.io/tutorial/landing-page.html)
```

However, supervised classification has two fundamental limitations. First, it only supports music understanding and search using fixed labels. This creates a problem where the model cannot handle unseen vocabulary. Second, language labels are represented through one-hot encoding, which means the model cannot capture relationships between different language labels. As a result, the trained model is specifically learned for the given supervision, limiting its ability to generalize and understand a wide range of musical language.


```{figure} ../img/listening.png
---
name: overview
---
```

## Early Stage of Music Generation

Compared to Discriminative Models $p(y|x)$, which are relatively easier to model, Generative Models $p(x|c)$ that need to model data distributions initially focused on generating short single-instrument pieces or speech datasets rather than complex multi-track music. In the early stages, unconditioned generation $p(x)$  methods such as likelihood-based models (represented by WaveNet {cite}`van2016wavenet` and SampleRNN {cite}`mehri2016samplernn`) or adversarial models (represented by WaveGAN {cite}`donahue2018adversarial`) were studied. 

Early Conditioned Generation models $p(x|c)$ included the Universal Music Translation Network {cite}`mor2018universal`, which used a single shared encoder and different decoders for each instrument condition, and NSynth {cite}`engel2017neural`, which added pitch conditioning to WaveNet Autoencoders. These models represented some of the first attempts at controlled music generation.

```{note}
If you're particularly interested in this area, please refer to the following tutorial:

- [ISMIR2019-Tutoiral: Waveform-based music processing with deep learning, part 3](https://zenodo.org/records/3529714)
- [Generating Music in the waveform domain - Sander Dieleman](https://sander.ai/2020/03/24/audio-generation.html#fn:umtn)
```

However, Generative Models capable of Natural Language Conditioning were not yet available at this stage. Despite the challenge of generating high-quality audio with long-term consistency, these early models laid the groundwork for future advancements in music generation technology.