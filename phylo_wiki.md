# New Features in Bio.Phylo

## Tree Construction

In addition to wrappers of tree construction programs(PHYLIP programs through EMBOSS wrappers in Bio.Emboss.Applications), now Biopython also provides several tree construction algorithm implementations in pure python in the `Bio.Phylo.TreeConstruction` module.

All algorithms are designed as worker subclasses of a base class `TreeConstructor`. All constructors have the same method `build_tree` that accept a `MultipleSeqAlignment` object to construct the tree. Currently there are two types of tree constructors: `DistanceTreeConstructor` and `ParsimonyTreeConstructor`. 

### DistanceTreeConstructor

The `DistanceTreeConstructor` has two algorithms: UPGMA(Unweighted Pair Group Method with Arithmetic Mean) and NJ(Neighbor Joining). 

Both algorithms construct trees based on a distance matrix. So before using these algorithms, let me introduce the `DistanceCalculator` to generate the distance matrix from a `MultipleSeqAlignment` object. The following code shows a common way to do this:

    >>> from Bio.Phylo.TreeConstruction import DistanceCalculator
    >>> from Bio import AlignIO
    >>> aln = AlignIO.read('Tests/TreeConstruction/msa.phy', 'phylip')
    >>> print aln
    SingleLetterAlphabet() alignment with 5 rows and 13 columns
    AACGTGGCCACAT Alpha
    AAGGTCGCCACAC Beta
    GAGATTTCCGCCT Delta
    GAGATCTCCGCCC Epsilon
    CAGTTCGCCACAA Gamma
    >>> calculator = DistanceCalculator('identity')
    >>> dm = calculator.get_distance(aln)
    >>> dm
    DistanceMatrix(names=['Alpha', 'Beta', 'Gamma', 'Delta', 'Epsilon'], matrix=[[0], [0.23076923076923073, 0], [0.3846153846153846, 0.23076923076923073, 0], [0.5384615384615384, 0.5384615384615384, 0.5384615384615384, 0], [0.6153846153846154, 0.3846153846153846, 0.46153846153846156, 0.15384615384615385, 0]])
    >>> print dm
    Alpha	0
    Beta	0.230769230769	0
    Gamma	0.384615384615	0.230769230769	0
    Delta	0.538461538462	0.538461538462	0.538461538462	0
    Epsilon	0.615384615385	0.384615384615	0.461538461538	0.153846153846	0
    	Alpha	Beta	Gamma	Delta	Epsilon

As you see, we create a `DistanceCalculator` object with a string 'identity', which is the name of the model(scoring matrix) to calculate the distance. The 'identity' model is the default one and can be used both for DNA and Protein sequence. To check available models for DNA, protein or all, use the attribute of the calculator `dna_models`, `protein_models`, `models` respectively. 

After the calculator is created with the model, simply use the `get_distance()` method to get the distance matrix of a given alignment object. Then you will get  a `DistanceMatrix` object, a subclass of `Matrix`(we will talk about this later).

Now, let's get back to the `DistanceTreeConstructor`. We can pass the `DistanceCalculator` object and a string parameter('nj' or 'upgma') to initialize it, and then call its `build_tree()` as mentioned before.  

    >>> from TreeConstruction import DistanceTreeConstructor
    >>> constructor = DistanceTreeConstructor(calculator, 'nj')
    >>> tree = constructor.build_tree(aln)
    >>> print tree
    Tree(rooted=False)
        Clade(branch_length=0, name='Inner3')
            Clade(branch_length=0.182692307692, name='Alpha')
            Clade(branch_length=0.0480769230769, name='Beta')
            Clade(branch_length=0.0480769230769, name='Inner2')
                Clade(branch_length=0.278846153846, name='Inner1')
                    Clade(branch_length=0.0512820512821, name='Epsilon')
                    Clade(branch_length=0.102564102564, name='Delta')
                Clade(branch_length=0.144230769231, name='Gamma')
    
    
While sometimes you might want to use your own `DistanceMatrix` directly instead of the raw alignment, we provide another direct way to use both algorithms.
    
    >>> from TreeConstruction import DistanceTreeConstructor
    >>> constructor = DistanceTreeConstructor()
    >>> tree = constructor.nj(dm)
    >>> print tree
    Tree(rooted=False)
        Clade(branch_length=0, name='Inner3')
            Clade(branch_length=0.182692307692, name='Alpha')
            Clade(branch_length=0.0480769230769, name='Beta')
            Clade(branch_length=0.0480769230769, name='Inner2')
                Clade(branch_length=0.278846153846, name='Inner1')
                    Clade(branch_length=0.0512820512821, name='Epsilon')
                    Clade(branch_length=0.102564102564, name='Delta')
                Clade(branch_length=0.144230769231, name='Gamma')
    >>> tree = constructor.upgma(dm)
    >>> print tree
    Tree(rooted=True)
        Clade(branch_length=0, name='Inner4')
            Clade(branch_length=0.1875, name='Inner1')
                Clade(branch_length=0.0769230769231, name='Epsilon')
                Clade(branch_length=0.0769230769231, name='Delta')
            Clade(branch_length=0.110576923077, name='Inner3')
                Clade(branch_length=0.0384615384615, name='Inner2')
                    Clade(branch_length=0.115384615385, name='Gamma')
                    Clade(branch_length=0.115384615385, name='Beta')
                Clade(branch_length=0.153846153846, name='Alpha')


### ParsimonyTreeConstructor

Unlike `DistanceTreeConstructor`, the concrete algorithm of `ParsimonyTreeConstructor` is delegated to two different worker classes: the `ParsimonyScorer` to calculate the parsimony score of a target tree by the given alignment, and the `TreeSearcher` to search the best tree that minimize the parsimony score. A typical usage example can be as follows:

    >>> from Bio import AlignIO
    >>> from TreeConstruction import *
    >>> aln = AlignIO.read(open('Tests/TreeConstruction/msa.phy'), 'phylip')
    >>> starting_tree = Phylo.read('Tests/TreeConstruction/nj.tre', 'newick')
    >>> scorer = ParsimonyScorer()
    >>> searcher = NNITreeSearcher(scorer)
    >>> constructor = ParsimonyTreeConstructor(searcher, starting_tree)
    >>> pars_tree = constructor.build_tree(aln)
    >>> print pars_tree
    Tree(weight=1.0, rooted=True)
        Clade(branch_length=0.0)
            Clade(branch_length=0.197335, name='Inner1')
                Clade(branch_length=0.13691, name='Delta')
                Clade(branch_length=0.08531, name='Epsilon')
            Clade(branch_length=0.041935, name='Inner2')
                Clade(branch_length=0.01421, name='Inner3')
                    Clade(branch_length=0.17523, name='Gamma')
                    Clade(branch_length=0.07477, name='Beta')
                Clade(branch_length=0.29231, name='Alpha')

The `ParsimonyScorer` is a combination of the Fitch algorithm and Sankoff algorithm. It will work as Fitch algorithm by default if no parameter is provide, and work as Sankoff algorithm if a parsimony scoring matrix(a `Matrix` object) is given, .
    
Then the scorer is passed to a `TreeSearcher` to tell it how to evaluate different trees during searching. Currently, only one searcher `NNITreeSearcher`, the Nearest Neighbor Interchange(NNI) algorithm, is implemented. 

By passing the searcher and a starting tree to the `ParsimonyTreeConstructor`, we finally get the instance of it. If no starting tree is provided, a simple upgma tree will be created instead, with the 'identity' model. To use this parsimony constructor, just simply call the `build_tree` method with an alignment.
 
## Consensus Tree

### Strict, Majority Rule and Adam Consensus
Same as tree construction algorithms, three consensus tree algorithms(Strict, Majority Rule and Adam Consensus) in pure python are also implemented in the `Bio.Phylo.Consensus` module. You can directly call `strict_consensus`, `majority_consensus` and `adam_consensus` to use these algorithms with a list of trees as the input. 

    >>> from Bio import Phylo
    >>> from Bio.Phylo.Consensus import *
    >>> trees = list(Phylo.parse('Tests/TreeConstruction/trees.tre', 'newick'))
    >>> strict_tree = strict_consensus(trees)
    >>> majority_tree = majority_consensus(trees, 0.5)
    >>> adam_tree = adam_consensus(trees)

Instead of using 50% as the cutoff, the `majority_consensus` method allows you to set your own one by providing an extra `cutoff` parameter(0~1, 0 by default). That means it can also work as strict consensus algorithm when the `cutoff` equals 1. One more thing different to strict and adam consensus tree, the result majority rule consensus tree has branch support value that are automatically assigned during calculation.  

### Bootstrap Methods

To get the consensus tree, we must construct a list of bootstrap replicate trees. So in the `Bio.Phylo.Consensus` module, we also provide several useful bootstrap methods to achieve this.

    >>> from Bio import Phylo
    >>> from Bio.Phylo.Consensus import *
    >>> msa = AlignIO.read('Tests/TreeConstruction/msa.phy', 'phylip')
    >>> msas = bootstrap(msa, 100)

As you see, the `bootstrap` method accepts a `MultipleSeqAlignment` object and generates its bootstrap replicate 100 times. The you can use them to build replicate trees. While, we also provide a convenient method to do this.

    >>> calculator = DistanceCalculator('blosum62')
    >>> constructor = DistanceTreeConstructor(calculator)
    >>> trees = bootstrap_trees(msa, 100, constructor)

This time we pass an extra `DistanceTreeConstructor` object to a `bootstrap_trees` method and finally got the replicate trees. Note that both `bootstrap` and `bootstrap_trees` are generator functions. You need to use `list()` function to turn the result into a list of alignment or trees.

Another useful function is `bootstrap_consensus`. By passing the consensus method as another extra parameter, we can directly get the consensus tree.

    >>> consensus_tree = bootstrap_consensus(msa, 100, constructor, majority_consensus)

### Branch Support

To get the branch support of a specific tree, we can use the `get_support` method.

    >>> from Bio import Phylo
    >>> from Bio.Phylo.Consensus import *
    >>> trees = list(Phylo.parse('Tests/TreeConstruction/trees.tre', 'newick'))
    >>> target_tree = trees[0]
    >>> support_tree = get_support(target_tree, trees)

In the above code, we use the first tree as the target tree that we want to calculate its branch support. The `get_support` method accepts the target tree and a list of trees, and returns a tree, with all internal clades assigned with branch support values.

## Other Useful Classes

There are some other classes in both `TreeConstruction` and `Consensus` module that are used in those algorithms. They may not be used commonly, but might be useful to you in some cases.

### Matrix.

The `Matrix` class in the `TreeConstruction` module is the super class of `DistanceMatrix`. They are both actually constructed by a list of names and a nested list of numbers in lower triangular matrix format. The only difference between them is that the diagonal elements in `DistanceMatrix` will all be 0 no matter what values are assigned.

To create a `Matrix` object:

    >>> from Bio.Phylo.TreeConstruction import Matrix
    >>> names = ['Alpha', 'Beta', 'Gamma', 'Delta']
    >>> matrix = [[0], [1, 0], [2, 3, 0], [4, 5, 6, 0]]
    >>> m = Matrix(names, matrix)
    >>> m
    Matrix(names=['Alpha', 'Beta', 'Gamma', 'Delta'], matrix=[[0], [1, 0], [2, 3, 0], [4, 5, 6, 0]])
    >>> print m
    Alpha	0
    Beta	1	0
    Gamma	2	3	0
    Delta	4	5	6	0
    	Alpha	Beta	Gamma	Delta

You can use two indices to get or assign an element in the matrix, and the indices are exchangeable.
    
    >>> m[1,2]
    3
    >>> m[2,1]
    3
    >>> m['Beta','Gamma']
    3
    >>> m['Beta','Gamma'] = 4
    >>> m['Beta','Gamma']
    4

Further more, you can use one index to get or assign a list of elements related to that index:
    
    >>> m[0]
    [0, 1, 2, 4]
    >>> m['Alpha']
    [0, 1, 2, 4]
    >>> m['Alpha'] = [0, 7, 8, 9]
    >>> m[0]
    [0, 7, 8, 9]
    >>> m[0,1]
    7

Also you can delete or insert a column&row of elements by index:

    >>> m
    Matrix(names=['Alpha', 'Beta', 'Gamma', 'Delta'], matrix=[[0], [7, 0], [8, 4, 0], [9, 5, 6, 0]])
    >>> del m['Alpha']
    >>> m
    Matrix(names=['Beta', 'Gamma', 'Delta'], matrix=[[0], [4, 0], [5, 6, 0]])
    >>> m.insert('Alpha', [0, 7, 8, 9] , 0)
    >>> m
    Matrix(names=['Alpha', 'Beta', 'Gamma', 'Delta'], matrix=[[0], [7, 0], [8, 4, 0], [9, 5, 6, 0]])

### BitString

`BitString` is an assistant class used frequently in the algorithms in the `Consensus` module. It's a sub-class of ``str`` object that only accepts two    characters('0' and '1'), with additional functions for binary-like     manipulation(&|^~). Common usage is as follows:

    >>> from Bio.Phylo.Consensus import BitString
    >>> bitstr1 = BitString('11111')
    >>> bitstr2 = BitString('11100')
    >>> bitstr3 = BitString('01101')
    >>> bitstr1
    BitString('11111')
    >>> bitstr2 & bitstr3
    BitString('01100')
    >>> bitstr2 | bitstr3
    BitString('11101')
    >>> bitstr2 ^ bitstr3
    BitString('10001')
    >>> bitstr2.index_one()
    [0, 1, 2]
    >>> bitstr3.index_one()
    [1, 2, 4]
    >>> bitstr3.index_zero()
    [0, 3]
    >>> bitstr1.contains(bitstr2)
    True
    >>> bitstr2.contains(bitstr3)
    False
    >>> bitstr2.independent(bitstr3)
    False
    >>> bitstr2.independent(bitstr4)
    True
    >>> bitstr1.iscompatible(bitstr2)
    True
    >>> bitstr2.iscompatible(bitstr3)
    False
    >>> bitstr2.iscompatible(bitstr4)
    True

In consensus and branch support algorithms, it is used to count and store the clades in multiple trees. During counting, the clades will be considered the same if their terminals(in terms of ``name`` attribute) are the same.

For example, let's say two trees are provided as below to search their strict consensus tree:
        
    tree1: (((A, B), C),(D, E))
    tree2: ((A, (B, C)),(D, E))

For both trees, a `BitString` object '11111' will represent their root clade. Each '1' stands for the terminal clade in the list [A, B, C, D, E] (the order might not be the same, it's determined by the ``get_terminal`` method of the first tree provided). For the clade ((A, B), C) in tree1 and (A, (B, C)) in tree2, they both can be represented by '11100'. Similarly, '11000' represents clade (A, B) in tree1, '01100' represents clade (B, C) in tree2, and '00011' represents clade (D, E) in both trees.

So, with the ``_count_clades`` function in this module, finally we can get the clade counts and their BitString representation as follows(the root and terminals are omitted):

    clade   BitString   count
    ABC     '11100'     2
    DE      '00011'     2
    AB      '11000'     1
    BC      '01100'     1

To get the BitString representation of a clade, we can use the following code snippet:

    # suppose we are provided with a tree list, the first thing to do is 
    # to get all the terminal names in the first tree
    term_names = [term.name for term in trees[0].get_terminals()]
    # for a specific clade in any of the tree, also get its terminal names
    clade_term_names = [term.name for term in clade.get_terminals()]
    # then create a boolean list 
    boolvals = [name in clade_term_names for name in term_names]
    # create the string version and pass it to BitString  
    bitstr = BitString(''.join(map(str, map(int, boolvals))))

To convert back:
        
    # get all the terminal clades of the first tree
    terms = [term for term in trees[0].get_terminals()]
    # get the index of terminal clades in bitstr
    index_list = bitstr.index_one()
    # get all terminal clades by index
    clade_terms = [terms[i] for i in index_list]
    # create a new calde and append all the terminal clades
    new_clade = BaseTree.Clade()
    new_clade.clades.extend(clade_terms)

This is how the `BitString` is used in the consensus and branch support algorithms. And I think it can be used in many other conditions. 

For example, we can even use it to check whether the structures of two trees are the same.

    # store and return all BitStrings
    def _bitstrs(tree):
        bitstrs = set()
        term_names = [term.name for term in tree.get_terminals()]
        term_names.sort()
        for clade in tree.get_nonterminals():
            clade_term_names = [term.name for term in clade.get_terminals()]
            boolvals = [name in clade_term_names for name in term_names]  
            bitstr = BitString(''.join(map(str, map(int, boolvals))))
            bitstrs.add(bitstr)
        return bitstrs
        
    def compare(tree1, tree2):
        term_names1 =  [term.name for term in tree1.get_terminals()]
        term_names2 =  [term.name for term in tree2.get_terminals()]
        # false if terminals are not the same 
        if set(term_names1) != set(term_names2):
            return False
        # true if BitStrings are the same
        if _bitstrs(tree1) == _bitstrs(tree2):
            return True
        else:
            return False

To use it:
    
    >>> tree1 = Phylo.read('Tests/TreeConstruction/upgma.tre', 'newick')
    >>> tree2 = Phylo.read('Tests/TreeConstruction/nj.tre', 'newick')
    >>> tree3 = Phylo.read('Tests/TreeConstruction/pars1.tre', 'newick')
    >>> compare(tree1, tree2)
    False
    >>> compare(tree1, tree3)
    True
    >>> compare(tree2, tree3)
    False