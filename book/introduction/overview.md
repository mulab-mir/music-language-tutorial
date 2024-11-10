# Overview of Tutorial

This tutorial will present the changes in music understanding, retrieval, and generation technologies following the development of language models.

```{figure} ./img/flow.png
---
name: scope
---
Illustration of the development of music and language models.
```

## Langauge Models

Chapter 2 presents an introduction to language models (LMs), essential for enabling machines to understand natural language and their wide-ranging applications. It traces the development from simple one-hot encoding and word embeddings to more advanced language models, including masked langauge model {cite}`devlin2018bert`, auto-regressive langauge model {cite}`radford2019language`, and encoder-decoder langauge model {cite}`raffel2020exploring`, progressing to cutting-edge instruction-following {cite}`wei2021finetuned` {cite}`ouyang2022training` {cite}`chung2024scaling` and large language models {cite}`achiam2023gpt`. Furthermore, we review the components and conditioning methods of language models, as well as explore current challenges and potential solutions when using language models as a framework.


## Music Description

```{figure} ./img/annotation.png
---
name: annotation
---
```

Chapter 3 offers an in-depth look at music annotation as a tool for enhancing music understanding. It begins with defining the task and problem formulation, transitioning from basic classification {cite}`turnbull2008semantic` {cite}`nam2018deep` to more complex language decoding tasks. The chapter further explores encoder-decoder models {cite}`manco2021muscaps` {cite}`doh2023lp` and the role of multimodal large language models (LLMs) in music understanding {cite}`gardner2023llark`. The chapter explores the evolution from `task-specific classification models` to `more generalized multitask models` trained with diverse natural language supervision. 


## Music Retrieval

```{figure} ./img/retrieval.png
---
name: retrieval
---
```

Chapter 4 focuses on text-to-music retrieval, a key component in music search, detailing the task's definition and various search methodologies. It spans from basic boolean and vector searches to advanced techniques that bridge words to music through joint embedding methods {cite}`choi2019zero`, addressing challenges like out-of-vocabulary terms. The chapter progresses to sentence-to-music retrieval {cite}`huang2022mulan` {cite}`manco2022contrastive` {cite}`doh2023toward`, exploring how to integrate complex musical semantics, and conversational music retrieval for multi-turn dialog-based music retrieval {cite}`chaganty2023beyond`. It introduces evaluation metrics and includes practical coding exercises for developing a basic joint embedding model for music search. This chapter focuses on how models address `users' musical queries` in various ways. 


## Music Generation

```{figure} ./img/generation.png
---
name: generation
---
```

Chapter 5 delves into the creation of new music through text-to-music generation techniques, emphasizing the production of novel sounds influenced by text prompts {cite}`dhariwal2020jukebox`. It introduces the concept of generating music without specific conditions and details the incorporation of text-based cues during the training phase. The discussion includes an overview of pertinent datasets and the evaluation of music based on auditory quality and relevance to the text. The chapter compares music generation methods, including diffusion models {cite}`chen2023musicldm` and discrete codec language models {cite}`agostinelli2023musiclm` {cite}`copet2024simple`. Furthermore, it examines the challenges associated with purely text-driven generation and investigates alternative methods of conditional generation that go beyond text, such as converting textual descriptions into musical attributes {cite}`wu2023music` {cite}`novack2024ditto`. 