---
layout: post
title: "Growing a ggtree - part 2, adding tip shapes"
date: 2017-07-07
---
### Adding tip labels

In addition to adding a heatmap I found that adding coloured shapes to tips was a useful feature

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
  
  |taxa|age_group|country_of_residence|
  |----|---------|--------------------|
  |sample1|adult female|Scotland|
  |sample2|adult male|Wales|
  |sample3|child female|England|
  |sample4|child male|England|
  
  - This data can then be plotted as tip labels where the colours are either determined randomly
  ```R
  p <- p %<+% tip_metadata + geom_tippoint(aes(color=age_group), size=3)
  plot(p)
  ```
  ![Coloured Tips Randomly]({{ site.url }}/assets/tip_shape_colour_random.png)

  - Or if you want to enter them manually use the scale_color_manual functionality where colours are assigned to the column values based on alphabetical order
  ```R
  p <- p %<+% tip_metadata + geom_tippoint(aes(color=age_group), size=3) + scale_color_manual(values=c("red", "blue","green","grey"))
  plot(p)
  ```
  ![Coloured Tips Manually]({{ site.url }}/assets/tip_shape_colour_manual.png)
  - The shapes can be altered based on another columns
  ```R
  p <- p %<+% tip_metadata + geom_tippoint(aes(color=age_group, shape=country_of_residence), size=3) + scale_color_manual(values=c("red", "blue","green","grey"))
  ```
  ![Random Shapes]({{ site.url }}/assets/tip_shape_random.png)
  - To specify the shapes manually use the + scale_shape_manual function
  ```R
  p <- p %<+% tip_metadata + geom_tippoint(aes(color=age_group, shape=country_of_residence), size=3) + scale_color_manual(values=c("red", "blue","green","grey")) + scale_shape_manual(values=c(1,2,3))
  ```
  ![Manual Shapes]({{ site.url }}/assets/tip_shape_manual.png)

  The shape numbers can be found [here](http://www.cookbook-r.com/Graphs/Shapes_and_line_types)

