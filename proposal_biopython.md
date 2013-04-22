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

### Project abstract:
Biopython is a set of open source python packages and modules for bioinformatics works. In the Bio.Phylo package, there are already implementations for some basic phylogenetics tasks: basic tree operations, parsers for Newick, Nexus and PhyloXML, and wrapers for Phyml, Raxml and PAML. While there are some important components that remain to be implemented to better support phylogenetic workflows. These include simple tree construction algorithms, consensus tree searching, tree comparison and tree visualization.

### Approach & Goals

- Implement simple tree inference algorithms of UPGMA, neighbor-joining and maximum parsimony.
- Implement consensus tree search functions of multiple trees. Phylip like majority-rule consensus tree, Adams consensus.
- Implement branch support calculation functions given a target tree and a list of bootstrap replicate trees.
- Implement a bootstrap method for a given alignment and provide two interface methods to generate a tree list and construct a consensus tree(given the parameters of treeMethod, consensusMethod and bootstrapTime).
- Implement SH test for multiple tree comparisons.
- Implement visualization functions for circular rooted tree and radial unrooted tree.

### Project Timeline

- **Week1(6.17-6.23)**: Tree inference algorithms
    - **Tasks**: 1. a method to calculate distance matrix for an alignment; 2. port my java code of UPGMA and NJ to python; 3. find a solution for maximum parsimony algorithms;
    - **Difficulty**: Easy. Because I've already write those algorithms in java before, it should be easy to complete the first two tasks. So there will be more time to concentrate on the third one.
- **Week2(6.24-6.30)**: Maximum parsimony
    - **Tasks**: 1. a method to calculate the parsimony score for a given tree and an alignment; 2. implement an efficient algorithm to search for a tree minimizing the score(Branch and Bound, Heuristic search etc.).
    - **Difficulty**: Hard. The first one is easy, which I wrote before. So the main task is on the second one. If no optimal algorithms can be implemented by the end, maybe we should just write a wrapper for external programs(i.e. dnapars and protpars in PHYLIP).
- **Week3(7.1-7.7)**: Consensus tree search
    - **Tasks**: 1. to be efficient in consensus tree search, create a binary array manipulation class to store and count clades; 2. write phylip like majority-rule consensus tree search method for a list of trees; 
    - **Difficulty**: Medium to Hard. The first one is the most important and difficult. I have written a similar class in Java for consensus tree search in BlastGraph. But it doesn't really use binary operations. This class really needs to be implemented using binary operations to be efficient.
- **Week4(7.8-7.14)**: Consensus tree and branch support
    - **Tasks**: 1. dig into adams consensus tree algorithms and implement it; 2. write method for branch support calculation given a tree and a list of trees;
    - **Difficulty**: Medium. Needs to get familiar with the adams consensus tree algorithms. The branch support calculation method for a target tree is similar to consensus tree search.
- **Week5(7.15-7.21)**: Bootstrap method
    - **Tasks**: 1. write the bootstrap method; 2. write a interface method to generate a bootstrapped tree list providing the parameter of tree method(upgma,nj,mp) and bootstrap time; 3 write another one for consensus tree given the tree method, consensus method and bootstrap time.
    - **Difficulty**: Easy.
- **Week6(7.22-7.28)**: cleanup
    - **Tasks**: cleanup existing code, write tests and documentation
- **Week7(7.29-8.4)**: Mid-term evaluations
    - **Tasks**: continue former tasks. Write and submit mid-term evaluations.
- **Week8(8.5-8.11)**: SH-test
    - **Tasks**: 1.understand how SH-test works; 2. write code for SH-test
    - **Difficulty**: Hard. Refer to *Inferring Phylogenies* Page 370. Any existing code in any language will be helpful.
- **Week9(8.12-8.18)**: circular rooted tree
    - **Tasks**: 1. read code of Bio.Phylo._utils module and understand how the draw and draw_graphviz work; 2. write adaptable method draw_circular for circular rooted tree.
    - **Difficulty**: Medium to Hard. There is existing code for circular tree in BlastGraph. May need to learn matplotlib API before porting the code.
- **Week10(8.19-8.25)**: radial unrooted tree
    - **Tasks**:  1. understand Felsenstein's Equal Daylight algorithm and find a proper solution for radial layout; 2. write code for radial unrooted tree.
    - **Difficulty**: Hard. Refer to *Inferring Phylogenies* Page 582. Any existing code in any language will be helpful.
- **Week11(8.26-9.1)**: continue radial unrooted tree
- **Week12(9.2-9.8)**: cleanup existing code, write tests and documentation
- **Week13(9.9-9.15)**: cleanup existing code, write tests and documentation
- **Week14(9.16-9.22)**: Suggested 'pencils down' date. Take a week to scrub code, write tests, improve documentation, etc.
- **Week15(9.23-9.29)**: Firm 'pencils down' date. Write and submit final evaluations to Google. Submit required code samples to Google.

## Conflict
No apparent conflicts yet.