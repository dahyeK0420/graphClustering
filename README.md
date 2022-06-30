# Graph Clustering
This program performs clustering to graph data set. Each observation in the data set represents the title of a research paper. The title is then assigned with a unique ID, which is also the unique ID of the representing node. The research paper referred by another is adjacent, connected by an edge. The key to this part of the assignment is different embedding techniques of the graph data set given, which could heavily impact the performance of clustering algorithm.

## Reading in the Datasets
Following datasets were utilised:
- `adjedges.txt`: a text file containing the neighbour information of each and every node possible. The files also
contain the nodes without labels from ‘labels.txt’.
- `docs.txt`: a text file showing the document that each node represents. The file contains two columns – the node
ID, and the paper title.
- `labels.txt`: a text file with the true labels of all the nodes. Each integer adjacent to the node ID is the group that the node belongs to.

## Perform Clustering
According to label_dict, one of the five unique labels are assigned to each node. The proportion of each label is not equal – class 0 takes up around 25% of all observations, followed by class 2, 1, 3, and 4. Class 4 is the one with the least number of observations assigned. To understand the meaning behind the cluster, I merged the data frame with document title and the label. 6 articles from each label are extracted, and I realised that the text data from some documents barely contain any information regarding the node itself. As an instance, the node 31916958’s document title is solely ‘Introduction’, which does not convey any information regarding the node when looking at the text. Therefore, we could assume that text embedding for this clustering task might have a poor performance compared to other embedding methods.  
  
  
Therefore, I aimed to perform clustering tasks and identified 5 different clusters. To perform the graph clustering, following algorithms and models are applied:  
  
  
- **Spectral Clustering**: using Laplacian Matrix Decomposition and K-means clustering using scipy and sklearn
  * The adjacency matrix of the graph is pre-processed into Laplacian matrix. The Laplacian matrix is then decomposed into lower-dimensional matrices through calculating eigenvalues and eigenvectors of the Laplacian matrix. The resulting matrix is a k-dimensional matrices, where K is the number of clusters we
desire. K means clustering with K = 5 is then applied using the product of decomposed matrices.
- **Random Walk Based Approach**: Node2Vec Network embedding with K-means clustering using node2vec and sklearn
  * The Node2Vec embedding, on the other hand, embeds each node based on their neighbouring nodes. Through random walks from a node within a specified walk, the Node2Vec is able to embed rich information based on adjacent nodes. For this type of embedding, I configured Node2Vec with walk length of 30 and 200 walks for each path and each node. The embedded matrix from this configuration is set to generate a matrix with the dimension of 18720x64. With this matrix, a K-mean clustering with K = 5 is applied.
- **Text Embedding using TF-IDF**: TF-IDF text embedding with K-means clustering using nltk and sklearn
  * given the document title assigned with each node, we can also embed the nodes with traditional text- embedding techniques. Here, I used TF-IDF embedding after pre-processing each title text with Regex tokenizer and Snowball stemmer. Unigrams with the length of > 1 without stopwords are reserved for TF- IDF transformation. With the sparse matrix returned by the vectorizer, K-mean clustering with K = 5 is
applied.
