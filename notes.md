# Onboarding Notes

These notes are primarily meant for undergraduate students at the University of Waterloo who are interested in getting involved in my research group.

## Current Focus (2026/11/10)

**What are we working on now?**

I am most excited about our recent Project Greenhouse work:

> Jimmy Lin, Sahel Sharifymoghaddam, Lingwei Gu, and Nour Jedidi. [Project Greenhouse: Progress Toward Fully Open and Sovereign Agentic Search.](https://arxiv.org/abs/2610.11922) arXiv:2610.11922, October 2026.

Project Greenhouse represents our exploration of a simple thesis: We believe that it is possible to build fully open and sovereign models for agentic search with only modest computational resources.

As a first milestone, we describe how to build a competitive pointwise decoder-only reranker using a simple two-step recipe comprising pre-training from scratch followed by supervised fine-tuning, starting only from commonly available datasets.
Contrary to the dominant approach in the literature, we do not rely on existing open-weight backbones from third parties, and thus we are fully in control of model training, from end to end.

To me, what's exciting is that we can pre-train models from scratch that achieve competitive task-specific effectiveness with limited compute resources.
More colloquially, a _meh_ pre-trained backbone can still yield a _good_ model with the right SFT recipe.
We've demonstrated this for reranking, and the hypothesis is that this carries over to other tasks as well.

## Current Focus (2026/08/31)

**What are we working on now?**

Our research group is always working on many different and exciting projects: for example, agentic search and disentangled evaluations are two priorities right now.
However, as a start (i.e., for Waterloo undergraduate students seeking to get more involved in our group), we are seeking help to improve [`rank_llm`](https://rankllm.ai/).

**How can I get involved?**

1. Read the [RankLLM paper](https://dl.acm.org/doi/10.1145/3726302.3730331).
2. Start with the [RankLLM onboarding path](https://github.com/castorini/rank_llm/blob/main/docs/install-colab-cli.md): this is our tactical entry point.
3. The onboarding path provides an introduction to the toolkit for pedagogical purposes (thus increasing the number of contributors), and will surface features that we should build out.

**Why is this important?**

Reranking is a core part of search, and in the context of transformers, our group has been working on it since 2019 (with BERT).
For example, see [this](https://arxiv.org/abs/1901.04085) and [this](https://arxiv.org/abs/1910.14424).

Today, there are many commercial offerings: see [this ChatGPT summary](https://chatgpt.com/share/6a95855b-585c-83ea-968d-4cfe5fa66af3).
RankLLM is our attempt to provide an open reranking toolkit for the community.

**What's in it for me?**

1. You get to learn about reranking.
2. You may be a co-author the next time we specifically write a paper about RankLLM.
3. Contributions to RankLLM provide an entry point to further engagement with the group. It gives me (and graduate students in the group) a chance to get to know you better. From there, you might get invited to join other research projects.
