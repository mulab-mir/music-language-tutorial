# Overview

## What is music description?
The goal of *automatic music description* (AMU) is to analyse music audio and "translate" it into human-readable form. 
As such, AMU is an umbrella term that covers several tasks. 

We can distinguish between different types of music description along two axes:
- **Abstraction**: the abstraction level of the features captured in the description ("*what* we describe")
- **Complexity**: the complexity of the description ("*how* we describe")

```{figure} ./img/description.png
---
name: overview
---

```

In this tutorial, we're particularly interested in natural language descriptions, which occupy [..] on the *complexity* axis, and usually span a wide range of abstraction levels within.

## Why do we need automatic music description?
Being able to automatically create descriptions of music audio content is useful for a variety of practical purposes. For example, through AMU we can:

- Annotate large collections of music for easier search, navigation and organisation
- Generate human-readable summaries of musical content for the Deaf or Hard-of-hearing, or when audio playback is not possible
- Automatically caption music sections in videos and films
- Produce educational resources 
- Enable personalised music recommendation systems using natural language queries

In the last few years, AMU systems have also become widely used for **synthetic data generation**. In this case, instead of finding a direct application, they are exploited to generate additional text data from unlabeled or partially labeled audio. The synthetic descriptions are then used to support training of machine learning models on other tasks that require (audio, text) pairs, such as [text-to-music retrieval]() and [text-to-music generation](). This is how some of the latest audio-text music datasets are produced (see []()).
