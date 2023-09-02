# Onboarding Guide

**Undergraduates at the University of Waterloo**: if you want to work with me read [this guide](ura.md) first.

## 🧱 Foundations of Retrieval

This onboarding path provides the starting point of working in our group and comprises the following lessons:

1. Begin your journey [here](https://github.com/castorini/anserini/blob/master/docs/start-here.md). 
2. [BM25 Baselines for MS MARCO Passage Ranking **in Anserini**](https://github.com/castorini/anserini/blob/master/docs/experiments-msmarco-passage.md).
3. [BM25 Baseline for MS MARCO Passage Ranking **in Pyserini**](https://github.com/castorini/pyserini/blob/master/docs/experiments-msmarco-passage.md).
4. [A Conceptual Framework for Retrieval](https://github.com/castorini/pyserini/blob/master/docs/conceptual-framework.md)
5. [Contriever Baseline for NFCorpus](https://github.com/castorini/pyserini/blob/master/docs/experiments-nfcorpus.md)
6. [A Deeper Dive into Dense and Sparse Representations](https://github.com/castorini/pyserini/blob/master/docs/conceptual-framework2.md)

## Resources

This repository introduces several methods for users without local GPU resources.

+ [Transform Google Colab to a GPU instance with full SSH access](docs/colab-instructions.md)
+ [Guide to ComputeCanada GPU resources](docs/cc-guide.md)
+ [Guide to use UW GPU resources](docs/school-gpu.md)


## Training monoBERT from Scratch

[This](https://github.com/capreolus-ir/capreolus/blob/feature/msmarco_psg/docs/reproduction/MS_MARCO.md) 
is the guide to fine-tuning monoBERT on [MS MARCO Passage](https://github.com/microsoft/MSMARCO-Passage-Ranking) dataset,
based on [Capreolus](https://capreolus.ai/) toolkit.
For Compute Canada users, 
you may need to set up the environment following [this](https://github.com/capreolus-ir/capreolus/blob/feature/msmarco_psg/docs/setup/setup-cc.md) guide. 
