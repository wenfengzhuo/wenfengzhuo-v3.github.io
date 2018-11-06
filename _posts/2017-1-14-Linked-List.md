---
layout: post
comments: true
categories: [algorithms, data structures]
title: Linked List vs Array List
---
### Implementation

Both of Linked List and Array List serve as collections for storing a sequence of elements. The difference between these two lists is their internal implementation of the ways of storing objects.




For linked list, every objects stored in it are connected or linked together by maintaining pointers that point its predecessor and successor, so the objects will be represented as a node internally, which holds the information of the object itself, the predecessor pointer and the successor pointer. Adding a new element is as simple as adding a node at the tail of the list.

For array list, all objects will be stored in a pre-allocated array. The object will be directly added in the array without wrapping it as a node. The internal array could grow as more objects are added.

### Performance

With knowing their implementation, we could derive their performance clearly from operation perspective.

**Add**

For adding new object at the tail of the list, both of linked list and array list will have O(1) time for this operation. A difference is that if all slots in array list are exhausted, the list will grow to hold more objects, then a extra copy from the old array to the new array will be executed.  This will cost extra time compared with linked list, although this cost will be amortized to O(1).

For adding  new object in a position other than the tail of the list, the linked list will cost O(n/2) in worst case because it will first iterate the list to reach the node in the target position and then insert the added object by linking it. For array list, finding the position to insert the added object cost O(1) time, but there is an extra O(n) time in worst case to shift all object (to the right of the target position) to the right by one position.

**Deletion**

Linked list will cost O(n/2) time in worst case for delete a object if the target is in the middle of the list. In linked list, the deletion could be achieved by first finding that target node and then unlinking it from its predecessor and successor. Finding the object will cost O(n/2) time, and the unlinking will cost O(1) time.

Array list will only cost O(1) time to delete a target object, but it still requires extra time to shift all objects (to the right of the deleted object) to the left. If we delete the first object of the list in array list, then a copy of the rest of objects will be performed and this will nearly cost O(n) time (n is the length of the array). In total, the deletion will cost O(n) time for array list in worst case.

**Retrieve by index**

For linked list, retrieving an object by its index in the list will cost O(n/2) time as we need to iterate as most as half of the list to find the object if it resides in the middle of the list.

For array list, retrieving an object is as simple as retrieving the object in its internal array, so this will guarantee that it only costs O(1) time.

**Space**

Linked list will consume more space than array list as it has overhead to store the pointers, and specifically in Java, there will also be overhead for objects, because linked list will create a node object to represent each data item. Array list directly stores the original data item in an array which makes it consume less space.

### Application

With their differences in implementation and performance, linked list and array list will be used in different scenarios.

**Random Access**

If we want to random access to the objects in the list, array list will be a better choice because each retrieval operation will only cost O(1) time. A typical example is that when we want to use quick-sort to sort the objects in the memory, storing those objects in an array list will achieve high performance.

Even for sequential access, the array list will generally perform better than linked list thanks to the good locality of reference. A simple explanation of locality of reference is that the memory near the already accessed memory is tended to be cached so retrieving it will be faster than retrieving those in RAM.

**Density of Data**

If the data that is stored in a grid-way, or there are multiple dimensions to access the data, and the data is sparse distributed, then it is better to use linked list. A typical example is sparse matrix. Using array list would consume too much unnecessary memory in this case. Another example I could think up is the representation of a polynomial equation.

**Manipulating data**

If we want to insert or delete data during the iterator of the list, linked list will outperform than array list. This is because array list will cost extra time to move data when adding or deleting an element in a specific.

**access order**

A common requirement in practice is that we might need to maintain the access order of the objects, for example a LRU cache. Using linked list to implement this cache is an easy task. The recently accessed object could be put in the tail of the list, and the front object of the list will be the one which is least recently used, and if the cache reaches its capacity, we could evict the front objects. Both of those operations will cost only O(1) time. In Java, it is very quick to implement a LRU cache by inheriting LinkedHashMap and overriding its removeEldestEntry method.

**memory**

As the overhead of memory to store objects in linked list, it is always better to choose array list to store large amount of data in memory.

**Basic data structure**

Linked list usually is the choice to be used as the underlying data structures of queues and stacks thanks to its O(1) operations of adding and removing object at the head or tail of the list.
