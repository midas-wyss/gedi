# Gene Expression Dynamics Inspector (GEDI) (v2.1)

GEDI was developed by Yuchun Guo, Ying Feng, Gabriel Eichler and Sui Huang at Children’s Hospital Boston, Harvard Medical School. This software is made available for non-commercial research use only, with permission from Children's Hospital, Boston. US and foreign patents pending. 

GEDI is currently hosted on GitHub by the [Predictive BioAnalytics](mailto:midas@wyss.harvard.edu) group at the [Wyss Institute](www.wyss.harvard.edu) and is provided `as is`. For user manual, settings, example inputs, please refer to the `HTML` files in this repository or keep reading.

If you decide to use GEDI in your work, please cite:
Eichler, G.S., Huang, S. Ingber, D.E., Gene Expression Dynamics Inspector (GEDI): for integrative analysis of expression profiles, Bioinformatics, 19(17),2321-2 [[PubMed](https://www.ncbi.nlm.nih.gov/pubmed/14630665)]


## Introduction
### What is GEDI?
GEDI is a program for the analysis of high-dimensional data, such as genome-wide profiling of gene expression. It allows the visualization, inspection and navigation through large sets of such data. It was originally developed for displaying dynamic data, specifically, multiple parallel gene expression profile time courses, in order to map the “gene expression state space”, e.g., following treatment of the same system with an various drugs. However, it can also effectively help analyze large amounts of static profiles (e.g. patient samples) and help find patterns of expression without a priori knowledge of any underlying structure.
           
### The basic philosophy
The basic idea behind GEDI is fundamentally different from conventional clustering programs, such as hierarchical clustering. The goal is not to find “clusters” of co-regulated genes. Data analysis in GEDI is sample-oriented rather than gene-oriented, i.e., the object of analysis is an array or sample (e.g., condition, patient, time point). Although in conventional hierarchical clustering, a “two-way” clustering is performed, producing both gene clusters and sample clusters, a given sample as an entity is lost, since it will end up as a branch in a dendrogram that can become very dense when many samples are involved. In GEDI, the notion of a sample as an entity is preserved. In contrast, the very idea of grouping (“clustering”) of samples based on their gene behaviors or of genes based on their behavior in the various sample is a secondary, “emerging” process. At the core of GEDI, each sample (= array) is mapped into a “mosaic (“GEDI map”) which facilitates the recognition of genome-wide patterns, but at the same time allows the user to zoom-in onto genes of interest given its participation in the emerging global patterns. Thus, GEDI covers multiple scales of information, adhering to the spirit of “systems approaches” to present “the whole” as an integrated entity - yet allowing to directly link system features to “the parts”, the individual genes.
 
### How GEDI presents the data
In brief, the output of GEDI is characteristic mosaic or “GEDI Maps”, a two-dimensional grid picture for each sample. Each tile in the mosaic represents a “minicluster” of genes (e.g., 10 genes) whose behavior across the entire set of analyzed samples is highly similar. Similarly behaving miniclusters in turn are placed in the same neighborhood on the grid, hence creating a higher-order mosaic pattern. The color of the tile in a particular mosaic (i.e., SOM grid) represents the expression level of the genes in that minicluster in that particular sample. The tiles in each mosaic of the same analysis represent the same genes, thus enabling the direct comparison of samples (microarrays). In summary, GEDI maps each array into a mosaic pattern as a well-recognizable, memorizable and characteristic object that gives each sample an engrammic identity.
 
### Under the hood of GEDI          
The underlying algorithm for creating the mosaics is an SOM (Self-Organizing Map) algorithm.  However, unlike the conventional applications of SOMs to classify genes or samples into a predetermined number of discrete groups (“clusters”), GEDI creates a SOMs-based mosaic for every sample. The learning process is not used to classify genes, but to generate the mosaic patterns based on miniclusters that allows human eye to find similarity based on “Gestalt” perception. In other words, GEDI can be thought of as taking the dots on a microarray that represent expression levels of each gene, grouping together small groups of genes that behave very similarly over the set of arrays to miniclusters (dimension reduction) ) and rearranging them so that similar miniclusters are placed next to each other.
 
GEDI runs SOMs in two phases. 1st phase is the rough training phase. 2nd phase is the fine training phase. For different phases, different parameters can be set in the settings (see below).
 
The number of SOM miniclusters clusters translates into the ‘resolution’ of the mosaic, and we use SOMs with hundreds of nodes, or ‘mini-clusters’, typically containing 0-20 genes, to create high resolution mosaic pictures. The color of each tile of the mosaic is determined by the centroid (approximate to the average of all the genes in that tile) value of that respective mini-cluster.
 
Because SOMs place similar genes into the same neighborhood, coherent and robust pictures emerge that are characteristic of every sample. Every sample (or array) is associated with one mosaic picture. Due to the concatenation of the samples and time courses in the input data matrix (see data file preparation documents), every tile of the mosaic in each of the mosaics corresponds to the same group of genes across the sample
 
GEDI can analyze two different types of gene expression profile data:
　
 - Dynamic analysis: Analysis of multiple parallel time-series data, allowing the comparison of multiple high-dimensional time courses, e.g. following treatments with various drugs. A mosaic then represents a state in the gene expression state space, and its trajectory through state space is reflected in the change of the mosaic patterns which can be displayed as a movie.
 - Static analysis: Analysis of non-time-series data, e.g. expression profiles of tumor samples from different patients, or comparison between normal or disease sample, etc. Because of its original design for parallel time series data and the concept of sample-oriented analysis, GEDI allows efficient navigation through “sample space”, facilitating quick visual comparison of individual samples in a large set of profiles, similar to sorting through a large stack of trading baseball cards.
 
　
_The input data format, which can be obtained by simple manual modification of the expression data spreadsheets (see 3.1.) determines whether GEDI will analyze in the Dynamic or Static data mode._

