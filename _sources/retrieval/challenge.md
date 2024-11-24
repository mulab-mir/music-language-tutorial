# Challenges

While audio-text joint embedding models have enabled significant advances in retrieval systems, several open challenges still remain. In this chapter, we review three major issues:

1. Query-Caption Distribution Mismatch: There is often a significant gap between how users naturally formulate their queries and the captions used to train retrieval models. Training datasets typically contain formal descriptions or technical annotations, while real user queries tend to be more colloquial and diverse in their expression. 

2. Single-Turn Retrieval Limitations: Current retrieval systems mostly operate in a single-turn fashion, where each query is processed independently. This fails to capture the natural back-and-forth nature of music discovery, where users often refine their searches based on previous results and may want to explore related but different musical directions.

## Query-Caption Distribution Mismatch

```{figure} ./img/limit_caption.png
---
name: Limitation of Current Caption Dataset
---
```

Current caption datasets unfortunately focus almost exclusively on musical attributes, lacking coverage of broader cultural aspects of music. For example, the [Song Describer dataset](https://github.com/mulab-mir/song-describer-dataset) presented in {cite}`manco2023song` is limited to technical musical attributes such as genre, style, mood, texture, instruments, and structure. While these attributes are important, they miss many other dimensions that users care about - like cultural historical significance, artist background, or similarity to other artists and songs. The dataset also tends to use formal, analytical language rather than the more natural, colloquial ways that people typically describe and search for music. This narrow focus leaves significant gaps in covering the full spectrum of users' musical needs and search behaviors. Without training data that captures these broader aspects of how people relate to and discover music, retrieval systems may struggle to effectively serve real-world use cases where users want to find music based on more than just its acoustic characteristics.


```{figure} ./img/limit_jinha.png
---
name: Query Understanding
---
```

We need to revisit previous research on user query understanding. In {cite}`lee2010analysis`, analysis of user query needs by topic revealed a much broader range of musical requirements than what current systems typically handle. For example, information about lyrics, scores, publishers, and dates are notably absent from current music caption datasets. More recently, {cite}`doh2024music` conducted a qualitative analysis of 917 music-related conversations, finding that users are interested in searching music not just based on musical content information, but also through queries about users, metadata, and other diverse aspects.


```{figure} ./img/limit_doh.png
---
name: User Query Analysis
---
```

These studies highlight the significant gap between how current retrieval systems are trained and actual user needs. While modern caption datasets focus primarily on musical content descriptions, real users seek music through a much richer variety of queries encompassing cultural, contextual, and metadata-based information. This mismatch limits the effectiveness of current retrieval systems in serving natural user queries.


## Single-Turn Retrieval Limitations

Current music retrieval systems are predominantly designed for single-turn interactions, where each query is treated as an independent event. However, this approach fails to capture the inherently iterative and conversational nature of music discovery. When users search for music, they often need multiple attempts to find what they're looking for, refining their queries based on previous results and system responses.

Consider a typical scenario: a user makes an initial query but isn't fully satisfied with the results. In a natural discovery process, they would build upon this first interaction - perhaps specifying additional criteria, requesting variations on the initial results, or steering the search in a slightly different direction. Current single-turn systems, however, treat each new query as a completely fresh start, discarding valuable context from previous interactions.

This limitation creates several key problems:

1. **Loss of Search Context**: Each new query starts from scratch, ignoring the valuable information contained in the user's search history and previous interactions. This forces users to repeatedly provide context that could have been inferred from their search trajectory.

2. **Inability to Learn from Feedback**: The system cannot effectively learn from the user's implicit or explicit feedback about previous results to improve subsequent recommendations. When a user modifies their query, the system doesn't understand which aspects of the previous results were unsatisfactory.

3. **Limited Refinement Capabilities**: Users cannot naturally refine their searches through iterative interactions. Instead of being able to say "like that, but more upbeat" or "similar but with different instruments," they must formulate entirely new queries.

4. **Missed Opportunities for Exploration**: The system cannot guide users through a natural exploration of the music space, suggesting related but different directions based on their evolving preferences and reactions to previous results.

To address these limitations, future retrieval systems need to incorporate multi-turn capabilities that can:
- Maintain conversation history and context across multiple interactions
- Understand when initial results don't fully satisfy the user's intent
- Use previous interactions to inform and improve subsequent recommendations
- Support natural query refinement and exploration patterns

This evolution towards conversational music retrieval would better align with how people naturally discover and explore music, leading to more satisfying and effective search experiences.


## References

```{bibliography}
:filter: docname in docnames
```
