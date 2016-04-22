---
layout: page
permalink: /features/
description: "Major features of Monocle."
modified: 2013-09-11
#tags: [Bowtie, Tophat, Cufflinks, CummeRbund]
---

Monocle is a toolkit for analyzing large, complex single-cell RNA-Seq experiments. It includes functions that can help you tackle the challenges that are common in most such experiments. Recently, we overhauled Monocle to support the next generation of single-cell RNA-Seq. The new improved algorithms in Monocle 2 can handle much larger and complex experiments. With Monocle 2, you can:

# Classify and count cells

Single-cell RNA-Seq experiments allow you to discover (and possibly rare) subtypes of cells. Monocle helps you identify them.

Most single-cell RNA-Seq protocols capture only a small percentage of the mRNA in each cell, which leads to so-called "drop outs", where mRNAs that are probably present just aren't detected. This makes it hard to determine how many cells of each type there are in your experiment. Monocle has a machine-learning based algorithm for classifying cells by type even in the face of missing data, and even when your cells have been treated in different ways or collected from varying conditions. You can use it to track population dynamics, measure responses to perturbation, and even discover new cell types.

<p style="text-align:center"><img src="/images/cell_type_impute.png"/></p>

In the plot above, we can recognize some cells as fibroblasts and others as myoblasts on the basis of a few key marker genes. Even though these markers drop-out in many cells, Monocle 2 can still identify cells that lack them by clustering all the cells together based on global expression profile.

# Compare cell populations through differential expression

Single-cell RNA-Seq promises to unveil new cell types and new functional states for known ones. Characterizing new cell types and states begins with comparing them to other, better understood cells. Monocle includes a sophisticated but easy-to-use system for differential expression.

<p style="text-align:center"><img src="/images/example_T_cell_differential_genes.png"/></p>

Monocle allows you to compare groups of cells to identify genes that differ between them, even after controlling for other variables in the experiment. For example, the plot above shows genes that differ between two conditions, but only one of them, LAG3, is **also** different between cell types, even after subtracting the other effects. You can use Monocle's differential expression system to zero in on the genes that are most relevant in your experiment.

# Identify new cell-type specific markers

Many researchers are using single-cell RNA-Seq to discover new cell types. Monocle can help you purify them or characterize them further by identifying key marker genes that you can use in follow up experiments such as immunofluorescence or flow sorting. Monocle leverages its differential analysis procedures so you can be assured of the statistical robustness of your marker panel, leading to reliable downstream analysis.

<p style="text-align:center"><img src="/images/all_cells_marker_heatmap.png"/></p>

In the plot above, Monocle has identified the top 20 most cell-type specific genes for some of the major lymphocytes in peripheral blood. 

# Robustly track changes over (pseudo) time

In development, disease, and throughout life, cells transition from one state to another. Monocle helps you discover these transitions.

Monocle introduced the concept of **pseudotime**, which is a measure of how far a cell has moved through biological progress. Monocle uses machine learning to reconstruct the sequence of gene regulatory changes each cell must execute as it transitions from one state to another. It can do so without knowing ahead of time which genes define these states or movement between them. After it learns the transition path, or *trajectory*, it places each cell at the correct position along it.

<p style="text-align:center"><img src="/images/neuron_mst.png"/></p>

Monocle 2 includes a vastly improved algorithm for learning trajectories and ordering cells. It's based on a recently introduced technique called [reverse graph embedding](http://dl.acm.org/citation.cfm?id=2783309) (Mao et al, KDD 15), a highly scalable nonlinear manifold learning technique.

The plot above shows the transcriptional trajectory from developing [olfactory neurons](http://science.sciencemag.org/content/350/6265/1251). (Hanchate et al, Science, 2015).

<p style="text-align:center"><img src="/images/neuron_pseudotime_changes.png"/></p>

Essentially, Monocle treats each cell as a separate time point in an ultra-high resolution time series. Instead of three or four time points, you get three or four thousand - one for each cell in the experiment. This resolution allows you distinguish genes regulated early in a complex cascade of transcriptional changes from those more downstream. For example, in the plot above you can see that Neurod1 is downregulated before cells enter an immature neuronal state, after which Gng8 is upregulated. As the cells reach maturity, Cnga2 becomes highly expressed.

# Dissect cellular decisions with branch analysis

Single-cell trajectory analysis not only exposes transient events as cells move from one state to another. It reveals how cells choose between one of several possible end states. For example, in development, multipotent cells make fate choices, ultimately differentiating into one of a number of terminal cell types. The new reconstruction algorithms introduced in Monocle 2 can robustly reveal branching trajectories. Inspecting the branch points helps you dissect the gene regulatory circuits that cells use to navigate these decisions. 

<p style="text-align:center"><img src="/images/branch_trajectory.png"/></p>


Here, you can see cells that start at a common state (shown in pink) and move through a branched trajectory to one of several functionally distinct outcomes. Which genes are affected at the branches? Monocle 2 includes a new statistical procedure, called Branched Expression Analysis Modeling (BEAM), that reports all genes that change around a selected branch. BEAM can be used to track genes along two lineages in parallel from both a **global** perspective:

<p style="text-align:center"><img src="/images/BEAM_heatmap.png"/></p>

As well as a on a **gene-by-gene** basis:

<p style="text-align:center"><img src="/images/branched_pseudotime.png"/></p>

In both plots, you can see the progressive lineage restriction of the genes along the two branches as a function of pseudotime. Further analyzing these genes can lead to insights about how the lineage choice is made in development.

# Count transcripts, not reads

In RNA-Seq, we measure expression by counting how many sequencing reads originate from each gene.  But what really matters is how many mRNAs there were for each gene. Spiking in "alien" transcripts such as the [ERCC spike-in controls](https://www.thermofisher.com/order/catalog/product/4456740) allows you to transform expression measurements from read counts into mRNA counts. However, spike-in controls can be tricky to use correctly, and they aren't always reliable. Monocle allows you to analyze mRNA counts *without the need for spike-in controls*. It includes an algorithm motivated by some surprising and fundamental statistical properties of the single-cell mRNA sampling process that lets you recover mRNA counts via inference rather than an exogenous ladder. These counts are generally as or more reliable than those obtained with spikes. 

<p style="text-align:center"><img src="/images/spike_free_consistency.png"/></p>

Here, you can see that mRNA counts from Monocle's spike-free algorithm are actually more consistent across cells over a range of sequencing depths than those from spike-in controls or a measure of relative expression.  

# Analyze huge experiments

With current single-cell RNA-Seq techniques such as [Drop-Seq](http://mccarrolllab.com/dropseq/) / [inDrop](http://www.cell.com/cell/abstract/S0092-8674(15)00500-0), you can easily capture thousands or even tens of thousands of cells in a single experiment. Such experiments often include many different conditions and controls meant to probe the structure of a complex cellular community. Analyzing such datasets is extremely challenging, in part because of their sheer size. For example, the plot below is from an experiment with more than 50,000 cells!

<p style="text-align:center"><img src="/images/cell_counts_10X.png"/></p>

Monocle 2 has been re-engineered from the ground up to support large, sparse RNA-Seq datasets. All of the major steps in the Monocle workflow including cell clustering, differential expression, and trajectory analysis can be performed on a single PC, even for huge datasets. 
