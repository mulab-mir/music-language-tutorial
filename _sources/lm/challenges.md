# Challenges

Now, taking a step back, let's consider the challenges of language model research.

## Trust & Safety

**Hallucination** is one of the biggest blockers for employing large language models for mission-critical applications, because LLMs often make things up.
Recent models tend to be better at this, but the problem of hallucination is not fully understood or solved.

Being trained on huge data collected from the internet, language models and especially multimodal models often exhibit **bias**, which can be anything from political, racial, economical, sexual, cultural, religious, and other biases. 
There are many research and engineering efforts for reducing those biases and making language models **fair**er, but it still remains an open problem, just as it is for the human society.

There are many potential **malicious use of LLMs**, such as spam bots and automated misinformation campaigns, and more egregious uses like emotional control and deepfakes.
As LLM-based AI agents become more autonomous with the advances of **agentic AI** that we discussed, increased AI risks and the impact on the economy due to job automation are also open challenges at this time.
Providers of advanced LLMs are carefully monitoring their usage to detect and prevent these malicious uses and testing their future models for potentially increased risks.
There are also **national and international efforts** to address these risks, as exemplified by the U.S. AI Safety Institute and the AI Safety Summit.


## Performance & Efficiency

LLMs are less strong at problems that require **multi-step reasoning and planning**, where recent reasoning models have shown great promise.

As the language models become bigger, managing its **complexity and cost**, as well as the **energy consumption**, are also major challenges,
Other main challenges include the limited **context length** and **knowledge cutoff** of language models, which often necessitates use of RAG but with its own limitations.
Related to bias and fairness, supporting **multiple languages** is also challenging. Tokenizers are often less efficient on non-latin-alphabet-based languages, and the models often perform better on the same task in English than other languages.
Lastly, implementing **real-time multimodal interactivity** comes with unique challenges, where we need to find the right tradeoff between the model throughput, latency, and the fidelity of generated audio and images.

---

While these are daunting challenges, the field is making steady progress toward safer, more efficient, and equitable language models through ongoing research and innovation, and I am excited about it.


