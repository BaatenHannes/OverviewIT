## Data structures

### LinkedList vs Array

#### Array

An array has a fixed size and is stored in a continuous memory location. This means that, since we know the exact size of the array, we can address consecutive memory adresses to each of the elements.

```
int array[3] = {1,2,3};
```

This will allocate next example addresses:

Address 14: value 1,
Address 15: value 2,
Address 16: value 3

##### Advantage - fast speed of access

As a result, on knowing the index of the element, each element in an array can be accessed at the same speed. You can just add the offset of the index to the address of the first element. There is no time difference between accessing the first or the 3th element.
This results in a big O notation of O(1) - which means a constant search time for each element.

##### Disadvantage - slow speed of insertion/deletion

Inserting or deleting items in the middle of the array, means you need to move every other element to another memory address. This means a O(n) or lineair speed (meaning 1 time extra for 1 element extra), since n-elements need to be shifted.


#### Linked List

![LinkedList](\img\LinkedList.png)

Linked lists are not fixed size and are not stored in a continuous memory location. They consist of a list of nodes that each contains data and the address of the next node.
Each node as such is stored in at least to consecutive memory addresses.

![LinkedListInMemory](\img\LinkedListInMemory.PNG)

##### Advantage - fast speed insertion/deletion - no need to predefine memory allocation

Since we only need to change at max 3 nodes upon insertion or deletion of an element, the speed is O(1) at start or end of the list. However at the middle, it is O(n), since we need to iterate through all the elements to get to the middle of the list.

##### Disadvantage - slow search time

The more nodes, the longer the search time since we need to iterate through every element to get to the next one. Big O of O(n)


-------- IMPORTANT ----------

.NET does not expose any singly-linked list type. List will use an array internally and just copy all variables to a new array when it needs to grow. LinkedList does exist but is a double-linked list.



### .NET Collections

![Collections](\img\collections.png)