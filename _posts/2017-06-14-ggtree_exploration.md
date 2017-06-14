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
  - If you prefer right aligned labels this can be done, and we'll also add a scale bar.
  **N.B** Addition of `ggplot2::xlim(0, 0.3)` is necessary to stop truncated labels when aligned right, the second argument needs to be determined by trial and error
  ```R
  p <- ggtree(tree, right = TRUE) + ggtitle("Test Tree") + geom_tiplab(size = 2, align=TRUE, linesize=.25)  + geom_treescale(x=0.05, y=0, offset=2, fontsize = 3) + ggplot2::xlim(0, 0.3)
 plot(p)
 ```
 ![Right aligned labels and Scale Tree]({{ site.url }}/assets/aligned_labels_and_scale_bar.png)
  
  
