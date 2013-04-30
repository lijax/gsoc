# Phylogenetics in Biopython: Filling in the gaps

## Personal Information

- **Name**: Yanbo Ye
- **Email**: yeyanbo289@gmail.com
- **University**: Wuhan Institute of Virology, University of Chinese Academy of Sciences
- **Current Enrollment**: Third-year Master Student in Bioinformatics
- **Biography**: 
    - 2005-2010: Bachelor of Science in Biotechnology, Nankai University, Tianjin, China;
    - 2010-2013: Master of Science in Bioinformatics, University of Chinese Academy of Sciences, Beijing, China.

## Interests and Motivation

I'm interested in the project of "Phylogenetics in Biopython: Filling in the gaps". 

My current research is about phylogenetics and genome evolution of baculoviruses. To facilitate my work, I have developed a Java program integrating the workflow of my research, which includes sequence clustering, gene content tree building, visualization and gene gain and loss estimation. The most important thing is that I've re-implement most of the algorithms that needed in this biopython project, such as UPGMA, NJ, MP, consensus tree finding, newick tree parsing, cladogram and circular tree visualization. Though I never developed a python module, I use Biopython a lot during my research to do some trivial tasks like sequence parsing and data to tree mapping. So I'm very familiar with those algorithms and Biopython packages. I believe I can port my Java code into Biopython and implement those (tree comparison and bootstrap search for a target tree) not exist in my current code. 

I have been looking for a chance to start a python project for a long time. Based on my research background and experience, this project is just right one for me. Through this project, I want to gain more experience in Python programming and improve my cooperation skills of open source software development. 

## Programming Experience and Skills

- **Java:** 4 years
    - **BlastGraph**(https://github.com/bigwiv/BlastGraph): This is a comparative genomics tool integrating a range of bioinformatics tools and algorithms. The function of it is to cluster homologous sequences, estimate gene content tree and gene gain and loss event.
- **Python:** 1 year
    - **Bioscript**(https://github.com/lijax/bioscript): This includes some python and shell scripts I used for my research. 

- I also like to hack some javascript and python code during my spare time. Here is my github account: https://github.com/lijax

## Project Plan

### Project Abstract:
Biopython is a set of open source python packages and modules for bioinformatics works. In the Bio.Phylo package, there are already implementations for some basic phylogenetics tasks: basic tree operations, parsers for Newick, Nexus and PhyloXML, and wrapers for Phyml, Raxml and PAML. While there are some important components that remain to be implemented to better support phylogenetic workflows. These include simple tree construction algorithms, consensus tree searching, tree comparison and tree visualization.

### Approach & Goals

- Implement simple tree inference algorithms of UPGMA, neighbor-joining and maximum parsimony.
- Implement consensus tree search functions of multiple trees. Phylip like majority-rule consensus tree, Adams consensus.
- Implement branch support calculation functions given a target tree and a list of bootstrap replicate trees.
- Implement a bootstrap method for a given alignment and provide two interface methods to generate a tree list and construct a consensus tree(given the parameters of treeMethod, consensusMethod and bootstrapTime).
- Implement visualization functions for circular rooted tree and radial unrooted tree.

### Project Timeline

- [Here is the detail description of the project timeline](./timeline_biopython.md)

## Conflict
No apparent conflicts yet.
