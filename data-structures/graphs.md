# Hash Tables

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

### [Codefellows: Graphs](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/graphs.html)

Graphs are non linear data structures that have:

- Vertices: nodes
- Edges: connection between nodes
- Neighbor: adjacent nodes (connected via an edge)
- Degree: the number of edges connected to a vertex

**Undirected Graphs** have undirected or bidirectional edges (meaning they are doubly linked edges)

**Directed Graphs** also called a **Digraph** where each edge has a direction (can be doubly linked but can also be singly linked)

**Complete Graphs** every vertex is connected to every other vertex

**Connected Graphs** all vertices have at least one edge (a tree is a form of a connected graph)

**Disconnected Graphs** vertices are not required to have edges

**Acyclic Graphs** are just that, acyclic so you cannot traverse back to the beginning. Trees are *(directed acyclic graphs or DAGS*).

**Cyclic Graphs** start and end at the same vertex

### Graph representation

We consider **Adjacency Matrix** and **Adjacency List**

#### Adjacency Matrix
Represented as a 2 dimensional array, if there are n vertices we have an n x n matrix
- Each row and column represent a vertex of the data structure
- Elements of the column and row must add up to 1 if there is an edge connecting them or zero if no connection
- Matrices are always symmetrical for undirected graphs (can be or not be for directed graphs)
- Sparse graphs have few connections, dense graphs have many

#### Adjacency List
Most common way to represent graphs - that is a list of linked lists or arrays that contain the directed edges from each vertex
- You can build this with a collection of arrays or linked lists
- Every time you add an edge you will find the appropriate vertices in the collection and add to the appropriate location

#### Weighted Graphs
Weighted graphs have numbers assigned to their edges
- In a matrix, this is done by assigning the actual weight of the edge instead of 0 or 1 in the matrix (some people use inifinity instead of zero in this case)
- In an adjacency list, you just add a tuple

### Traversals
Do this similarly to the way you traverse a tree

#### Breadth First
You start at a set point
- Keep a record of visited vertices to avoid infinite looping if the structure is Cyclic

Basic Steps to Implement
- Enqueue the start node into an empty queue
- Create a loop that runs while there is something in the queue
- Dequeue the first node and enqueue it's children
- **when you add to the queue just make sure that you're not adding something that is already contained in the "visited" set**

Notes: If there are disconnected nodes, they will not be visited; only outputs nodes that are in some way connected to the vertex that you supply

#### Depth First
Uses a stack similarly to how the depth first uses a queue

Basic Steps to Implement
- Push the root node into a stack
- Start a while loop while the stack is not empty
- Peek at the top node in the stack
- If the top node has unvisited children, mark the top node as visited and push the unvisited children back on the stack
- If the top node does not have unvisited children pop it off the stack
- Repeat until stack is emptied

### Applications of Graphs

- GPS and Mapping
- Driving Directions
- Social Networks
- Airline Traffic
- Netflix (product suggestions)
