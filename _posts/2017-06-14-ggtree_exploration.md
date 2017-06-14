---
layout: post
title: "Growing a ggtree"
date: 2017-06-14
---
### Experiments with ggtree

Being predominantly a python user when it comes to bioinformatics analyses I was keen to try and find a python library to draw trees with associated 'heatmaps' that display categorical meta data alongside the tip labels of the tree. I really tried to like the ete3 library that some had recommended but found the interface to the functions too obtuse. Other colleagues suggested the ggtree package in R. Although it has a similar API to ggplot2, which in itself takes some getting used to, the documentation is good and with a little effort taken to read the extnsive docs has a fairly intuitive interface.

This post will explore my explorations in how phylogenies and associated heatmaps can be drawn using the [ggtree package](https://guangchuangyu.github.io/ggtree/) in R. I would strongly recommend using [RStudio](https://www.rstudio.com/) since it makes working within R a breeze, particularly with graphical outputs.

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
  **N.B** Addition of `ggplot2::xlim(0, 0.3)` is necessary to stop truncated labels when aligned right, the second parameter needs to be determined by trial and error. See [FAQ](https://guangchuangyu.github.io/ggtree/faq/)
  ```R
  p <- ggtree(tree, right = TRUE) + ggtitle("Test Tree") + geom_tiplab(size = 2, align=TRUE, linesize=.25)  + geom_treescale(x=0.05, y=0, offset=2, fontsize = 3) + ggplot2::xlim(0, 0.3)
 plot(p)
 ```
 ![Right aligned labels and Scale Tree]({{ site.url }}/assets/aligned_labels_and_scale_bar.png)
 - Adding a bootstrap value is a bit more fun, requiring some data manipulation.
   - Get all data from the tree and find only non leaf nodes.
   - Convert the lables to numeric values and only keep those where the value is >65
   - Add it to the tree as `geom_text`. The `hjust` and `vjust` parameters can be eited to control position (again by trial and error)
   - **N.B** This is also foind in the [FAQ](https://guangchuangyu.github.io/ggtree/faq/)
 
 ```R
 p <- ggtree(tree, right = TRUE) + ggtitle("Test Tree") + geom_tiplab(size = 2, align=TRUE, linesize=.25)  + geom_treescale(x=0.05, y=0, offset=2, fontsize = 3) + ggplot2::xlim(0, 0.3)
 
 d <- p$data
 d <- d[!d$isTip,]
 d$label <- as.numeric(d$label)
 d <- d[d$label > 65,]
 p <- p + geom_text(data=d, aes(label=label), size=3, hjust = 1.25, vjust = -0.4)
 plot(p)
 ```
 ![Bootstrapped Tree]({{ site.url }}/assets/add_bootstraps.png)
 - Now the tree is looking kinda OK, we can get round to adding the heatmap
   Data is provided in the format as a tsv and read using the following code. Header and row names are specified using the two parameters `header=TRUE` and `row.names=1`. `check.names=FALSE` is necessary in case sample names begin with a numeric.
 ```R
 meta_data <- read.table("meta.tsv", sep="\t", header=TRUE,check.names=FALSE, stringsAsFactor=F, row.names = 1)
 ```
 
   | sample | phenotype | MIC |
   |--------|-----------|-----|
   | sample_60546 | resistant | 10 |
   | sample_40537 | high level resistant | 256 |
   | sample_00125 | sensitive | 0 |
   | sample_01454 | intermediate | 0.5 |


  - We can plot a heatmap of the data contained in this file with the following code
   ```R
   hm <- gheatmap(p,meta_data, offset = 0.02, width=0.15, font.size=3, colnames_position= "top", colnames_angle = 90, colnames_offset_y = 0, hjust = 0) + scale_fill_manual(values=c("sensitive" = "green", "intermediate" = "turquoise", "resistant" = "blue", "high level resistant" = "purple3", "0" = "white", "0.25" = "white", "0.5" = "gold", "10" = "darkorange2", "15" = "darkorange2", "20" = "darkorange2", "256" = "firebrick3"))
 plot(hm)
 ```
 Breaking this down:
   - The offset param determines the distance between the tree and the heatmap (trial and error to set the best distance)
   - width represents the proportion of the entire plot that will be used by the heatmap
   - font.size is the size of the column headings
   - colnames_position is the position of the columns labels (could also be 'bottom')
   - colnames_angle is self explanatory
   - colnames_offset_y and hjust allow fine tuning of the column name position
   - scale_fill_manual - this is the most critical param and the pairs in the values vector of the format <value> = <colout> determines which values in the meta data tsv file will be coloured with which colour (see [R colour chart](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf))
   - The legend may not be broken up appropriately into value groups so I would suggest a bit of manipulation in a vecor drawing program such as [Inkscape ](https://inkscape.org) to get it publication ready
   
 ![Heatmap Tree]({{ site.url }}/assets/heat_map.png)
 
  
  
