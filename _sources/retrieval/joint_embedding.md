# Audio-Text Joint Embedding

## Classification to Joint Embedding

Following the classification framework, the audio-text joint embedding methodology emerged as a way to handle more flexible user queries. Audio-text joint embedding, as a multimodal deep metric learning model, enables music search beyond fixed vocabularies by leveraging language embeddings from pretrained language models. In this approach, we project audio content and its associated text into a space where dot product operations are possible. 

## Model Architecture

```{figure} ./img/cls_to_je.png
---
name: classification to joint embedding
---
```

At a high level, a joint embedding model is trained with paired text and music audio samples, learning to map related pairs close together in the embedding space while pushing unrelated samples further apart.

Let $x_{a}$ represent a musical audio sample and $x_{t}$ denote its paired text description. The functions $f(\cdot)$ and $g(\cdot)$ represent the audio and text encoders respectively. The output feature embeddings from each encoder are mapped to a shared co-embedding space through projection layers. During training, the model typically employs either triplet loss based on hinge margins or contrastive loss based on cross entropy to learn these mappings.

## Loss Functions

The most common metric learning loss functions used to train joint embedding models are triplet loss and contrastive loss.

```{figure} ./img/loss_functions.png
---
name: loss functions
---
```

The goal of triplet-loss models is to learn an embedding space where relevant input pairs are mapped closer than irrelevant pairs in the latent space. The objective function is formulated as follows:

$$
\mathcal{L}_{triplet}= \text{max}(0, - f(x_{a}) \cdot g(x_{t}^{+}) + f(x_{a}) \cdot g(x_{t}^{-}) + \delta )
$$
where $\delta$ is the margin, $f(x_{a})$ is the audio embedding, $g(x_{t}^{+})$ is the paired text embedding for the music audio, and $g(x_{t}^{-})$ is the irrelevant text embedding.

The core idea of contrastive-loss models is to reduce the distance between positive sample pairs while increasing the distance between negative sample pairs. Unlike triplet-loss models, contrastive-loss models can utilize a large number of negative samples that exist in a mini batch $N$. During training, the audio and text encoders are jointly trained to maximize the similarity between $N$ positive pairs of (music, text) associations while minimizing the similarity for $N \times (N-1)$ negative pairs. This is known as the multi-modal version of InfoNCE loss {cite}`oord2018representation`, {cite}`radford2021learning` and formulated as follows:

$$
\mathcal{L}_\text{Contrastive} = - \frac{1}{N} \sum_{i=1}^N \log \frac{\exp(f(x_{a_i}) \cdot g(x_{t_i}^{+}) / \tau)}{\sum_{j=1}^N \exp(f(x_{a_i}) \cdot g(x_{t_j}) / \tau)}
$$
where $\tau$ is a learnable parameter.

## What is the Benefit of Joint Embedding?

```{figure} ./img/joint_embedding_benefit.png
---
name: joint embedding benefit
---
```
The key advantage of joint embedding is that we can leverage the embedding space of pretrained language models as supervision, rather than being limited to a fixed vocabulary. Since pretrained language models are trained on vast text corpora from the internet, they effectively encode language relationships between words and phrases. In music retrieval scenarios, this allows us to handle zero-shot user queries efficiently by utilizing these rich language representations.

Additionally, by using language model encoders, we can address the out-of-vocabulary problem through subword tokenization techniques like byte-pair encoding (BPE) or sentence-piece encoding. These methods break down unknown words into smaller subword units that exist in the model's vocabulary, enabling the system to handle novel queries.

This combination of pretrained language model semantics and subword tokenization provides two key benefits:
1. Flexible handling of open vocabulary queries through language model representations
2. Robust processing of out-of-vocabulary words through subword tokenization


## References

```{bibliography}
:filter: docname in docnames
```
