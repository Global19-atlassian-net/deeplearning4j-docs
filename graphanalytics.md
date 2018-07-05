---
title: A Beginner's Guide to Graph Analytics and Deep Learning
layout: default
redirect_from: graphtheory
redirect_from: graphdata
redirect_to:
  - https://skymind.ai/wiki/graph-analysis
---

# A Beginner's Guide to Graph Analytics and Deep Learning

Contents

* [Concrete Examples of Graph Data Structures](#example)
* [Difficulties of Graph Data: Size and Structure](#difficulty)
* [Representing and Traversing Graphs for Machine Learning](#represent)
* [Footnotes](#footnote)
* [Further Resources on Graph Data Structures and Deep Learning](#reading)

Graphs are data structures that can be ingested by various algorithms, notably neural nets, learning to perform tasks such as classification, clustering and regression. 

TL;DR: here's one way to make graph data ingestable for the algorithms: 

```
Data (graph, words) -> Real number vector -> Deep neural network
```

Algorithms can “embed” each node of a graph into a real vector (similar to the embedding a [word](./word2vec)). The result will be vector representation of each node in the graph with some information preserved. Once you have the real number vector, you can feed it to the neural network.

## <a name="example">Concrete Examples of Graph Data Structures</a>

The simplest definition of a graph is "a collection of items connected by edges." There are many problems where it's helpful to think of things as a graph.<sup>[1](#one)</sup> The items are often called *nodes* or *points* and the edges are often called *vertices*, the plural of vertex. Here are a few concrete examples of a graph:  

* Cities are nodes and highways are edges
* Humans are nodes and relationships between them are edges (in a social network)
* States are nodes and the transitions between them are edges (for more on states, see our post on [deep reinforcement learning](./deepreinforcementlearning)). For example, a video game is a graph of states connected by actions that lead from one state to the next...
* Atoms are nodes and chemical bonds are edges (in a molecule)
* Web pages are nodes and hyperlinks are edges 
* A thought is a graph of synaptic firings (edges) between neurons (nodes)
* A [neural network](./neuralnet-overview) is a graph ... that makes predictions about other graphs. The nodes are places where computation happens and the edges are the paths by which signal flows through the mathematical operations

Any ontology or knowledge graph charts the interrelationship of entities (combining [symbolic AI](./symbolicreasoning) with the graph structure):

* Taxonomies of animal species
* Diseases that share etiologies and symptoms
* Medications that share ingredients

<p align="center">
<a href="https://docs.skymind.ai/docs/welcome" type="button" class="btn btn-lg btn-success" onClick="ga('send', 'event', ‘quickstart', 'click');">GET STARTED WITH GRAPHS & DEEP LEARNING</a>
</p>

## <a name="difficulty">Difficulties of Graph Data: Size and Structure</a>

Applying neural networks and other machine-learning techniques to graph data can de difficult. 

The first question to answer is: What kind of graph are you dealing with?

Let's say you have a finite state machine, where each state is a node in the graph. You can give each state node a unique ID, maybe a number. Then you give all the rows the names of the states, and you give all the columns the same names, so that the matrix contains an element for every state to intersect with every other state. Then you could mark those elements with a 1 or 0 to indicate whether the two states were connected in the graph, or even use weighted nodes (a continuous number) to indicate the likelihood of a transition from one state to the next. (The transition matrix below represents a finite state machine for the weather.) 

![Alt text](./img/transition-matrix.png)

That seems simple enough, but many graphs, like social network graphs with billions of nodes (where each member is a node and each connection to another member is an edge), are simply too large to be computed. **Size** is one problem that graphs present as a data structure. In other words, you can't efficiently store a large social network in a tensor. They don't compute.

Neural nets do well on vectors and tensors; data types like images (which have structure embedded in them via pixel proximity -- they have fixed size and spatiality); and sequences such as text and time series (which display structure in one direction, forward in time). 

Graphs have an **arbitrary structure**: they are collections of things without a location in space, or with an arbitrary location. They have no proper beginning and no end, and two nodes connected to each other are not necessarily "close". 

You usually don't feed whole graphs into neural networks, for example. They would have to be the same shape and size, and you'd have to line up your graph nodes with your network's input nodes. But the whole point of graph-structured input is to not know or have that order. There's no first, there's no last. 

![Alt text](./img/graph1.jpg)

The second question when dealing with graphs is: What kind of question are you trying to answer by applying machine learning to them? In social networks, you're usually trying to make a decision about what kind person you're looking at, represented by the node, or what kind of friends and interactions does that person have. So you're making predictions about the node itself or the edges. 

Since that's the case, you can address the uncomputable size of a Facebook-scale graph by looking at a node and its neighbors maybe 1-3 degrees away; i.e. a subgraph. The immediate neighborhood of the node, taking `k` steps down the graph in all directions, probably captures most of the information you care about. You're filtering out the giant graph's overwhelming size.

## <a name="represent">Representing and Traversing Graphs for Machine Learning</a>

Let's say you decide to give each node an arbitrary representation vector, like a low-dimensional word embedding, each node's vector being the same length. The next step would be to traverse the graph, and that traversal could be represented by arranging the node vectors next to each other in a matrix. You could then feed that matrix representing the graph to a recurrent neural net. That's basically DeepWalk (see below), which treats truncated random walks across a large graph as sentences. 

If you turn each node into an embedding, much like word2vec does with words, then you can force a neural net model to learn representations for each node, which can then be helpful in making downstream predictions about them. (How close is this node to other things we care about?)

Another more recent approach is a *graph convolutional network*, which very similar to convolutional networks: it passes a node filter over a graph much as you would pass a convolutional filter over an image, registering each time it sees a certain kind of node. The readings taken by the filters are stacked and passed to a maxpooling layer, which discards all but the strongest signal, before we return to a filter-passing convolutional layer.  

One interesting aspect of graph is so-called side information, or the attributes and features associated with each node. For example, each node could have an image associated to it, in which case an algorithm attempting to make a decision about that graph might have a CNN subroutine embedded in it for those image nodes. Or the side data could be text, and the graph could be a tree (the leaves are words, intermediate nodes are phrases combining the words) over which we run a recursive neural net, an algorithm popolarized by Richard Socher. 

Finally, you can compute derivative functions such as graph Lapalians from the tensors that represent the graphs, much like you might perform an eigen analysis on a tensor. These functions will tell you things about the graph that may help you classify or cluster it. (See below for more information.)  

### <a name="beginner">More Deep Learning Tutorials</a>

* [Introduction to Neural Networks](./neuralnet-overview)
* [Deep Reinforcement Learning](./deepreinforcementlearning)
* [Convolutional Networks (CNNs)](./convolutionalnetwork)
* [Generative Adversarial Networks (GANs)](./generative-adversarial-network)
* [LSTMs and Recurrent Networks](./lstm)
* [Word2Vec: Neural Embeddings for Java](./word2vec)
* [Combining Symbolic Learning & Deep Learning](./symbolicreasoning)
* [Restricted Boltzmann Machines](./restrictedboltzmannmachine)
* [Eigenvectors, Covariance, PCA and Entropy](./eigenvector)
* [Neural Networks & Regression](./logistic-regression)
* [Open Datasets for Machine Learning](./opendata)
* [Inference: Machine Learning Model Server](./machine-learning-modelserver)

## <a name="footnote">Footnotes</a>

<a name="one">1)</a> *In a weird meta way it's just graphs all the way down, [not turtles](https://en.wikipedia.org/wiki/Turtles_all_the_way_down). A human scientist whose head is full of firing synapses (graph) is both embedded in a larger social network (graph) and engaged in constructing ontologies of knowledge (graph) and making predictions about data with neural nets (graph).*

## <a name="reading">Further Resources on Graph Data Structures and Deep Learning</a>

Below are a few papers discussing how neural nets can be applied to data in graphs. 

[Relational inductive biases, deep learning, and graph networks](https://arxiv.org/abs/1806.01261)

By Peter W. Battaglia, Jessica B. Hamrick, Victor Bapst, Alvaro Sanchez-Gonzalez, Vinicius Zambaldi, Mateusz Malinowski, Andrea Tacchetti, David Raposo, Adam Santoro, Ryan Faulkner, Caglar Gulcehre, Francis Song, Andrew Ballard, Justin Gilmer, George Dahl, Ashish Vaswani, Kelsey Allen, Charles Nash, Victoria Langston, Chris Dyer, Nicolas Heess, Daan Wierstra, Pushmeet Kohli, Matt Botvinick, Oriol Vinyals, Yujia Li, Razvan Pascanu (DeepMind, Google Brain, MIT, U. of Edinburgh)

*Artificial intelligence (AI) has undergone a renaissance recently, making major progress in key domains such as vision, language, control, and decision-making. This has been due, in part, to cheap data and cheap compute resources, which have fit the natural strengths of deep learning. However, many defining characteristics of human intelligence, which developed under much different pressures, remain out of reach for current approaches. In particular, generalizing beyond one’s experiences—a hallmark of human intelligence from infancy—remains a formidable challenge for modern AI.*

*The following is part position paper, part review, and part unification. We argue that combinatorial generalization must be a top priority for AI to achieve human-like abilities, and that structured representations and computations are key to realizing this objective. Just as biology uses nature and nurture cooperatively, we reject the false choice between “hand-engineering” and “end-to-end” learning, and instead advocate for an approach which benefits from their complementary strengths. We explore how using relational inductive biases within deep learning architectures can facilitate learning about entities, relations, and rules for composing them. We present a new building block for the AI toolkit with a strong relational inductive bias—the graph network—which generalizes and extends various approaches for neural networks that operate on graphs, and provides a straightforward interface for manipulating structured knowledge and producing structured behaviors. We discuss how graph networks can support relational reasoning and combinatorial generalization, laying the foundation for more sophisticated, interpretable, and flexible patterns of reasoning.*

*A key signature of human intelligence is the ability to make “infinite use of finite means” (Humboldt, 1836; Chomsky, 1965), in which a small set of elements (such as words) can be productively composed in limitless ways (such as into new sentences). This reflects the principle of combinatorial generalization, that is, constructing new inferences, predictions, and behaviors from known building blocks. Here we explore how to improve modern AI’s capacity for combinatorial generalization by biasing learning towards structured representations and computations, and in particular, systems that operate on graphs.*

[Representation Learning on Graphs: Methods and Applications](https://cs.stanford.edu/people/jure/pubs/graphrepresentation-ieee17.pdf) (2017)

by William Hamilton, Rex Ying and Jure Leskovec

*Machine learning on graphs is an important and ubiquitous task with applications ranging from drug design to friendship recommendation in social networks. The primary challenge in this domain is finding a way to represent, or encode, graph structure so that it can be easily exploited by machine learning models. Traditionally, machine learning approaches relied on user-defined heuristics to extract features encoding structural information about a graph (e.g., degree statistics or kernel functions). However, recent years have seen a surge in approaches that automatically learn to encode graph structure into low-dimensional embeddings, using techniques based on deep learning and nonlinear dimensionality reduction. Here we provide a conceptual review of key advancements in this area of representation learning on graphs, including matrix factorization-based methods, random-walk based algorithms, and graph convolutional networks. We review methods to embed individual nodes as well as approaches to embed entire (sub)graphs. In doing so, we develop a unified framework to describe these recent approaches, and we highlight a number of important applications and directions for future work.*

[A Short Tutorial on Graph Laplacians, Laplacian Embedding, and Spectral Clustering](https://csustan.csustan.edu/~tom/Clustering/GraphLaplacian-tutorial.pdf)

by Radu Horaud

[Community Detection with Graph Neural Networks](https://arxiv.org/abs/1705.08415) (2017)

by Joan Bruna and Xiang Li

[DeepWalk: Online Learning of Social Representations](https://arxiv.org/abs/1403.6652) (2014)

by Bryan Perozzi, Rami Al-Rfou and Steven Skiena

*We present DeepWalk, a novel approach for learning latent representations of vertices in a network. These latent representations encode social relations in a continuous vector space, which is easily exploited by statistical models. DeepWalk generalizes recent advancements in language modeling and unsupervised feature learning (or deep learning) from sequences of words to graphs. DeepWalk uses local information obtained from truncated random walks to learn latent representations by treating walks as the equivalent of sentences. We demonstrate DeepWalk's latent representations on several multi-label network classification tasks for social networks such as BlogCatalog, Flickr, and YouTube. Our results show that DeepWalk outperforms challenging baselines which are allowed a global view of the network, especially in the presence of missing information. DeepWalk's representations can provide F1 scores up to 10% higher than competing methods when labeled data is sparse. In some experiments, DeepWalk's representations are able to outperform all baseline methods while using 60% less training data. DeepWalk is also scalable. It is an online learning algorithm which builds useful incremental results, and is trivially parallelizable. These qualities make it suitable for a broad class of real world applications such as network classification, and anomaly detection.*

[DeepWalk is implemented in Deeplearning4j](https://github.com/deeplearning4j/deeplearning4j/blob/1f8af820c29cc5567a2c5eaa290f094c4d1492a7/deeplearning4j-graph/src/main/java/org/deeplearning4j/graph/models/deepwalk/DeepWalk.java).

[Deep Neural Networks for Learning Graph Representations](https://pdfs.semanticscholar.org/1a37/f07606d60df365d74752857e8ce909f700b3.pdf) (2016)
by Shaosheng Cao, Wei Lu and Qiongkai Xu

*In this paper, we propose a novel model for learning graph representations, which generates a low-dimensional vector
representation for each vertex by capturing the graph structural information. Different from other previous research efforts,
we adopt a random surfing model to capture graph structural information directly, instead of using the samplingbased
method for generating linear sequences proposed by Perozzi et al. (2014). The advantages of our approach will
be illustrated from both theorical and empirical perspectives. We also give a new perspective for the matrix factorization
method proposed by Levy and Goldberg (2014), in which the pointwise mutual information (PMI) matrix is considered as
an analytical solution to the objective function of the skipgram model with negative sampling proposed by Mikolov et
al. (2013). Unlike their approach which involves the use of the SVD for finding the low-dimensitonal projections from
the PMI matrix, however, the stacked denoising autoencoder is introduced in our model to extract complex features and
model non-linearities. To demonstrate the effectiveness of our model, we conduct experiments on clustering and visualization
tasks, employing the learned vertex representations as features. Empirical results on datasets of varying sizes show
that our model outperforms other state-of-the-art models in such tasks.*

[Deep Feature Learning for Graphs](https://arxiv.org/abs/1704.08829)

by Ryan A. Rossi, Rong Zhou, Nesreen K. Ahmed

[Learning multi-faceted representations of individuals from heterogeneous evidence using neural networks](https://arxiv.org/abs/1510.05198) (2015)

by Jiwei Li, Alan Ritter and Dan Jurafsky

*Inferring latent attributes of people online is an important social computing task, but requires integrating the many heterogeneous sources of information available on the web. We propose learning individual representations of people using neural nets to integrate rich linguistic and network evidence gathered from social media. The algorithm is able to combine diverse cues, such as the text a person writes, their attributes (e.g. gender, employer, education, location) and social relations to other people. We show that by integrating both textual and network evidence, these representations offer improved performance at four important tasks in social media inference on Twitter: predicting (1) gender, (2) occupation, (3) location, and (4) friendships for users. Our approach scales to large datasets and the learned representations can be used as general features in and have the potential to benefit a large number of downstream tasks including link prediction, community detection, or probabilistic reasoning over social networks.*

[node2vec: Scalable Feature Learning for Networks](https://arxiv.org/abs/1607.00653) (Stanford, 2016)
by Aditya Grover and Jure Leskovec

*Prediction tasks over nodes and edges in networks require careful effort in engineering features used by learning algorithms. Recent research in the broader field of representation learning has led to significant progress in automating prediction by learning the features themselves. However, present feature learning approaches are not expressive enough to capture the diversity of connectivity patterns observed in networks. Here we propose node2vec, an algorithmic framework for learning continuous feature representations for nodes in networks. In node2vec, we learn a mapping of nodes to a low-dimensional space of features that maximizes the likelihood of preserving network neighborhoods of nodes. We define a flexible notion of a node's network neighborhood and design a biased random walk procedure, which efficiently explores diverse neighborhoods. Our algorithm generalizes prior work which is based on rigid notions of network neighborhoods, and we argue that the added flexibility in exploring neighborhoods is the key to learning richer representations. We demonstrate the efficacy of node2vec over existing state-of-the-art techniques on multi-label classification and link prediction in several real-world networks from diverse domains. Taken together, our work represents a new way for efficiently learning state-of-the-art task-independent representations in complex networks.*

[Gated Graph Sequence Neural Networks](https://arxiv.org/abs/1511.05493) (Toronto and Microsoft, 2017)
by Yujia Li, Daniel Tarlow, Marc Brockschmidt and Richard Zemel

*Graph-structured data appears frequently in domains including chemistry, natural language semantics, social networks, and knowledge bases. In this work, we study feature learning techniques for graph-structured inputs. Our starting point is previous work on Graph Neural Networks (Scarselli et al., 2009), which we modify to use gated recurrent units and modern optimization techniques and then extend to output sequences. The result is a flexible and broadly useful class of neural network models that has favorable inductive biases relative to purely sequence-based models (e.g., LSTMs) when the problem is graph-structured. We demonstrate the capabilities on some simple AI (bAbI) and graph algorithm learning tasks. We then show it achieves state-of-the-art performance on a problem from program verification, in which subgraphs need to be matched to abstract data structures.*

[Graph Classification with 2D Convolutional Neural Networks](https://arxiv.org/abs/1708.02218)
