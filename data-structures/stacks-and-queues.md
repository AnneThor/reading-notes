# Stacks and Queues

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Stacks

A stack is a data structure comprised of a series of nodes with a reference pointing to the "next" node (no prev pointing reference). 

### Common Methods for **Stacks** include

Method | Complexity
------ | ----------
```push(value)``` create node with new value, add it to top of stack | O(1), always the same, doesn't depend on size of stack
```pop()``` removes and returns a node from the stack (popping an empty stack throws an exception) | O(1)
```top``` reference to top of stack | n/a
```peek()``` returns value of the top node (throws exception when peeking an empty stack) | O(1), remember to call ```isEmpty()``` first
```isEmpty()``` returns boolean of whether stack is empty | O(1)

Stacks are like a stack of plates

- FILO and LIFO: first item in will be the last item out
- LIFO: last item in will be first item out

## Queues

Queues are like waiting in line and you add to the back (```rear```) and remove from the ```front```

### Common Methods for **Stacks** include

Method | Complexity
------ | ----------
```enqueue(value)``` create node with input value, add to back of queue | O(1) does not depend on size of existing queue
```dequeue()``` removes item from front of queue, throws exception if empty | O(1), remember to call ```isEmpty()``` first
```front()``` reference to front node of queue | n/a
```rear()``` reference to last node in the queue | n/a
```peek()``` returns value of ```front``` node | O(1)
```isEmpty()``` returns boolean of if queue is empty | O(1)

Queues are

- FIFO: first item in will be the first item out
- LILO: last item in will be the last item out 
