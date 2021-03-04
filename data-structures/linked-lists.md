# Linked Lists

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [CodeFellows: Linked Lists Overview](#code-fellows-linked-lists)
- [Medium: What's A Linked List Anyways](#medium-linked-lists)
- [Medium: What's A Linked List Pt.2](#medium-part-two)


### [CodeFellows Linked Lists](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-05/resources/singly_linked_list.html)

A linked list is a series of nodes with one directional pointers to other nodes:

- Singly Linked Lists: have only one pointer, to another node, can have a relationship with maximum 1 other node
- Doubly Linked Lists: point forward and backwards, therefore can have relationships with maximum 2 other nodes (previous/next)
- Nodes: contain some kind of data as well as their reference(s) to other nodes
- ```Next```: each contains this property which points to the next node
- ```Head```: reference name for the first node in a linked list
- ```Current```: reference to the Node that is currently examined during traversal

#### Node Traversal

Cannot use regular array methods, must be done exclusively with the next/previous references.
```While``` loops are the useful way to manage linked lists so that you can continuously check that the next node is not null (if it is null it will through an error that will stop your program):
```while (current) { do something }```

#### Big O Considerations

Traversing a linked list has worse case scenario of O(n), where whatever you're looking for is the very furthest node from the head; in terms of space, traversing a list doesn't require external references so O(1)

#### Adding a Node to the Beginning

1. Set ```Current = Head``` to ensure you start at beginning of list
2. Instantiate a new ```Node``` and set the new Node's ```next = Head```
3. Now you can reassign ```Head = newNode``` and ```Current = newNode```

No matter how big the list gets, it's always the same operational cost to add a new node to the beginning O(1).

#### Adding a Node to the nth Location

1. Set ```Current = Head``` to ensure you start at beginning of list
2. Instantiate your newNode, it will just exist with Next = null for now
3. Traverse until you get to the point where you want to add the node, call is nodeBreak 
4. Set ```newNode.next = nodeBreak.next``, now they both point to the same location 
5. Insert the new node by redirecting the break point, ```nodeBreak.next = newNode```
6. Now you're done assuming it's singly linked

Your outcomes here are O(n), that you add to the very end of the list, and space O(1) since you're not adding anything additionally to the input

#### Print the Nodes in a Linked List

Make a ```while()``` loop and print out whatever contents you while while the node is not null

#### Considerations

1. When making the Node class, consider a check requiring a value is passed to avoid empty nodes 
2. You may want to require at least 1 node is passed in when instantiating the class so you always have a HEAD/CURRENT node

### [Medium Linked Lists](https://medium.com/basecs/whats-a-linked-list-anyway-part-1-d8b7e6508b9d)

Linear data structure (like arrays), but they're accessed and traversed differently.

Memory allocation is a differentiator: arrays are indexed and require to be stored in the same place - a contiguous block of memory; linked lists are just containing references to one another so they can be stored apart from one another

Array = static data structure (always needs a fixed amount of memory)
Linked list = dynamic data structure (can shrinka nd grow, can be allocated in different locations)

Starts with the head and ends with a null; linked lists require much less memory because each individual piece knows nothing about the other pieces, just contains a reference pointer to 1-2

Circular linked lists exist (idea being that the tail replaces teh head and the first and last element point to one another)

### [Medium Part Two](https://medium.com/basecs/whats-a-linked-list-anyway-part-2-131d96f71996)

There are two considerations for choosing a data structure:

1. How much time they require at runtime
2. How much time/memory it needs

Many factors affect the actual performance of an algorithm (speed of processor, background processes, etc, so we use Big O to describe a theoretical scenario of how quickly RUNTIME grows)

Linked Lists:

- O(1) = constant time, what it means is no matter how big your list becomes, it will take the same amount of time to add to it
- O(n) = linear time, meaning that the traversals, etc. grow in proportion to the size of the list in a linear fashion

You don't have to preallocate memory space when you're adding/removing from the linked lists

- Adding to beginning of linked list is O(1)
- Adding to end of linked list is O(n)
- Still they are both linear

When to use them? They are good for adding/removign but traversing can be expensive

Arrays | Linked Lists
------ | -------------
Inserting/deleting can be slow | Inserting/deleting is fast
Searching is fast | Searching depends on size (likely slow)
Static/fixed size, not as easy to change size | Dynamic size, easily changeable
Allocates memory in contiguous chunk when created | Only allocates resources as needed
Finding elements is fast and binary search is possible | Finding requires traversal, binary search impossible
Helpful if you know the size of the list, need random access to elements, want to iterate quickly | Helpful if you do not know the size of the list, and mostly want to add/remove quickly without random access (so smaller problem set basically)