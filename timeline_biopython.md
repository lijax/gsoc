# Phylogenetics in Biopython: Filling in the gaps

### Project Timeline

- **Week1(6.17-6.23)**: Distance Matrix and UPGMA
    - **Tasks**: 1. a method to calculate distance matrix for an alignment; 2. port my java code of UPGMA to python;
    - **Difficulty**: Easy. Because I've already write those algorithms in java before, it should be easy to complete. A potential problem is how to be compatible with existing modules in Phylo package, same for other tasks.
- **Week2(6.24-6.30)**: NJ and Maximum parsimony
    - **Tasks**: 1. port my java code of UPGMA to python; 2. a method to calculate the parsimony score for a given tree and an alignment;
    - **Difficulty**: Easy. Both of them were written before. 
- **Week3(7.1-7.7)**: Maximum parsimony
    - **Tasks**: 1. implement the Nearest-neighbor interchanges algorithm to search for a tree minimizing the score. 2. implement Subtree pruning and regrafting algorithm.
    - **Difficulty**: Medium. I've already know how both algorithm work. Just need to figure out how to write a method to change the tree structure, compatible with existing tree manipulation method.
- **Week4(7.8-7.14)**: Consensus tree search
    - **Tasks**: 1. to be efficient in consensus tree search, create a binary array manipulation class to store and count clades; 2. write phylip like majority-rule consensus tree search method for a list of trees; 
    - **Difficulty**: Medium to Hard. The first one is the most important and difficult. I have written a similar class in Java for consensus tree search in BlastGraph. But it doesn't really use binary operations. This class really needs to be implemented using binary operations to be efficient.
- **Week5(7.15-7.21)**: Adams consensus and branch support
    - **Tasks**: 1. dig into adams consensus tree algorithms and implement it; 2. write method for branch support calculation given a tree and a list of trees;
    - **Difficulty**: Medium. Needs to get familiar with the adams consensus tree algorithms. The branch support calculation method for a target tree is similar to consensus tree search method.
- **Week6(7.22-7.28)**: Cleanup
    - **Tasks**: cleanup existing code, write tests and documentation
- **Week7(7.29-8.4)**: Mid-term evaluations
    - **Tasks**: continue former tasks. Write and submit mid-term evaluations.
- **Week8(8.5-8.11)**: Bootstrap method
    - **Tasks**: 1. write the bootstrap method; 2. write a interface method to generate a bootstrapped tree list providing the parameter of tree method(upgma,nj,mp) and bootstrap time; 3 write another one for consensus tree given the tree method, consensus method and bootstrap time.
    - **Difficulty**: Easy.
- **Week9(8.12-8.18)**: Circular rooted tree
    - **Tasks**: 1. read code of Bio.Phylo._utils module and understand how the draw and draw_graphviz work; 2. write adaptable method draw_circular for circular rooted tree.
    - **Difficulty**: Medium to Hard. There is existing code for circular tree in BlastGraph. May need to learn matplotlib API before porting the code.
- **Week10(8.19-8.25)**: Radial unrooted tree
    - **Tasks**: Implement radial layout, which is a fairly widely used algorithm; 
    - **Difficulty**: Hard.
- **Week11(8.26-9.1)**: Felsenstein's Equal Daylight algorithm
    - **Tasks**: Implement Felsenstein's Equal Daylight algorithm, which takes a radial layout and iteratively adjusts it to make it prettier.
    - **Difficulty**: Hard. Refer to *Inferring Phylogenies* Page 582. Any existing code in any language will be helpful.
- **Week12(9.2-9.8)**: cleanup existing code, write tests and documentation
- **Week13(9.9-9.15)**: cleanup existing code, write tests and documentation
- **Week14(9.16-9.22)**: Suggested 'pencils down' date. Take a week to scrub code, write tests, improve documentation, etc.
- **Week15(9.23-9.29)**: Firm 'pencils down' date. Write and submit final evaluations to Google. Submit required code samples to Google.