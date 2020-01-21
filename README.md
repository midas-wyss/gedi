# Gene Expression Dynamics Inspector (GEDI) (v2.1)

GEDI was developed by Yuchun Guo, Ying Feng, Gabriel Eichler and Sui Huang at Children’s Hospital Boston, Harvard Medical School. This software is made available for non-commercial research use only, with permission from Children's Hospital, Boston. US and foreign patents pending. 

GEDI is currently hosted on GitHub by the [Predictive BioAnalytics](mailto:midas@wyss.harvard.edu) group at the [Wyss Institute](www.wyss.harvard.edu) and is provided `as is`. For user manual, settings, example inputs, please refer to the `HTML` files in this repository or keep reading.

If you decide to use GEDI in your work, please cite:
Eichler, G.S., Huang, S. Ingber, D.E., Gene Expression Dynamics Inspector (GEDI): for integrative analysis of expression profiles, Bioinformatics, 19(17),2321-2 [[PubMed](https://www.ncbi.nlm.nih.gov/pubmed/14630665)]


## 1. Introduction
### 1.1 What is GEDI?
GEDI is a program for the analysis of high-dimensional data, such as genome-wide profiling of gene expression. It allows the visualization, inspection and navigation through large sets of such data. It was originally developed for displaying dynamic data, specifically, multiple parallel gene expression profile time courses, in order to map the “gene expression state space”, e.g., following treatment of the same system with an various drugs. However, it can also effectively help analyze large amounts of static profiles (e.g. patient samples) and help find patterns of expression without a priori knowledge of any underlying structure.
           
### 1.2 The basic philosophy
The basic idea behind GEDI is fundamentally different from conventional clustering programs, such as hierarchical clustering. The goal is not to find “clusters” of co-regulated genes. Data analysis in GEDI is sample-oriented rather than gene-oriented, i.e., the object of analysis is an array or sample (e.g., condition, patient, time point). Although in conventional hierarchical clustering, a “two-way” clustering is performed, producing both gene clusters and sample clusters, a given sample as an entity is lost, since it will end up as a branch in a dendrogram that can become very dense when many samples are involved. In GEDI, the notion of a sample as an entity is preserved. In contrast, the very idea of grouping (“clustering”) of samples based on their gene behaviors or of genes based on their behavior in the various sample is a secondary, “emerging” process. At the core of GEDI, each sample (= array) is mapped into a “mosaic (“GEDI map”) which facilitates the recognition of genome-wide patterns, but at the same time allows the user to zoom-in onto genes of interest given its participation in the emerging global patterns. Thus, GEDI covers multiple scales of information, adhering to the spirit of “systems approaches” to present “the whole” as an integrated entity - yet allowing to directly link system features to “the parts”, the individual genes.
 
### 1.3 How GEDI presents the data
In brief, the output of GEDI is characteristic mosaic or “GEDI Maps”, a two-dimensional grid picture for each sample. Each tile in the mosaic represents a “minicluster” of genes (e.g., 10 genes) whose behavior across the entire set of analyzed samples is highly similar. Similarly behaving miniclusters in turn are placed in the same neighborhood on the grid, hence creating a higher-order mosaic pattern. The color of the tile in a particular mosaic (i.e., SOM grid) represents the expression level of the genes in that minicluster in that particular sample. The tiles in each mosaic of the same analysis represent the same genes, thus enabling the direct comparison of samples (microarrays). In summary, GEDI maps each array into a mosaic pattern as a well-recognizable, memorizable and characteristic object that gives each sample an engrammic identity.
 
### 1.4 Under the hood of GEDI          
The underlying algorithm for creating the mosaics is an SOM (Self-Organizing Map) algorithm.  However, unlike the conventional applications of SOMs to classify genes or samples into a predetermined number of discrete groups (“clusters”), GEDI creates a SOMs-based mosaic for every sample. The learning process is not used to classify genes, but to generate the mosaic patterns based on miniclusters that allows human eye to find similarity based on “Gestalt” perception. In other words, GEDI can be thought of as taking the dots on a microarray that represent expression levels of each gene, grouping together small groups of genes that behave very similarly over the set of arrays to miniclusters (dimension reduction) ) and rearranging them so that similar miniclusters are placed next to each other.
 
GEDI runs SOMs in two phases. 1st phase is the rough training phase. 2nd phase is the fine training phase. For different phases, different parameters can be set in the settings (see below).
 
The number of SOM miniclusters clusters translates into the ‘resolution’ of the mosaic, and we use SOMs with hundreds of nodes, or ‘mini-clusters’, typically containing 0-20 genes, to create high resolution mosaic pictures. The color of each tile of the mosaic is determined by the centroid (approximate to the average of all the genes in that tile) value of that respective mini-cluster.
 
Because SOMs place similar genes into the same neighborhood, coherent and robust pictures emerge that are characteristic of every sample. Every sample (or array) is associated with one mosaic picture. Due to the concatenation of the samples and time courses in the input data matrix (see data file preparation documents), every tile of the mosaic in each of the mosaics corresponds to the same group of genes across the sample
 
GEDI can analyze two different types of gene expression profile data:
　
 - Dynamic analysis: Analysis of multiple parallel time-series data, allowing the comparison of multiple high-dimensional time courses, e.g. following treatments with various drugs. A mosaic then represents a state in the gene expression state space, and its trajectory through state space is reflected in the change of the mosaic patterns which can be displayed as a movie.
 - Static analysis: Analysis of non-time-series data, e.g. expression profiles of tumor samples from different patients, or comparison between normal or disease sample, etc. Because of its original design for parallel time series data and the concept of sample-oriented analysis, GEDI allows efficient navigation through “sample space”, facilitating quick visual comparison of individual samples in a large set of profiles, similar to sorting through a large stack of trading baseball cards.
 
　
_The input data format, which can be obtained by simple manual modification of the expression data spreadsheets (see 3.1.) determines whether GEDI will analyze in the Dynamic or Static data mode._

### 1.5 Screenshot
![gedi_screenshot](screenshots/screenShot.jpg)

GEDI displays the analysis name and the data set name on the title bar of the program. The main area of GEDI is the working areas; it is organized into 4 Tabs:

 - Welcome: This is the welcome screen when you just open GEDI.
 - Data: This is to display the information about the dataset.
 - GEDI-Map/Movie: This is the main working area to view GEDI Maps and detailed information about genes, etc.
 - Display/Calculation: This allows user to selectively display a subset of the samples, or to calculate “Average Maps” or “Difference Maps” from selected GEDI mosaics.
  
Above the working area are the menu items and some tool bar buttons for commonly used functions.


### 1.6 Basic Capabilities and Feature
 
 - Sample clustering: Every sample is translated into a distinct mosaic, yielding a stack of mosaics. The user can browse though the stacks and can choose to display selected samples The mosaics can be saved independently as jpeg files, and used for presentation and printing using standard graphics programs, such as PowerPoint or Corel Draw.
 - Gene clustering: Genes that behave similarly across the set of samples are located in tiles in the same region of the grid. Tightly co-regulated genes are assigned to the same tiles. Thus, genes that behave similarly in a subset of the samples, and distinctively between samples, will spring to eye, e.g., “as a red island in a sea of blue”. Clicking on the spots will show the genes in the gene list window.
 - Sample comparison: Static samples can be selected and used to calculate either “Average maps”, representing the “average mosaic” that displays the average value for every tile from the various samples, e.g., of a group of selected samples representing same diagnosis; or “Difference maps” by subtracting two mosaics tile-wise to facilitate sample comparison.
 - Animation: If a stack of mosaic contains a group of sample representing time points of a time course, the stack can be animated to show the change of the transcriptomes, allowing recognition of coherent dynamic patterns across the profile.
 - Analysis of individual genes: User can retrieve the genes associated with features in the mosaic patterns. Gene description of a gene selected based on a feature in a GEDI map for one sample and along with its behavior in other samples can be displayed by clicking on the tiles, which displays the genes contained in that respective minicluster, and by clicking on the specific gene in the list. Alternatively, a specific gene name can be search for by entering its name in the search window, which will display its location on the mosaics.
 
### 1.7 The work flow and work environment of GEDI
 
The GEDI environment is simple. It consists of one single user interface window containing various, fixed displays. Working with GEDI consists of the following steps:

 - Loading the data as one text file. The data will need to be in a simple predefined format, see section 3.1., based on which GEDI will recognize whether the data represent static or dynamic data.
 - Setting the parameters. This step is optional, the default settings work well. Settings pertain to both SOM parameters (mosaic resolution, learning) as well as the display (coloring, grid shape, etc)
 - Create mosaic stacks (static analysis) or movie (dynamic analysis).
 - Visual inspection of the results in the dynamic user interface: Watching movies of the animated mosaics, browsing through stacks of mosaics, selecting interesting samples for direct comparison, retrieving gene names based on interesting patterns or locating genes by name in the patterns
 - Export results as Html pages, Jpeg images, or gene lists. The session with results can also be saved.
 
 
