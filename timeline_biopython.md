# Phylogenetics in Biopython: Filling in the gaps

### Project Timeline

- **Week1(6.17-6.23)**: Distance Matrix and UPGMA
    - **Tasks**: 1. design and write document for the distance matrix calculation and UPGMA methods; 2. implement distance matrix calculation of multiple sequence alignment object; 3. implement UPGMA method by porting my java code for it; 4. write tests
    - **Difficulty**: Easy. Because I've already write those algorithms in java before, it should be easy to complete. A potential problem is how to be compatible with existing modules in Phylo package, same for other tasks.
- **Week2(6.24-6.30)**: NJ and Maximum parsimony
    - **Tasks**: 1. design and write document for the Neighbor Joining and parsimony score methods; 2. implement NJ by porting my java code for it; 3. implement method to calculate the parsimony score for a given tree and an alignment; 4. write tests
    - **Difficulty**: Easy. Both of them were written before.
- **Week3(7.1-7.7)**: Maximum parsimony
    - **Tasks**: 1. design and write document for the parsimony tree searching method; 2. implement the Nearest-neighbor interchanges algorithm to search for a tree minimizing the score; 3. write tests
    - **Difficulty**: Medium. I've already know how both algorithm work. Just need to figure out how to write a method to change the tree structure, compatible with existing tree manipulation method.
- **Week4(7.8-7.14)**: Consensus tree search
    - **Tasks**: 1. design and write document for a binary array class and the majority-rule consensus method; 2. to be efficient in consensus tree search, create a binary array manipulation class to store and count clades; 3. write phylip like majority-rule consensus tree search method for a list of trees; 4. write tests
    - **Difficulty**: Medium to Hard. The first one is the most important and difficult. I'll implement a classwith the same API using common array and improve the performance later.
- **Week5(7.15-7.21)**: Adams consensus and branch support
    - **Tasks**: 1. design and write document for the adams consensus and branch support methods; 2. dig into adams consensus tree algorithms and implement it; 3. write method for branch support calculation given a tree and a list of trees; 4. write tests
    - **Difficulty**: Medium. Need to get familiar with the adams consensus tree algorithms. The branch support calculation method for a target tree is similar to consensus tree search method.
- **Week6(7.22-7.28)**: Bootstrap method
    - **Tasks**: 1. design and write document for the bootstrap and some interface methods; 2. write the bootstrap method; 2. write a interface method to generate a bootstrapped tree list providing the parameter of tree method(upgma,nj,mp) and bootstrap time; 3 write another one for consensus tree given the tree method, consensus method and bootstrap time; 4. write tests
    - **Difficulty**: Easy.
- **Week7(7.29-8.4)**: cleanup and Mid-term evaluations
    - **Tasks**: 1. cleanup existing code, improve tests and document; 2. Write and submit mid-term evaluations.
- **Week8(8.5-8.11)**: Tree drawing preparation
    - **Tasks**: 1. read code of Bio.Phylo._utils module and understand how the draw and draw_graphviz work; 2. extract a Bio.Phylo._draw module from it; 3. design new methods of circular layout, radial layout and Equal Daylight algorithm.
    - **Difficulty**: Medium. May need to learn matplotlib API before porting the code.
- **Week9(8.12-8.18)**: Circular rooted tree
    - **Tasks**: 1. design and write document for the circular tree method; 2. write adaptable method draw_circular for circular rooted tree; 3. write tests
    - **Difficulty**: Medium. There is existing code for circular tree in BlastGraph. 
- **Week10(8.19-8.25)**: Radial unrooted tree
    - **Tasks**: 1. design and write document for the radial layout method; 2. Implement radial layout, which is a fairly widely used algorithm; 3. write tests
    - **Difficulty**: Medium. Already get a reference with pseudocode of the algorithm.
- **Week11(8.26-9.1)**: Felsenstein's Equal Daylight algorithm
    - **Tasks**: 1. design and write document for the Felsenstein's Equal Daylight algorithm; 2. Implement Felsenstein's Equal Daylight algorithm, which takes a radial layout and iteratively adjusts it to make it prettier; 3. write tests
    - **Difficulty**: Medium. Refer to *Inferring Phylogenies* Page 582. Already understand how it works.
- **Week12(9.2-9.8)**: cleanup existing code, improve tests and documentation
- **Week13(9.9-9.15)**: write tutorials, improve the phylo cookbook for biopython
- **Week14(9.16-9.22)**: Suggested 'pencils down' date. Take a week to scrub code, improve documentation, etc.
- **Week15(9.23-9.29)**: Firm 'pencils down' date. Write and submit final evaluations to Google. Submit required code samples to Google.