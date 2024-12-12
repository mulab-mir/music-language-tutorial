(description_evaluation)=
# Evaluation

Reliably evaluating music description systems is a challenging endeavour. Even when we have "grounth-truth" captions, it is not always clear how to score generated text, as music description is open-ended, and at least partially subjective. The quality of a description is also strongly dependent on the context in which it is used. This issue gets even more pronounced with more dialogue-based tasks like MQA or other forms of instruction-based description.
Comparing outputs to gold standard from static datasets can help, but it's only the first step.

Overall, current strategies are usually a mix of automatic evaluation via reference-based metrics and human evaluation, alongside newer protocols designed to assess language generation. Let's review automatic evaluation in more detail below.

## Match-based metrics
A common approach for automatic evaluation of captioning systems relies on metrics that score the syntactic or semantic similarity between the generated caption and a set of reference captions. Borrowed from the computer vision and NLP literature, these metrics were originally created to evaluate text output in machine translation (BLEU, METEOR, ROUGE), image captioning (CIDEr, SPICE, SPIDEr) or other text generation tasks (BERT-Score). As we will see, their use in evaluating music captioning isn't without issues, but they continue to be used in objective evaluation as they provide a convenient way to compare generated text to reference captions.

We briefly review each of these metrics below:

* **BLEU** measures the similarity between candidate and ground-truth captions by taking the geometric mean of the $n$-gram-based precision $P_n$, using either only unigrams (BLEU_1), or combinations of n-grams up to 4 (BLEU\textsubscript{4}). The final computation of the score also includes a brevity penalty term to discount, at an aggregate level, candidate text that does not match the reference text in length.

* **METEOR** also measures the similarity between generated and reference captions, but does so by combining an $F$-score -- the harmonic mean of unigram precision $P_1$ and recall $R_1$ -- with a penalty term which reduces the score based on the number of longer $n$-grams matched between candidate and reference text.

* **ROUGE**, commonly used in text summarisation, is similarly based on an F-score computed from matches of overlapping units between generated and ground-truth text. Depending on the length of the units considered, this metric comes in different variants. Among them, we adopt ROUGE\textsubscript{L}, which considers matching between the longest common subsequence instead of using predefined $n$-grams lengths.

* **CIDEr** is instead designed to capture human consensus by measuring how well a candidate caption matches the majority of reference captions. This is done by adding the average cosine similarity between tf-idf weighted $n$-grams of candidate and reference text for different values of $n$ (typically $n=\{1, 2, 3, 4\}$), leading to better correlation with human judgement in image captioning, compared to match-based metrics. 
    
* **SPICE** is based on a comparison of semantic similarity of scene graphs, representations of objects and their relationship parsed from the candidate and reference caption. These abstract away much of the lexical and syntactic characteristics of the captions, focusing instead on semantically meaningful components of the text. 

* **SPIDEr** is a linear combination of SPICE and CIDEr.

* **BERT-Score** also computes the similarity between tokens in a generated sentence and tokens in the ground-truth text, but does so using contextual embeddings obtained from a pre-trained BERT model instead of exact matches, resulting in a higher correlation with human judgements.

## Other types of automatic evaluation
* Multiple-choice question answering: MuChoMusic {cite}`weck_muchomusic_2024`
* Other benchmarks: OpenMU {cite}`zhao_openmu_2024`
* LLM-as-a-judge
* Non audio: {cite}`li_music_2024`

## References

```{bibliography}
:filter: docname in docnames
```
