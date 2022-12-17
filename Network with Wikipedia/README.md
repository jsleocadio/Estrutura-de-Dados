# Building a network with Wikipedia Pages

In this project, we create a network from wikipedia pages and analyse it. We'll use `Brazil` wikipage as root of our network. To accomplish that, the `networkx` and `wikipedia` libraries will help out.

![Flag of Brazil](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/Flag_of_Brazil.svg)

With `Brazil` as `SEED` of our network. We had create a network and pre-processed it. With that, it was analysed the degree, closeness, betweenness and eigenvector centrality for this network; also was visualized the core and the shell; and was created a data pipeline that including: Collecting data; Cleaning data; And export the results od the final artifact;

This works was done by [Jefferson Leocadio](https://github.com/jsleocadio) using as inspiration [Ivanovitch Silva's Data Structure](https://github.com/ivanovitchm/datastructure)

# Data Pipeline

It's important to build a data pipeline to automate all process: Colecting the data; Cleaning; and Results and exportation.

![Data Pipeline](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/pipeline.png)

A Pipeline is a set of functions that an input of the next function is the output of the function before. In this project, the `create_graph` step receives the `SEED` and `STOPS` and returns the raw graph. Next, the `preprocessing` step receives the raw graph and returns a processed graph for the `truncate` step to eliminate some duplicate nodes and contract them. 

Finally, we'll save the final graph as a .graphml file and reports the results obtained. And with that, the `explore_network` step reports all the graphs with the metrics studied in class, as well as the .txt files containing the extractions performed and the processing performed on the graph.

All outputs from the pipeline can be reached in [outputs](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/outputs) folder.

# Creating Graph

Initially, as explained we recieved the `SEED` and `STOPS` set. From there, we visit all pages from the root and generate an file with all the nodes.

# Preprocessing and truncate the network

In this step, we eliminated some duplicated nodes. The criteria for exclusion are plural and singular nodes and words separeted with '-'. After that, we exclude all nodes with degree = 1.

With this step we removed:

```
Nodes removed: 63.58%
Edges removed: 25.26%
Edges per nodes: 5.16
```

# Exploring the network

## Most significant nodes

To show the most significant nodes, we calculated the `indegree` of each degree, which is, the number of connections entering the node, since this is a network directed.

![Top 10 indegree](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/top_10_indegree.png)

The image above shows 10 top most significant nodes. The most topics are: Cities and Historic facts.

## Degree, closeness, betweenness and eigenvector centrality

Let's explain what each of this values are:

* **Degree centrality**: checks the number of connections (neighbors) of a node according to the number of nodes in the network;
* **Closeness centrality**: checks the average distance of a node to all other nodes. Thus, if the value of this metric is small for a certain node, it means that this node is farther from all other nodes in the network.
* **Betweenness centrality**: checks the position of a node on the shortest path. That is, if I am analyzing `node i`, among all the shortest paths from `node j` to `node h`, how many of these paths does `node i` form part of?
* **Eigenvector centrality**: checks the importance of a node based on the importance of its neighbors, that is, it checks whether a given node has important neighbors.

## Degree

![Degree](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/Degree.png)

## Eigenvector centrality

![Eigenvector Centrality](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/Eigenvector_centrality.png)

## Centrality distributions
The centrality distributions will be analyzed according to two metrics: degree and closeness centrality, both previously seen in the previous topic.

For this, it is important to understand two important concepts of statistics for read the graphs:

* **PDF**: Probability Density Function. For this case, it will describe that approximately X% of the nodes have degree/closeness centrality equal to Y;
* **CDF**: Cumulative Density Function. For this case, it will describe that approximately X% of the nodes have degree/closeness centrality equal to or less than Y.
### Centrality distribution for degree

First, the histogram was plotted for the analysis of the data referring to the degree of each node in the network, and as can be seen in the figure below, it appears that most nodes have a step value of less than 500.

![Degree Histogram](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/degree_all_hist.png)

Then, this histogram was plotted with the PDF and CDF curves, as can be seen in the figure below, and it is possible to see that many nodes have a degree below 100. When checked in the CDF curve, it is inferred that it comes very close to 100% nodes with degree less than 100.

![All nodes PDF](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/probability_density_function_all_nodes.png)
![All nodes CDF](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/cumulative_density_function_all_nodes.png)

Thus, in order to try to carry out a more detailed analysis in relation to degree, the network nodes were filtered, so that a subgraph was built only with nodes of degree 100 or higher. It can be noticed that the histogram has already become more distributed, as seen in the plots below, and that approximately 60% of the nodes (among the nodes with degree greater than 100) have a degree in the range of 100-200.

![Nodes 100 PDF](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/probability_density_function_nodes_100.png)
![Nodes 100 CDF](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/cumulative_density_function_nodes_100.png)

Similarly, the same analysis was performed for the closeness centraliy metric. It was not necessary to filter the graph, as in the previous case, since the data were already well distributed, with the highest occurrences 0.008 and 0.01, as seen in the figure below.

![Closeness Histogram](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/closeness_all_hist.png)

Then, plotting the histogram with the CDF and PDF functions, it is possible to see that approximately 90% of the network nodes have a closeness centrality value equal to or less than 0.01.

![Closeness PDF](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/probability_density_function_closeness.png)
![Closeness CDF](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/cumulative_density_function_closeness.png)

After that, it was made a plot which to compare the metrics. On the diagonal, it's possible to notice that the distributions have a long tail to the right side, with a positive symmetry, where many nodes have a small degree, and few nodes have a neighborhood of little importance. Also, the eigenvector and closeness have an exponential trend, i. e., as one increases, the other increases as well. However, there is a limitation in this trend, in that after a certain point, this is no longer a valid thing.

![All](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/all.png)

# Network visualization

There are some tools to help us visualize our work. We had use `Gephi` before, and now we will use [Retina](https://ouestware.gitlab.io/retina/beta/) and [Gephisto](https://jacomyma.github.io/gephisto/) to get better visualization.

## Editing our network

Even after all procedure done above, our network is GIGANTIC! With 25963 nodes and 134093 edges is very complex to show the Graph with quality. Because of that, we filtering our network by excluding all nodes with a degree inferior to 300. 

To make things easy, we used `pandas` library to help make that filter. After that, we add the node within a group by:

* Group 5: $300 \leq$ degree $\leq 499$
* Group 4: $500 \leq$ degree $\leq 699$
* Group 3: $700 \leq$ degree $\leq 999$
* Group 2: $1000 \leq$ degree $\leq 1199$
* Group 1: $1200 \leq$ degree

Notebook with codes can be accessed by: [CSV Parsing](https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/CSV_Parsing.ipynb)

After that our network has: 222 nodes and 16938

With the results of that procedure, we imported the `csv file` generated on `Gephi` and produced our final visualization. Yet on Gephi, we generated a new `graphml file` to use on `Retina` and `Gephisto`.

## Degree Visualizations

* Visualization on Retina
<figure align="center">
    <img src="https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/retina_degree.png">
    <figcaption>Graph generated on Retina. Each node size is based on how large is his degree, and the color of the node is based on which group it is in</figcaption>
</figure>


* Visualization on Gephisto
<figure align="center">
    <img src="https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/Gephisto_degree.png">
</figure>

## Community Visualization

To come with another visualization, we decided to use `the community detection`. The data was generated on `Gephi`, in the `statistics` tab of the gephi software, the `modularity metric` was chosen.

* Visualization on Retina
<figure align="center">
    <img src="https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/retina_community.png">
    <figcaption>Graph generated on Retina. It is possible to notice that three communities were detected in our network</figcaption>
</figure>


* Visualization on Gephisto
<figure align="center">
    <img src="https://github.com/jsleocadio/Estrutura-de-Dados/blob/main/Network%20with%20Wikipedia/images/Gephisto_modularity.png">
</figure>
