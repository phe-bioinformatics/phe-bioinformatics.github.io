---
layout: post
title: "Growing a ggtree - part 2, adding tip shapes"
date: 2017-07-07
---
### Adding tip labels

In addition to adding a heatmap I found that adding coloured shapes to tips was a useful addition

  - First off read and plot the tree as before
  ```R
  tree <- read.tree("/path/to/newick/tree")
  p <- ggtree(tree, right = TRUE)
  plot(p)
  ```
  - Next read in an additional tsv file with taxa names in the first column and meta data to plots as shapes in additional columns
  ```R
  # read in tiplabel metadata
  tip_metadata <- read.table("meta_data.txt", sep="\t", header=TRUE,check.names=FALSE, stringsAsFactor=F)
  ```
  The format for this is as follows:
  
  |taxa|age_group|
  ======================
  |sample1|adult female|
  |sample2|adult male|
  |sample3|child female|
  |sample4|child male|
  

p <- p %<+% tip_metadata + geom_tippoint(aes(color=sexual_orientation), size=3) + scale_color_manual(values=c("red", "blue","green","grey"))

