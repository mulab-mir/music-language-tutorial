# Models

In this chapter, we review recent advances in audio-text joint embedding models and discuss useful design choices and tips for training them.


## Audio-Tag Joint Embedding

```{figure} ./img/choi_zeroshot.png
---
name: Audio-Tag Joint Embedding
---
```

The early audio-text joint embedding work introduced to the ISMIR community was by {cite}`choi2019zero`. They emphasized the effectiveness of pretrained word embeddings (GloVe) in zero-shot music annotation and retrieval scenarios. Subsequently, {cite}`won2021multimodal` extended this idea beyond just audio by including collaborative filtering embeddings, covering both acoustic and cultural aspects of music. {cite}`won2021multimodal`, {cite}`doh2024musical` addressed the non-music-domain-specific nature of word embeddings by training audio-text joint embeddings using music domain-specific word embeddings.

However, these models faced limitations in handling multiple attribute queries or complex sentence-level queries due to their reliance on word embeddings. This is because word embeddings are static - they do not encode different meanings based on surrounding context tokens. As a result, research using these models was constrained to tag-level retrieval scenarios.

## Audio-Multi Tag Joint Embedding

To better handle multiple attribute semantic queries, researchers have shifted their focus from word embeddings to bi-directional transformer encoders {cite}`devlin2018bert` {cite}`liu2019roberta`. They aimed to leverage **Contextualized Word Representations** that can encode different meanings of multiple attributes or sentences based on co-occurring words. {cite}`chen2022learning` and {cite}`doh2023toward` evaluated the language model's ability to understand multiple attribute queries by utilizing existing multilabel tagging datasets.

## Audio-Sentence Joint Embedding

```{figure} ./img/clap_mulan.png
---
name: Audio-Sentence Joint Embedding
---
```

To handle flexible natural language queries, researchers focused on noisy audio-text datasets {cite}`huang2022mulan` and human-generated natural language annotations beyond traditional annotation datasets {cite}`manco2022contrastive`. Thanks to sufficient dataset scaling and contrastive loss with the advantage of large batch sizes, they built joint embedding models with stronger audio-text associations compared to previous studies. {cite}`manco2022contrastive`, {cite}`huang2022mulan`, {cite}`wu2023large` demonstrated that contrastive learning with large-scale audio-text pairs could effectively learn semantic relationships between music and natural language descriptions.


## Beyond semntica attributes, toward handle similarity queries


```{figure} ./img/doh_enrich.png
---
name: Similarity Queries
---
```

Recent work has explored expanding joint embedding models beyond semantic attribute queries. While existing datasets focus on genre, mood, instruments, style, and theme attributes, {cite}`doh2024musical` proposed training joint embedding models that can handle similarity-based queries by leveraging diverse metadata and music knowledge graphs. This enables the model to understand relationships between songs based on metadata similarity rather than just semantic attributes, supporting more flexible music retrieval use cases.

## Design choices for audio-text joint embedding models


## Tips for training audio-text joint embedding models


## References

```{bibliography}
:filter: docname in docnames
```
