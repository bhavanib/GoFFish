Title:
GoFFish - A Graph Oriented Framework for Insight using Scalable Heuristics

Guide:
Dr. Dinkar Sitaram
Professor,
PES Institute of Technology,
Bangalore - 85

Team Members:
Bhavani B		- 1PI11IS017
Anushree Prasanna Kumar	- 1PI11IS027
Mithilesh Kumar Gupta	- 1PI11IS059


Introduction to GoFFish
-----------------------

The GoFFish project offers a distributed framework for storing time-series graphs and composing graph analytics. It takes a clean-slate approach that leverages best practices and patterns from scalable data analytics such as Hadoop, HDFS, Hive, and Giraph, but with an emphasis on performing native analytics on graph (rather than tuple) data structures. This offers an more intuitive storage, access and programming model for graph datasets while also ensuring performance optimized for efficient analysis over large graphs (millions-billions of vertices) and many instances of them (thousands-millions of graph instances).

Time-series graphs are increasingly common. E.g. a network of sensors sampling the environment effectively emits periodic snapshots of a graph, whose vertices are sensors readings and edges are relationships between sensors (e.g. distance between traffic cameras). Or a corpus of network route traces offers a series of temporal snapshot of the internet, with IP addresses forming vertices, and edges being known routes between them. Further, static or time-variant name-value properties can be associated with vertices and edges. Analytics over timeseries graphs leverage both (horizontal) traversals over space and (vertical) traversals over time. Hence, the distributed storage and analytics framework for such datasets need to offer efficiently and accessible primitives for network-oriented and timeseries-oriented access.

GoFFish has two loosely-coupled and independently usable components, developed in Java 7 with limited Jython bindings: the GoFS distributed graph file system, and the Gopher subgraph-centric graph analytics framework. These play similar roles as HDFS and Hadoop do for tuple datasets. GoFS spatially partitions graph instances and stores them across different hosts in a commodity-cluster. The default partitioning scheme reduces edge cuts and balances vertices per partition. Each partition stores all temporal instances of the vertices and edges in that partition, with APIs provided to retrieve temporal subsets of the instances from disk. Basic projection capabilities allow the retrieval of only specific properties of interest, though selection based on property values is not supported - GoFS is meant for efficient processing of large graph instances rather than free-form graph querying. Further, users can also access connected subgraphs within a partition and their instances independently - subgraphs are semantically more meaningful since they guarantee that every vertex in the subgraph has a path to every other vertex using just local edges present within that subgraph. Partitions do not have tis requirement, and so a partition can be decomposed into one or more subgraphs. This allows users to maximize local processing of the subgraph within a host before distributed coordination of analytics. The API also identifies remote edges of the subgraph, and the partition/host holding it, to allow distributed processing.

While users can use their favourite programming framework to access the distributed graph datasets using GoFS's Java APIs (e.g. Giraph, Hama), Gopher is subgraph-centric composition framework for building graph analytics and is naturally couples with GoFS. Gopher offers a distributed programming model that is inspired by Google's Pregel model but relaxes the constraint on vertex-centric programming that forces many unnecessary supersteps and the added coordination overhead. Rather, Gopher takes a subgraph-centric notion where the user's logic has access to the entire subgraph structure, its state and the remote edges (rather than a single vertex and its edges). This allows more processing to take place within a subgraph before coordinating across subgraphs using synchronized message-passing across supersteps. Furthermore, Gopher exposes not just a single subgraph to the user logic but also allows them to bind to GoFS so that the user logic can access an iterator over subgraphs. This offer a hybrid programming model that leverages the best features of MapReduce (iterator over subgraphs, rather than tuples) and Pregel (subgraph, rather than vertex, centric computation with BSP-style message passing). Alternatively, users can use/implement their own backend storage model to pass subgraph iterators to Gopher instead of GoFS. Gopher is implemented on top of the award-winning Floe continuous dataflow engine for composable analytics.


Installation
------------
*Single Node:
GoFFish framework can be installed on a single node by following the manual that is present with this file. The attached scripts folder is to be used for this process.

*Cluster installation:
GoFFish framework can be installed on a cluster of nodes by following the manual that is present with this file. The attached goffish-2.6.1 folder is to be used for this process.


