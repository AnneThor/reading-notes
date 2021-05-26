# Hash Tables

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

- [Video: Paul Programming Hash Table](#hash-table-overview)
- [Codefellows: Hash Table Overview](#hash-table-notes)
- [Hacker Earth: Basics of Hash Tables](#hash-table-basics)
- [Wikipedia: Hash Table](#wikipedia-hast-table)


### **[Hash Table Overview](https://www.youtube.com/watch?v=MfhjkfocRR0)**

Implementation of an associative array. It can be thought of like an array of buckets, generally hash tables are very large (long).

Data in the hash table is in key/value form.

There is a hash function that will take the key and return a hash value. The hash function has to be a pure function, it must return the same value for the key every time that it is entered.

Hash(key) => index number

The buckets can have different implementations to avoid COLLISIONS. It can be a linked list, or a subarray. This is called CHAINING.

To retrieve the data later on, you just need to provide the KEY to the HASH FUNCTION and that will return the index where the data is located, then you will need to peruse the bucket by whatever method to find the specific piece.

A well implemented hash table has shallow buckets.

### **[Hash Table Notes](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-30/resources/Hashtables.html)**

Term | Definition
---- | ----------
Hash | a function that takes an incoming string and converts it to another value (must be a pure function so that there is a 1:1 relationship between inputs and outputs)
Buckets | a data structure that exists at each index of the hashtable (able to handle collisions)
Collision | what happens when multiple keys get hashed to the same value

#### Basic Hashtable Structure

Hashtables contain key/value pairs. Basic idea is to make them quick to sort and store. This is theoretically an O(1) time complexity to get the hash and look at that index of the hashtable.

The hash function takes a string and returns an integer that corresponds to an existing array index in the storage array. The piece of data is then placed at that index and the same function can be used to store/retrieve it later.
- Must be deterministic (output determined only by input)
- Should have some random aspect
- Key should always produce same hash

Hash tables have a load factor that can be calculated (and they can have internal functions to grow accordingly when they tip the load factor)

Internal Methods:
- hashFunction
- get
- contains
- set

### **[Hash Table Basics](https://www.hackerearth.com/practice/data-structures/hash-tables/basics-of-hash-tables/tutorial/)**

Tips for hash functions
1. Be simple and not become an algorithm in and of itself
2. Provide uniform distribution
3. Minimize collisions

Recommended to use prime numbers for the modulo to minimize collisions

*Separate Chaining* is the term for putting linked lists in buckets.
*Linear Probing* is a technique that avoids collisions by searching from 0 ---> through the index arrays until it finds an empty bucket (can be a stepped search approach)

### **[Wikipedia Hash Table](https://en.wikipedia.org/wiki/Hash_table)

Data structure that implements an associative array abstract data type 
