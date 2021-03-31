# Trees

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [CodeFellows: Trees Overview](#code-fellows-trees)

### [Code Fellows Trees](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-15/resources/Trees.html)

#### Tree Terms 

Term | Definition
---- | ----------
Node | Component containing value, reference(s) to other nodes
Root | The beginning node of the tree
K | Number indicating max number of children nodes in the tree 
Left | Ref to one child node in binary tree
Right | Ref to one child node in a binary tree
Edge | Link between parent/child node
Leaf | A terminal node with no children (children can be added here)
Height | Number of levels present in the tree below root (depth); number of edges from root to furthest leaf

#### Depth Vs Breadth Search

**Depth First Search**
Prioritize going through the depth of the tree first; there are multiple ways to organize this search method and they are named in reference to the root node

- Pre: ```root --> left --> right```
- In order: ```left --> root --> right```
- Post order: ```left --> right --> root```

Recursion is used to traverse trees utilizing the call stack

**Breadth First Search**
Goes through the tree level by level; traditionally this process uses a queue instead of recursion

The idea is as follows:

- Enqueue the root node, then dequeue it
- When you dequeue a node, enqueue it's children left to right
- Then dequeue the nodes and enqueue their children until you reach leaves


#### Binary Trees 

Just remember that by default binary trees are not sorted or balanced, you have to add those things onto them

**Adding a node** there are no rules in a general binary tree about where to place; one straegy is to fill child spots top-down using breadth first travelersal (i.e. insert to the first parent node that does not have full children).

#### K-ary Trees

K-ary trees just have a value of child nodes > 2; you can use the same breadth first searching technique (using the queue) but you just have to account for the actual number of child nodes present in each node that you enqueue when you dequeue it. You can do that using ```for ... in``` or similar logic.

#### Binary Search Trees (BST)

Organzed s.t. values that are larger are placed to the right of the parent node; values that are smaller are placed to the left of the parent

#### Searching a BST

This is much faster bc you can use comparisons to root node values; best way to approach this search is using a ```while``` loop to cycle until you hit either the value you want or a leaf node


#### Big O Notations

**Big O for adding node** is ```O(n)```

**Big O for searching node** is ```O(n)```

**Big O for inserting node breadth first** is ```O(w)``` (w is widest level)

A "perfect" binarty tree is completely full and its's max width is ```2^(h-1)```, where ```h``` is the height; height is ```log n``` where ```n``` is number of nodes. In an unbalanced tree, the worst case height of the tree is ```n``` (i.e. linked list)

**Big O for inserting into BST** is ```O(h)```; worst case you search down to a leaf

**Big O space complexity of BST search is O(1)** searching does not allocate additional space
