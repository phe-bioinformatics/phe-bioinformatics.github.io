---
layout: post
title: "Growing a ggtree"
date: 2017-06-14
---
### Experiments with ggtree

  - Reading in a tree
  ```R
  library("ape")
  library("ggtree")
  tree <- read.tree("/path/to/newick_file")
  ```
  - Plot the tree
  ```R
  p <- ggtree(tree)
  plot(p)
  ```
  ![First Tree]({{ site.url }}/assets/first_tree.png)
  
  - I prefer a right ladderized tree
  ```R
  p <- ggtree(tree, right = TRUE)
  plot(p)
  ```
  ![Ladderized right tree]({{ site.url }}/assets/ladderized_right_tree.png)
  - Let's add some tip lables and a title
  ```R
  p <- ggtree(tree, right = TRUE) + ggtitle("Test Tree") + geom_tiplab(size = 2)
  plot(p)
  ```
  ![Labelled Tree]({{ site.url }}/assets/title_and_labels.png)
  
  
