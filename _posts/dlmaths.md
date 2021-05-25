---
title: 'Solving Differential Equations with Transformers: Deep Learning for Symbolic Mathematics'
date: 2020-01-21
permalink: /posts/2020/01/dlmaths/
tags:
  - maths
  - deep learning
  - machine learning
  - transformers
---
![Deep Learning for Symbolic Mathematics](https://chinglamchoi.github.io/cchoi/files/dl.png)

Article published in Analytics Vidhya: [https://medium.com/analytics-vidhya/solving-differential-equations-with-transformers-21648d3a1695](https://medium.com/analytics-vidhya/solving-differential-equations-with-transformers-21648d3a1695)  
In this article, I will cover a new Neural Network approach to solving 1st and 2nd order Ordinary Differential Equations, introduced in Guillaume Lample and François Charton Facebook AI Research)'s ICLR 2020 spotlight paper, "Deep Learning for Symbolic Mathematics". This paper tackles symbolic computation tasks of integration and solving 1st & 2nd order ODEs with a seq2seq Transformer, we will focus on the latter today.
  
To give context to this paper, although Neural Network methods have seen great success in clearly structured statistical pattern recognition tasks -- e.g. object detection (Computer Vision), speech recognition, semantic analysis (Natural Language Processing) -- symbolic reasoning is not one of its strengths.
  
Not only does Symbolic Computation require AI to infer complex mathematical rules, they also require a flexible, contextual understanding of abstract mathematical symbols in relation to each other. At the time of authoring, Computer Algebra Systems (CAS) (such as Matlab, Mathematica) held state-of-the-art performance on symbolic mathematics tasks, driven by a backend of complex algorithms such as the 100-page long Risch algorithm for indefinite integration.
  
However, these semi-algorithms are far from perfect: failing in specific cases and sometimes timing out indefinitely. In their paper, Lample and Charton develop a seq2seq approach in the attempt to outperform Computer Algebra Systems. Their contributions can be summarised as follows:  
1. Demonstrating the potential of seq2seq transformers in 3 symbolic mathematics tasks and by extension in symbolic reasoning  
2. Achieving state-of-the-art performance in these tasks (with non-trivial dataset & inference time constraints)  
3. Introducing a novel approach to generate arbitrarily large datasets of expressions & corresponding solutions, where each expression is uniformly sampled from the specified problem space.  
  
Before performing inference, extensive pre-processing is done in areas of task structuring & definition, and dataset generation. Lample and Charton make 2 insightful observations -- firstly, that specific cases in function integration can be significantly simplified by pattern recognition; secondly, that formal mathematics can be described using natural language prefix syntax (also in previous research). The authors first parse mathematical expressions into tree structures, subsequently represent trees as sequences, then investigate the size of their problem space, and finally propose methods for dataset generation. Although the authors' methods of investigating the problem space and of data generation are extremely interesting, for the purposes of this article, I will focus on sections "Expressions as Trees", "Trees as Sequences" and "Experiments". I will write a follow-up article on the whole paper soon!

Expressions as Trees
======
Tree data structures are inherently hierarchical and can reflect important features of mathematical expressions. By considering operators and functions as internal nodes of trees; operands as nodes; constants and variables as leaves, the authors achieve 3 things: encode order of operations information, describe associativity of operators, simplification by eliminating the need for parentheses. To illustrate this method, let's parse the expression $a^{2}+2ab+b^{2}:
![tree](https://chinglamchoi.github.io/cchoi/files/tree.png)  
Following the convention in the paper, the tree is associative to the right. In order to guarantee a one-to-one mapping between expressions and trees (particularly that different expressions correspond to different trees, even if they are identical in evaluation), constraints (e.g. each parent as at most 2 children, no unary operators may be used) are imposed).

Trees as Sequences
======
The authors elect to use seq2seq models instead of tree-to-tree networks to avoid the run-time overhead and to avoid having to choose between different grammar syntax (e.g. Penn Treebank, Head Driven Phrase Structure Grammar). To translate trees into sequences, the authors use prefix notation to model expressions as a natural language. For instance, $2(a+b)^2$ would become * 2 pow + a b 2].
  
Model
======
One of the notable features of the method in this paper is that a very simple, vanilla seq2seq transformer model is used. Introduced by Vaswani et al. in 2017, this model has 8 attention heads, 6 layers and produces outputs of 512 dimensions. The model leverages the encoder-decoder structure, with the addition of stacked self-attention and point-wise fully-connected layers.
![transformer](https://chinglamchoi.github.io/cchoi/files/transformer.png)  
As described in the original paper, Attention mechanisms improve conventional sequence models in 2 ways: it better encodes long-distance dependencies in language data; escapes from having to encode sentence input to a fix length feature vector, allowing the decoder to selectively attend to relevant input words. This Transformer model extends the original attention model by completely replacing the recurrent model and convolutions with self-attention mechanisms. Via the Multi-Head Attention mechanism, this 2017 model facilitates acceleration via parallelism. The Scaled Dot-Product Attention, defined as:
![attention](https://chinglamchoi.github.io/cchoi/files/attention.png)
where $Q$ is a matrix of input queries, $K$ and $V$ are matrices of input keys and values respectively. $\sigma$ applies the softmax function on the dot product of query and keys scaled by $1/sqrt(d_k)$ to obtain the weights for attention. By using the dot product operation, and using 8 attention head in parallel, the Transformer model becomes much more computationally efficient than its recurrent model counterparts. This benefit is evidenced in differential equation solving experiments of this paper, whereby using Transformers, Lample and Charton are able to compute solutions with higher accuracy than CAS under 30 seconds, outperforming Mathematica which times out indefinitely in 20% of these test cases.
  
Experiment & Evaluation
======
The seq2seq models for solving 1st & 2nd order ODEs are trained on training sets with 40M expressions each. As described in the paper, the authors consider expressions which satisfy the following:  
![conditions](https://chinglamchoi.github.io/cchoi/files/conditions.png)  
The size of problem space can be calculated by applying  
![problem space](https://chinglamchoi.github.io/cchoi/files/problem_space.png)  
Other implementation details include using the Adam optimiser with a learning rate of 1e-04, removing expressions that exceed 512 tokens, and training with a batch size of 256 expressions. Inference results are generated by beam search and the log-likelihood loss is minimised. SymPy is used to compare if inferred results are equivalent to the reference answer.
  
Evaluation & Results  
======
Evaluation is performed on a testset of 5000 equations, with the following results:  
![Table 1](https://chinglamchoi.github.io/cchoi/files/table1.png)
Authors focus on 2 comparisons: comparing results with that of Mathematica (limited to 30s of computation time), Matlab and Maple, comparing results using different beam size. Results for the former are computed on a testset of 500 equations due to timeouts and delays from Mathematica (which the authors mention makes testing on larger sets infeasible), and are as follows:  
![Table 2](https://chinglamchoi.github.io/cchoi/files/table2.png)  
The authors also conclude that wider beams significantly improve accuracy. It is important to note that results for the Transformer are reported for recall@k=1, recall@k=10 and recall@k=50 (corresponding to the 3 beam sizes), meaning if at least 1 of the top k results found by beam search is equivalent to the reference answer, the inference result is judged as correct.  
  
Conclusion
======
This paper explores seq2seq Transformer models as an alternative and competitor of conventional methods -- Computer Algebra Systems and Tree-to-tree models. Lample and Charton model mathematics as a natural language, introduce methods for parsing unary-binary trees as sequences, introduce comprehensive methods for investigating their problem space, as well as methods for data generation. Although symbolic mathematics computation has long been dominated by CAS, Lample and Charton demonstrate the superiority of neural architectures in tasks of function integration and solving 1st & 2nd order differential equations with constraints, evidencing the potential of sequence models in tasks of symbolic reasoning.
  
Notes on Implementation
======
As of January 21, 2020, neither the code nor datatset has been made available for this paper. To implement differential equation solvers with Neural Networks in general, apart from using Python libraries, we can also consider Rewrite.jl, ModelingToolki.jl and Transformers.jl packages in the Julia Programming Language -- all of which are actively being maintained and improved. With the tagline "Looks like Python, feels like Lisp, runs like C/Fortran", Julia is a great alternative for scientific and numerical computing.  
  
References
======
[1] G. Lample and F. Charton. Deep Learning for Symbolic Mathematics, ICLR 2020.  
[2] E.Davis. The Use of Deep Learning for Symbolic Integration A Review of (Lample and Charton, 2019) ArXiv e-prints, abs/1912.05752v2.,2019.  
[3] B. Piotrowski, C. Brown, J. Urban, and C. Kaliszyk. Can Neural Networks Learn Symbolic Rewriting?, AITP 2019.  
[4] A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. N. Gomez, L. Kaiser, and I. Polosukhin. Attention Is All You Need. ArXiv e-prints, abs/1706.03762., 2017.  
[5] D. Bahdanau, K. Cho, and Y. Bengio. Neural Machine Translation by Jointly Learning to Align and Translate. ArXiv e-prints, abs/1409.0473.,2014.  
[6]Rewrite.jl  
[7] ModelingTookit.jl  
[8] Transformers.jl  
[9] The Julia Programming Language  
