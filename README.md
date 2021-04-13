# Onboarding Guide

This repository introduces several methods for users without local GPU resources.

+ [Transform Google Colab to a GPU instance with full SSH access](docs/colab-instructions.md)
+ [Guide to ComputeCanada GPU resources](docs/cc-guide.md)
+ [Guide to use UW GPU resources](docs/school-gpu.md)

## Reading List
Here's a book that provides an overview of text ranking with neural network architectures:

+ Jimmy Lin, Rodrigo Nogueira, Andrew Yates. [Pretrained Transformers for Text Ranking: BERT and Beyond.](https://arxiv.org/abs/2010.06467) _arXiv:2010.06467_, October 2020.

It's long, so take your time to digest it.
For a succinct foundation in neural information retrieval research, we strongly recommend the following sections:

- 1.1 Text Ranking Problems
- 2.0 Setting the Stage
- 3.1 High-Level Overview of BERT
- 3.2 monoBERT

They provide the fundamentals of our group's research.

## State of the Group in Information Access

Here's a guide to the group's capabilities in information access (search, question answering, etc.).

If you're interested in working on text ranking (e.g., search), start with

+ Edwin Zhang, Nikhil Gupta, Raphael Tang, Xiao Han, Ronak Pradeep, Kuang Lu, Yue Zhang, Rodrigo Nogueira, Kyunghyun Cho, Hui Fang, and Jimmy Lin. [Covidex: Neural Ranking Models and Keyword Search Infrastructure for the COVID-19 Open Research Dataset.](https://arxiv.org/abs/2007.07846) _arXiv:2006.12975_, July 2020. (Describes expando-mono-duoT5.)
+ Rodrigo Nogueira, Zhiying Jiang, and Jimmy Lin. [Document Ranking with a Pretrained Sequence-to-Sequence Model.](https://arxiv.org/abs/2003.06713) _arXiv:2003.06713_, March 2020. (Describes monoT5.)
+ Rodrigo Nogueira, Wei Yang, Kyunghyun Cho, and Jimmy Lin. [Multi-Stage Document Ranking with BERT.](https://arxiv.org/abs/1910.14424) arXiv:1910.14424, October 2019. (Describes monoBERT and duoBERT.)
+ Rodrigo Nogueira, Wei Yang, Jimmy Lin, and Kyunghyun Cho. [Document Expansion by Query Prediction.](https://arxiv.org/abs/1904.08375) _arXiv:1904.08375_, April 2019. (Describes the basic doc2query setup.)
+ Rodrigo Nogueira and Jimmy Lin. [From doc2query to docTTTTTquery.](https://cs.uwaterloo.ca/~jimmylin/publications/Nogueira_Lin_2019_docTTTTTquery-v2.pdf) December 2019. (Describes doc2query with T5.)

If you're interested in working on question answering, start with

+ Wei Yang, Yuqing Xie, Aileen Lin, Xingyu Li, Luchen Tan, Kun Xiong, Ming Li, and Jimmy Lin. [End-to-End Open-Domain Question Answering with BERTserini.](https://www.aclweb.org/anthology/N19-4013/) Proceedings of the 2019 Conference of the North American Chapter of the Association for Computational Linguistics (Demonstrations) (NAACL 2019), pages 72-77, June 2019, Minneapolis, Minnesota.

## Task: Training monoBERT from Scratch
[This](https://github.com/capreolus-ir/capreolus/blob/feature/msmarco_psg/docs/reproduction/MS_MARCO.md) 
is the guide to fine-tuning monoBERT on [MS MARCO Passage](https://github.com/microsoft/MSMARCO-Passage-Ranking) dataset,
based on [Capreolus](https://capreolus.ai/) toolkit.
For Compute Canada users, 
you may need to set up the environment following [this](https://github.com/capreolus-ir/capreolus/blob/feature/msmarco_psg/docs/setup/setup-cc.md) guide. 

## The TODO list

Here's a bunch of tasks our group is interested in tackling:

For passage and document ranking,

+ Bajaj, Payal, et al. [Ms marco: A human generated machine reading comprehension dataset.](https://microsoft.github.io/msmarco/) arXiv preprint arXiv:1611.09268 (2016). 

For fact verification,

+ Thorne, James, et al. [FEVER: a large-scale dataset for fact extraction and verification.](https://arxiv.org/abs/1803.05355) arXiv preprint arXiv:1803.05355 (2018).
+ Wadden, David, et al. [Fact or Fiction: Verifying Scientific Claims.](https://arxiv.org/abs/2004.14974) arXiv preprint arXiv:2004.14974 (2020).

For knowledge-intensive language tasks in general,

+ Petroni, Fabio, et al. [KILT: a Benchmark for Knowledge Intensive Language Tasks.](https://arxiv.org/abs/2009.02252) arXiv preprint arXiv:2009.02252 (2020).
