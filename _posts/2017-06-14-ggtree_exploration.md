---
layout: post
title: "Growing a ggtree"
date: 2017-06-14
---
# Experiments with ggtree

  - Reading in a tree
  ```R
library("ape")
library("ggtree")
tree <- read.tree("/path/to/newick_file")
```
  - plot the tree
  ```R
  p <- ggtree(tree)
  plot(p)
  ```
  ![First Tree]({{ site.url }}/assets/first_tree.png)
