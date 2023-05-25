# 🎓𝗗𝗦𝗣: The Demonstrate–Search–Predict Framework

The **DSP** framework provides a programming abstraction for _rapidly building sophisticated AI systems_.
It's primarily (but not exclusively) designed for tasks that are knowledge intensive (e.g., answering user questions or researching complex topics).

You write a **DSP program** in a few lines of code, describing at high level how the problem you'd like to solve should be _decomposed_ into smaller _transformations_. 
This line was an overkill. Let me break this down. 

You have a problem that you would like to solve. You need to describe how the problem should be decomposed into smaller transformation.

**Here is my understanding at the first glance:**
You want to make a tea. You have no idea how to make one. But you know the incredients. So, you may tell saying, 
Prepare a tea by using milk, tea powder from the shelf 1 and a little bit of sugar. The system would indentify the steps by looking for the way to prepare based on attentions provided by tea powder and sugar. This is just my opinion at the moment. 

There are two models at work to solve the problem:
RM - Retreival model
LM - Language model

There is a concept called transformation as per the research paper where in the input problem is transformed into a different state by using a LM. 
Let us say, the input query is rephrased by the language model here. 

AND or OR -> What it means is the system determines whether to make an LM query or an RM query by taking decisions implicitly. 

What is the context mentioned here: 
The context could be the real piece of information that is required by a language model to answer the original query. 
The RM model takes in a query that resolves to some intermediate attributes that are required. 
The response from RM are appended as context for the later LM to parse and answer the original solution. 
In simple words, 
Query: What is the capital of India?
RM: Delhi is Indias capital. 
Query to LM: Indias capital is Delhi. What is the capital of India?
Output: Capital of India is Delhi based on the hints provided by RM. (Some exaggeration)

Transformations generate text (by invoking a language model; LM) and/or search for information (by invoking a retrieval model; RM) in high-level steps like `generate a search query to find missing information` or `answer this question using the supplied context`. 
[Advertisement] Our [research paper](https://arxiv.org/abs/2212.14024) shows that building NLP systems with **DSP** can easily outperform GPT-3.5 by up to 120%.

**DSP** programs invoke LMs in a declarative way: you focus on the _what_ (i.e., the algorithmic design of decomposing the problem) and delegate _how_ the transformations are mapped to LM (or RM) calls to the **DSP** runtime. In particular, **DSP** discourages "prompt engineering", which we view much the same way as hyperparameter tuning in traditional ML: a final and minor step that's best done _after_ building up an effective architecture (and which could be delegated to automatic tuning).

What is a declarative way mean?
There are two types of invoking a system: Imperative and declarative. 
As per POE: Invoking a language model (LM) in a declarative way means providing input to the model in a structured, concise, and declarative format that specifies the desired output, without explicitly specifying the steps or algorithms required to produce it. This is in contrast to an imperative approach, where the user specifies a sequence of steps to be executed by the LM in order to produce the desired output. In a declarative approach, the user typically specifies the inputs and outputs of the LM, along with any relevant constraints or preferences, using a high-level language or formalism. The LM then uses its internal algorithms and knowledge to generate the desired output, without requiring the user to specify the details of how this should be done.

"How" is delegated to LM and(or RM) calls.

What does it mean by DSP discourages "prompt engineering"?
As per DSP (Demonstrate Search, Predict): 

    Demonstrate: The user provides one or more example outputs that the LM should be able to generate.

    Search: The LM generates a large number of candidate outputs using a search algorithm.

    Predict: The LM selects the best candidate output from the search results based on its predicted probability of being correct.

POE: By using this approach, DSP avoids the need for explicit prompt engineering, as the user only needs to provide example outputs, rather than specific prompts or constraints. This allows the LM to learn to generate the desired outputs in a more flexible and generalizable way, without being overly constrained by specific prompt designs. However, it's worth noting that DSP is not always the best approach for every type of task or application. In some cases, prompt engineering may still be necessary or beneficial in order to achieve the desired results.



To this end, **DSP** offers a number of powerful _primitives_ for building architectures that compose transformations and offers corresponding implementations that map these transformations to effective LM and RM calls. For instance, **DSP** *annotates* few-shot demonstrations for the LM calls within your arbitrary pipeline automatically, and uses them to improve the quality of your transformations. Once you're happy with things, **DSP** can *compile* your program into a much cheaper version in which LM calls are transparently replaced with calls to a tiny LM created by the **DSP** runtime.


<p align="center">
  <img align="center" src="docs/images/DSP-tasks.png" width="460px" />
</p>
<p align="left">
  <b>Figure 1:</b> A comparison between three GPT3.5-based systems. The LM often makes false assertions, while the popular retrieve-then-read pipeline fails when simple search can’t find an answer. In contrast, a task-aware DSP program systematically decomposes the problem and produces a correct response. Texts edited for presentation.
</p>


## Installation

```pip install dsp-ml```

## 🏃 Getting Started

Our [intro notebook](intro.ipynb) provides examples of five "multi-hop" question answering programs of increasing complexity written in DSP.

You can **[open the intro notebook in Google Colab](https://colab.research.google.com/github/stanfordnlp/dsp/blob/main/intro.ipynb)**. You don't even need an API key to get started with it.

Once you go through the notebook, you'll be ready to create your own DSP pipelines!

<p align="center">
  <img align="center" src="docs/images/DSP-example.png" width="850px" />
</p>
<p align="left">
  <b>Figure 2:</b> A DSP program for multi-hop question answering, given an input question and a 2-shot training set. The Demonstrate stage programmatically annotates intermediate transformations on the training examples. Learning from the resulting demonstration, the Search stage decomposes the complex input question and retrieves supporting information over two hops. The Predict stage uses the retrieved passages to answer the question.
</p>


## ⚡️ DSP Compiler [NEW!]

Our [compiler notebook](compiler.ipynb) introduces the new experimental compiler, which can optimize DSP programs automatically for (much) cheaper execution.

You can **[open the compiler notebook in Google Colab](https://colab.research.google.com/github/stanfordnlp/dsp/blob/main/compiler.ipynb)**. You don't even need an API key to get started with it.

## Picking in-context examples using KNN/ANN methods [NEW!]

Our [knn demo notebook](tests/knn_demonstrations_test.ipynb) provides examples of adding the KNN stage, as described in the paper. This improvement in the Demonstrate stage of DSP allows you not to sample Examples randomly but instead search for better and similar options. You can get an idea from [this paper](https://arxiv.org/abs/2101.06804).

## 📜 Reading More

You can get an overview via our Twitter threads:
* [**Introducing DSP**](https://twitter.com/lateinteraction/status/1617953413576425472)  (Jan 24, 2023)
* [**Releasing the DSP Compiler (v0.1)**](https://twitter.com/lateinteraction/status/1625231662849073160)  (Feb 13, 2023)

And read more in the academic paper:
* [**Demonstrate-Search-Predict: Composing retrieval and language models for knowledge-intensive NLP**](https://arxiv.org/abs/2212.14024.pdf)

## ✍️ Reference

If you use DSP in a research paper, please cite our work as follows:

```
@article{khattab2022demonstrate,
  title={Demonstrate-Search-Predict: Composing Retrieval and Language Models for Knowledge-Intensive {NLP}},
  author={Khattab, Omar and Santhanam, Keshav and Li, Xiang Lisa and Hall, David and Liang, Percy and Potts, Christopher and Zaharia, Matei},
  journal={arXiv preprint arXiv:2212.14024},
  year={2022}
}
```
