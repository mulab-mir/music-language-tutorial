# Models

Music retrieval systems have evolved from simple classification approaches to more sophisticated methods that can handle natural language queries. Audio-text joint embedding represents a significant advancement in this field, offering a powerful framework for matching music with textual descriptions. This approach, built on multimodal deep metric learning, enables users to search for music using flexible natural language descriptions rather than being constrained to predefined categories. By projecting both audio content and textual descriptions into a shared embedding space, the system can efficiently compute similarities between music and text, making it possible to retrieve music that best matches arbitrary text queries.

```{figure} ./img/cls_to_je.png
---
name: classification to joint embedding
---
```

## Multi-modal Joint Embedding Model Architecture

At a high level, a joint embedding model is trained using paired text and music audio samples. The model learns to map semantically related pairs close together in a shared embedding space while simultaneously pushing unrelated samples further apart. This creates a meaningful geometric arrangement where the distance between embeddings reflects their semantic similarity.

Let $x_{a}$ represent a musical audio sample and $x_{t}$ denote its paired text description. The functions $f(\cdot)$ and $g(\cdot)$ represent the audio and text encoders respectively, which are typically implemented as deep neural networks. The audio encoder processes raw audio waveforms or spectrograms to extract relevant acoustic features, while the text encoder converts textual descriptions into dense vector representations. The output feature embeddings from each encoder are then mapped to a shared co-embedding space through projection layers that align the dimensionality and scale of the embeddings. During training, the model typically employs either `triplet loss` based on hinge margins or `contrastive loss` based on cross entropy to learn these mappings. These loss functions encourage the model to minimize the distance between positive pairs while maximizing the distance between negative pairs, effectively shaping the structure of the embedding space.

## Metric Learning Loss Functions

The most common metric learning loss functions used to train joint embedding models are triplet loss and contrastive loss.

```{figure} ./img/loss_function.png
---
name: loss functions
---
```

The goal of triplet-loss models is to learn an embedding space where semantically relevant input pairs are mapped closer together than irrelevant pairs in the latent space. For each training sample, the model takes an anchor audio input, a positive text description that matches the audio, and a negative text description that is unrelated. The objective function is formulated as follows:

$$
\mathcal{L}_{triplet}= \text{max}(0, - f(x_{a}) \cdot g(x_{t}^{+}) + f(x_{a}) \cdot g(x_{t}^{-}) + \delta )
$$

where $\delta$ is the margin hyperparameter that controls the minimum distance between positive and negative pairs, $f(x_{a})$ is the audio embedding of the anchor sample, $g(x_{t}^{+})$ is the text embedding of the positive description, and $g(x_{t}^{-})$ is the text embedding of the negative description. The dot product measures similarity between the embeddings. The loss function encourages the similarity between the anchor and positive pair to be greater than the similarity between the anchor and negative pair by at least the margin $\delta$.

The core idea of contrastive-loss models is to reduce the distance between positive sample pairs while increasing the distance between negative sample pairs across an entire mini-batch of samples. Unlike triplet-loss which only uses one negative sample per anchor, contrastive-loss models leverage multiple negative samples that exist within a mini batch of size $N$. During training, the audio and text encoders are jointly optimized to maximize the similarity between $N$ positive pairs of (music, text) associations while minimizing the similarity for $N \times (N-1)$ negative pairs. This approach, known as the multi-modal version of InfoNCE loss {cite}`oord2018representation`, {cite}`radford2021learning`, allows for more efficient training by considering many negative examples simultaneously. The loss is formulated as follows:

$$
\mathcal{L}_\text{Contrastive} = - \frac{1}{N} \sum_{i=1}^N \log \frac{\exp(f(x_{a_i}) \cdot g(x_{t_i}^{+}) / \tau)}{\sum_{j=1}^N \exp(f(x_{a_i}) \cdot g(x_{t_j}) / \tau)}
$$
where $\tau$ is a learnable parameter.

## What is the Benefit of Joint Embedding?

```{figure} ./img/benefit.png
---
name: joint embedding benefit
---
```

The key advantage of joint embedding is that it allows us to leverage the embedding space of pretrained language models as supervision, rather than being limited to a fixed vocabulary. Pretrained language models, which are trained on vast text corpora from the internet, effectively encode rich semantic relationships between words and phrases. This enables music retrieval systems to handle zero-shot user queries efficiently by utilizing these comprehensive language representations that capture nuanced meanings and associations.

Furthermore, by incorporating language model encoders, we can effectively address the out-of-vocabulary problem through advanced subword tokenization techniques such as byte-pair encoding (BPE) or sentence-piece encoding. These tokenization methods intelligently break down unknown words into smaller subword units that already exist in the model's vocabulary. For example, if a user queries an unfamiliar artist name "Radiohead", the tokenizer might break it down into "radio" + "head", allowing the system to process and understand this novel term based on its constituent parts.

This powerful combination of pretrained language model semantics and subword tokenization provides two crucial benefits:

1. Flexible handling of open vocabulary queries through language model representations - The system can understand and process a virtually unlimited range of natural language descriptions by leveraging the rich semantic knowledge encoded in pretrained language models
2. Robust processing of out-of-vocabulary words through subword tokenization - Even completely new or rare terms can be effectively handled by breaking them down into meaningful subword components, ensuring the system remains functional and accurate when encountering novel vocabulary

## Models

In this section, we comprehensively review recent advances in audio-text joint embedding models for music retrieval. We examine how these models have evolved from simple word embedding approaches to more deep architectures leveraging pretrain language models and contrastive learning.  Additionally, we discuss practical design choices and implementation tips that can help researchers and practitioners build robust audio-text joint embedding systems. 

### Audio-Tag Joint Embedding

```{figure} ./img/choi_zeroshot.png
---
name: Audio-Tag Joint Embedding
---
```

The first significant audio-text joint embedding work introduced to the ISMIR community was presented by {cite}`choi2019zero`. Their research demonstrated the effectiveness of pretrained word embeddings, specifically GloVe (Global Vectors for Word Representation), in enabling zero-shot music annotation and retrieval scenarios. This approach allowed the model to understand and process musical attributes it had never seen during training.

Building on this foundation, {cite}`won2021multimodal` expanded the scope beyond pure audio analysis by incorporating collaborative filtering embeddings. This innovation allowed the model to capture both acoustic properties (how the music sounds) and cultural aspects (how people interact with and perceive the music) simultaneously. Further advancements came from {cite}`won2021multimodal` and {cite}`doh2024musical`, who tackled a critical limitation of general word embeddings - their lack of music-specific context. They developed specialized audio-text joint embeddings trained specifically on music domain vocabulary and concepts, leading to more accurate music-related representations.

However, these early models encountered significant limitations when handling more complex queries. Specifically, they struggled with multiple attribute queries (e.g., "happy, energetic, rock") or complex sentence-level queries (e.g., "a song that reminds me of a sunny day at the beach"). This limitation stemmed from their reliance on static word embeddings, which assign fixed vector representations to words regardless of context. Unlike more advanced language models, these embeddings cannot capture different meanings of words based on surrounding context tokens. Consequently, research utilizing these models was primarily restricted to simple tag-level retrieval scenarios, where queries consisted of single words or simple phrases.

### Audio-Sentence Joint Embedding

To better handle multiple attribute semantic queries, researchers have shifted their focus from word embeddings to bi-directional transformer encoders {cite}`devlin2018bert` {cite}`liu2019roberta`. These transformer models offer several key advantages over static word embeddings:


```{figure} ./img/contextualize_word.png
---
name: Contextualized Word Representation
---
```

1. **Contextualized Word Representations**: Unlike static word embeddings that assign a fixed vector to each word, transformers generate dynamic representations based on the surrounding context. For example, the word "heavy" would have different representations in "heavy metal music" versus "heavy bass line".

2. **Attention Mechanism**: The self-attention layers in transformers allow the model to weigh the importance of different words in relation to each other. This is particularly useful for understanding how multiple attributes interact - for instance, distinguishing between "soft rock" and "hard rock".

3. **Bidirectional Context**: Unlike autoregressive transformers (e.g., GPT) that use causal masking to only attend to previous tokens, BERT and RoBERTa use bidirectional self-attention without masking. This allows each token to attend to both previous and future tokens in the sequence. For example, when processing "upbeat jazz with smooth saxophone solos", the word "smooth" can attend to both "upbeat jazz" (previous context) and "saxophone solos" (future context) simultaneously, enabling richer contextual understanding. This bidirectional attention is particularly important for music queries where the meaning of descriptive terms often depends on both preceding and following context.

```{figure} ./img/clap_mulan.png
---
name: Audio-Sentence Joint Embedding
---
```

Recent works have demonstrated the effectiveness of these transformer-based approaches. {cite}`chen2022learning` evaluated BERT langauge model emedding with the MagnaTagATune dataset, showing improved accuracy in handling queries with multiple genre and instrument tags. Similarly, {cite}`doh2023toward` utilized BERT to process complex attribute combinations on the [MSD-ECALS dataset](https://zenodo.org/records/7107130), demonstrating better understanding of musical descriptions like "electronic, female vocal elements" or "ambient, piano".

These studies leveraged existing multilabel tagging datasets to train and evaluate the language models' ability to understand multiple attribute queries. The results showed that transformer-based models could not only handle individual attributes but also effectively capture the relationships and dependencies between multiple musical characteristics in a query.

To handle flexible natural language queries, researchers focused on noisy audio-text datasets {cite}`huang2022mulan` and human-generated natural language annotations {cite}`manco2022contrastive`. Thanks to sufficient dataset scaling and contrastive loss with the advantage of large batch sizes, they built joint embedding models with stronger audio-text associations compared to previous studies. {cite}`manco2022contrastive`, {cite}`huang2022mulan`, {cite}`wu2023large` demonstrated that contrastive learning with large-scale audio-text pairs could effectively learn semantic relationships between music and natural language descriptions.

```{figure} ./img/benefit_lm.png
---
name: Benefit of Language Model Text Encoders
---
```

In practice, pre-trained language model text encoders have not shown significant performance improvements in multi-tag query retrieval {cite}`doh2023toward` and language query retrieval {cite}`manco2022contrastive` scenarios. 


### Beyond semntica attributes, toward handle similarity queries


```{figure} ./img/similarity_query.png
---
name: Similarity Queries
---
```

Recent work has explored expanding joint embedding models beyond semantic attribute queries to handle similarity-based search scenarios. While existing datasets and models primarily focus on semantic attributes like genre, mood, instruments, style, and themes, {cite}`doh2024musical` proposed a novel approach that enables similarity-based queries by leveraging rich metadata and music knowledge graphs.

Specifically, they constructed a large-scale music knowledge graph that captures various relationships between songs, such as artist similarity (songs by artists with similar styles), metadata connections (songs by the same artist), and track-attribute relationships. By training joint embedding models on this knowledge graph, the model learns to understand complex relationships between songs and other musical entities. This allows the system to handle queries like "songs similar to Hotel California" or "music that sounds like early Beatles" by embedding both the query and music in a shared semantic space that captures these rich relationships. The approach significantly expands the capabilities of music retrieval systems, enabling more natural and flexible search experiences that better match how humans think about and relate different pieces of music. 

### Tips for Training Audio-Text Joint Embedding Models

Recent research has identified several key strategies for effectively training audio-text joint embedding models that have significantly improved model performance and robustness. One major challenge is query format diversity - models must understand and process queries ranging from simple tags (e.g., "rock", "energetic") to complex natural language descriptions (e.g., "a melancholic piano piece with subtle jazz influences"). Additionally, models must bridge the semantic gap between audio features and semantic concepts expressed in text descriptions. Researchers have developed several effective strategies to address these fundamental challenges, which are detailed below.

#### Leverage Diverse Training Data Sources

- Utilize noisy web-scale text data {cite}`huang2022mulan,weck2024wikimute` to expose models to real-world music descriptions
- Combine multiple annotation datasets {cite}`wu2023large,doh2023toward,manco2024augment` to capture various aspects of music-text relationships
- This multi-source approach helps models develop robust representations across different description styles and musical concepts

#### Initialize with Pre-trained Models

- Begin with pre-trained text encoders (e.g., BERT, RoBERTa) and audio encoders (e.g., MERT, HT-AST) {cite}`wu2023large,manco2024augment`
- This transfer learning approach leverages general language/audio understanding while adapting to music-specific requirements

#### Apply Text Augmentation Techniques

- Implement text dropout corruption {cite}`doh2023toward,manco2024augment` to enhance model robustness
- Utilize LLM-generated tag-to-caption data {cite}`wu2023large,doh2023toward` to expand training data
- These techniques help models handle various query phrasings and improve generalization capabilities

#### Employ Strategic Negative Sampling

- Implement hard negative sampling strategies {cite}`won2021multimodal,manco2024augment` to improve discrimination ability
- Carefully select challenging negative examples that are semantically similar but distinct
- This approach helps models develop the ability to make fine-grained distinctions between similar musical concepts


```{figure} ./img/tips.png
---
name: Tips for Training Audio-Text Joint Embedding Models
---
```
In {cite}`manco2024augment`, these ideas were implemented to significantly improve the retrieval performance of audio-text joint embedding models. The experimental results showed dramatic improvements, with the R@10 metric increasing from 9.6 to 15.8 when applying pre-trained encoders, tag-to-caption augmentation, dropout, and hard negative sampling (text swap) techniques on top of the baseline model {cite}`doh2023toward`. This substantial performance gain demonstrates the effectiveness of combining these training strategies.

## References

```{bibliography}
:filter: docname in docnames
```
