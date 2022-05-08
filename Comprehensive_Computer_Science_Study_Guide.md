# This Is The Master Study Guide

## Design Patterns

A design pattern is a general and repeatable solution to a commonly occurring problem in software design. They can speed up the development process by providing a well regarded, testable, and preidentified solution to a problem that has been studied before. They provide general solutions instead of concrete implementations.

### Creational Design Patterns

#### Abstract Factory

The intent of the Abstract Factory is to provide an interface for creating families of related or dependent objects without specifying their concrete classes. To accomplish this, a hierarchy is specified that encapsulates many possible "platforms" and supports the construction of a suite of "products". The new operator is considered harmful.

If an application is to be portable, it needs to encapsulate platform dependencies. Some examples are windowing systems, operating systems, or databases. The "factory" object has the responsibility for providing the creation services for the entire platform family. The factory is asked by the client to create those resources. This makes exchanging product families, or resources, easy because the specific class of the factory object appears only once in the application. The Abstract Factory represents the Single Responsibility facet of SOLID software principles. It is often implemented as a singleton.

An example might be the organization of a machine that creates car parts. There is only one machine but it can make many parts. An abstract factory might be called **create** and expose **one method for each part**.

In an implementation, the **new** keyword is relegated to the factory and all concrete instances of the object in the client are created using the Factory.

#### Builder

The builder design pattern separates the construction of a complex object from its representation. This allows the construction process to create differing representations. The "director" invokes "builder" services as it interprets common external input. The "builder" creates parts of the complex object each time it is called and maintains the total state. Once the product is finished, the client can retrieve to complex object. This allows for fine control over the construction process. 

There are many types of situations where compositions are compound enough as to warrant extra control over their construction. This is useful for situations where the constructed object consists of disparate parts such as data from a database, data from a file, logging tasks, network calls, or heavy calculations.

Imagine a simple example of a fast food restaurant that builds combo meals. The **client** orders a kids meal such that the **director** can order the **builder** to build each part, the burger, the fries, the desert, and the drink, all resulting in a finished meal.

#### Factory

The factory design pattern is a lot like the Abstract Factory pattern but with less emphasis on building families of objects. Factories are again useful as a single source of truth for the creation of objects. However, Factories can also be used as useful abstractions such that concrete objects can be produced under a common framework and methodology. You can think of a Factory as a protected, parameterized, abstract constructor. Oftentimes, Factories are implemented as static resources.

Consider a framework that works with colors. One could create a Color factory and abstract away the details of creating the color from the caller and yet make it ultimately accessible to the entire application:

```Colors.makeRGB(int r, int g, int b); and Colors.makeHSB(float hue, float saturation, float brightness)```

#### Object Pool

Object pooling has two main advantages: resource containment and performance boosting. It is very effective in situations where the cost and rate of initializing a class is high, but there is no particular need to keep recreating them. Object pooling could be called object caching in some sense; they are otherwise known as resource pools. Pools should be implemented as self regulating; they should create new objects by themselves, know about how many items are in the pool, and have a universal policy for access and creation.

Object pools let other routines "check out" resources much as a library does books. If the resource is checked out, others can not use it. When they are no longer needed, the resource is returned to the pool. This policy makes the distribution of conserved resources such as threads easy and straightforward. Complex policies can be set up around pools such as scheduling so as to make the access policy more robust. It is also useful to limit memory overhead. Object pools are best implemented as Singletons.

#### Prototype

The Prototype pattern specifies the kinds of objects to create using a prototypical instance and creates these objects by copying that prototype. Initially, one instance of a class is created to be used as a breeder class for future instances. The new keyword is handled entirely in the prototype. The method is to declare an abstract base class the specifies a virtual clone method and contains a dictionary of all "clonable" concrete derived classes. Each of the derived classes implements the clone method which will copy itself and return the clone. To retrieve a particular class instance, an enum value is supplied to the clone method of the abstract base class, the class type is looked up in a table or switch, and the local instance of the class is cloned. In this way, the Prototype class operates as a factory for specific and related concrete classes. Prototypes are useful because they organize very specific instances of organizations of classes. An important distinction between a pure factory and a prototype is that factories create through inheritance and prototypes create through delegation. As such, Prototypes require an initialization function that initializes all the local concrete classes that the prototype specifies such that they can be cloned. Prototypes are useful when you want to avoid too much inheritance.

Take an example where we want to create models of types of cells to analyze. The prototype for all cells in the body is a cell prototype but to gain access to models, you would instantiate the types of cells that are able to be analyzed in the prototypical cell: Erythrocyte, Monocyte, Dendridic, etc. and pass in an enum type to the cell prototype's clone() method to produce the corresponding concrete class.

#### Singleton

Singletons ensure that a class has only one instance and is globally accessible to the rest of the application. This is often implemented as a static resource in most language. The key to Singletons is that the Singleton class is responsible for its own creation, initialization, and interface access. When a Singleton is to be used, it produces a new and unique instance of the class in question. One issue is that Singletons are hard to kill.

### Structural Patterns

#### Adapter

One should consider the Adapter pattern if the component offers compelling reasons for its reuse but its "view of the world" is not compatible with the philosophy and architecture of the system currently being developed. Reuse has always been a goal but is difficult to consistently achieve. **The Adapter pattern is about creating an intermediary interface between the old and the new system**. A very simple example might be to adapt a legacy component to a new one where the parameter ordering is different.

```A: rectangle.display(x1, x2, x3, x4)``` vs ```B: rectangle.display(x1, x2, h, w)```

The adapter would be built as a new interface that is expected by clients. If A is to be the standard, the old component would be wrapped by the Adapter object which would convert A's input to the the input of B, B would be called and it would be displayed.

#### Bridge

The Bridge design pattern is intended to split monolithic hierarchies into two orthogonal hierarchies, one for platform independent abstractions and one for platform dependent implementations. The interface contains a pointer to the abstract implementation class. This pointer is initialized with with an instance of a concrete implementation class but all subsequent interaction with the instantiated class is restricted to what is implemented in the base interface class which delegates to the implementation class or classes. The bridge class is useful for platform based development when there are more than one implementations that need to interface with other classes seamlessly. One great example is a threading architecture. An application may have a single interface for externalizing thread execution but it may need to interface with several OS threading architectures like the JVM, Unix, or Windows. The Bridge pattern is responsible for the instantiation of the proper platform specific implementation **within the platform dependent hierarchy**. The implementation independent architecture submits to the interface and the interface delegates to the concrete class behind it. 

The Bridge pattern serves to decouple the objects interfaces from one another, increase extensibility, and hides details from the client. The handle class is observed by the client and it handles the platform dependent implementation from there.

#### Composite

Sometimes a client needs to operate on hierarchical collections of types. The goal of the composite design pattern is to assimilate primitive and composites of those primitives into one interface such that they can be manipulated in a uniform way. Problems arise when you wish to control groups of primitive objects that depend on one another based upon some order. Rules about what to do when can explode and cause real problems. By implementing the Composite Design pattern you can effectively treat these operations recursively by organizing them in a tree structure. The Composite Design pattern has three main parts: The Component Interface, the primitive leaf object, and the Composite Object. The intent of the leaf primitive is that it can be any number of things as long as it has common control interfaces with other primitives in the hierarchy. The intent of the composite object is to contain groups of primitives that are related and should be treated in the same way.

The best example is a file system. The folders are a composite object and may have an ```open()``` function. The primitives could be the actual files in the folder, which could be of any type, yet also implement ```open()```. It is easy to see how composing primitives with common functionality can be easily manipulated with the Composite Design Pattern.

#### Decorator

The Decorator pattern is useful when you want to add behavior or state to individual objects at runtime. This is accomplished by continual wrapping of objects. This is powerful because it is immediate and dynamic with no inheritance required. See:

```Widget* aWidget = new BorderDecorator(  new HorizontalScrollBarDecorator(    new VerticalScrollBarDecorator(      new Window( 80, 24 )))); aWidget->draw();```

#### Fascade

The Fascade Design Pattern is intended to provide a unified interface to a set of interfaces in a subsystem. In short, you are wrapping complicated subsystems into a single easy to use system. This limits the clients ability to interfere with subsystems as well.

#### Flyweight

The Flyweight pattern uses sharing to support a large number of fine grained objects efficiently. Designing object to the lowest level of granularity can be a problem when it comes to memory or compute performance. Each Flyweight object is divided into two pieces: the **extrinsic, state-dependent part** and the **intrinsic, state independent part**. Extrinsic state is stored or computed by client objects and passed to the flyweight when its operations are invoked. Flyweights are stored in a Factory's repository where they can be requested by the client.

### Private Class Data

The Private Class Data pattern is very straightforward. You simply remove any setters, and set variables through the constructor.

#### Proxy

The problem that the Proxy Design Pattern is when you have a resource hungry object and you do not want to instantiate such objects unless and until they are actually requested by the client. Very simply, you can design a surrogate, or proxy, object that instantiates the real object the first time the client requests its functionality. In future, all requests are simply forwarded to the encapsulated real object. Proxies are useful in that they can also function as gate-guards to the real object for permission, authentication checking, and locking.

### Behavioral Patterns

#### Chain of Responsibility

The problem that the Chain of Responsibility Pattern solves is handling request streams while there are a potentially variable number of handlers, nodes, or processing objects. The pattern simply sends the request down the set of handlers until one is available to process it. It is essentially a pipeline abstraction.

#### Command

Simplistically, the Command design pattern decouples an object that invokes an operation from another that knows how to perform it. This encapsulates requests as objects which lets you parameterize clients with different requests. A Command class holds some subset of the following: an object, a method to be applied to the object, and the arguments to be passed when the method is applied. Calling execute() on the Command class brings the pieces together.

#### Mediator

The Mediator pattern helps in designing reusable components. Partitioning a system into many objects generally enhances reusability, but proliferating interconnections between those objects tends to reduce it again. The mediator object encapsulates all interconnections, acts as a hub of communication, is responsible for controlling and coordinating interactions between clients, and promotes loose coupling by keeping objects from referring to one another explicitly. A simple example might be that of different types of aircraft communicating with an ATC tower and their interactions are controlled through that mediation.

#### Memento

The Memento pattern captures and externalizes an objects internal state so that the object can be returned to this state later. A client can request a memento from the source object when it needs to checkpoint the source objects state. The client is then the care taker of that state but only the source object can store and retrieve the information from the memento.

The Memento pattern defines three roles:

1. Originator: the object that knows how to save itself.
2. Caretaker: the object that knows why and when the Originator needs to save and restore itself.
3. Memento: the lock box that is written and read by the Originator and shepherded by the Caretaker.

#### Null Object

Null checks can become tedious and burgeon into spaghetti code. The Null Object Pattern can help with this problem when there is a collaboration between objects. Instead of defining special cases to check and avoid operations on the collaborator object, a Null and opaque object is introduced that has no functionality at all. In this way, the caller can go on about its day by still calling the collaborator like it normally would but because the Null Object is now in the way, nothing happens. It is most easily implemented when the Null Object implements the same interface as the other collaborator type.

#### Observer

The Observer pattern defines a one-to-many dependency between objects such that when one object changes state, all of its dependents are notified and updated automatically. When the data model changes, it broadcasts its changes to all registered Observers that it has changed, and each Observer queries the data model for that subset of the Subjects state that it is responsible for monitoring. The most well know implementation of the Observer pattern is the V in MVC.

#### Template Method

The Template method has to do with Algorithms. You define the skeleton of an algorithm in an operation and define some steps to client subclasses. This lets subclasses redefine certain steps of an algorithm without changing the algorithms structure. Invariant components of an algorithm are defined in an abstract base class while the variant steps are given a default implementation or no implementation at all, such that a derived class can implement them concretely. 

## Lists

### Linked List

​	Linked lists are lists that are build be linking data together by reference. There is no backing array, only a HEAD link as a reference to the beginning of the list. List items are wrapped in a Node class that contains the data and a reference to the next node in the sequence.

```java
private class Node<T> {}
    private T data;
    private Node next;
}
```

Linked lists implement the same operations as an array based list: add, remove, replace, etc. but the implementations are slightly different. Adding and removing involves searching through the list to find the appropriate place to insert the node which is an O(n) task. However, adding and removing at the front of the List is O(1). Linked lists start at index 1 due to the HEAD link.

```java
public void add(int index, Node toAdd) {
    int count = 0;
    Node HEAD = this.head;
    while(count <= index && count <= this.length) {
        HEAD = HEAD.getNext();
    }
    //We've found the insertion spot
    if(count < this.length) {
		Node next = HEAD.getNext();
        toAdd.setNext(next);
        HEAD.setNext(toAdd);
    } else {
        return null;
    }
}
```

### Doubly Linked Lists

### Iterators

An iterator is an object that traverses a collection of data. While traversing, you may look, modify, add, or remove entries. Imagine that you are using your finger to look down a list of lines to consider them. This is what an iterator does with data. This is useful because an iterator can add, remove, and replace items <u>in place</u> instead of having to keep iterating over the data again and again.

A simple iterator interface:

```
public interface Interator<T> {
    
    public boolean hasNext();
    public T next();
    
    //other list operations to support adding and removal and so on
}
```

##### A useful iterator pattern

One pattern with iterators is to re-traverse a list to find the number of elements that are identical or meet some other criteria. Let's say you have a list of items and you would like to know how many times each unique element appears in the same list. You can use two iterators, with iterator A iterating where at each iteration iterator B iterates the entire list, incrementing the item being considered each time another instance of it appears. If you compile a list of unique entries at the same time then you can associate the appearance count as you go. What you end up with is a nice and easy way to avoid many traversals with convoluted and dense code.

### Circular Lists

Circular Lists are usually always linked. They are useful in modeling inherently "circular" domains. An example might be keeping track of whose turn it is in a boar game with more than two players. Instead of jumping back to the beginning of the list with indeces, we can just keep iterating forever. 

### Contiguous Lists

Lists can also be constructed using static contiguous structures as backing data organizers. An array is such an instance. There are marked differences in performance and considerations between contiguous lists and linked lists. When we use an array to represent a list, the indeces start at 0 just like the array. The elements are placed side by side for the length of the items in the list. This leads to an issue where a list could outgrow its backing array. When this happens, a new array of double the size of the old one must be created and the contents of the old array must be copied to the new array. 

Performance considerations:

- Access is O(1) because we have direct addresses to the items in the array.
- Addition and removal are O(N) because the backing array must be reallocated to shift the elements back into their places.
- Memory Overhead: ArrayLists have unused memory in the form of the unused array spaces at the end of the array.

### Sorted Lists

Sorted lists have special properties that make them the best implementation in general for ordinal collections. Provided the items are sorted, binary search - like techniques can be used to add, remove, get, and check containment in O(log(n)) time. Better still, a fast sorting algorithm can be used to order an unsorted list in as little as O(n log(n)) time, making the strategy of sort, then search a very effective one. See Merge Sort and Binary Search for more implementation details. In order for sorted lists to be quickest, they should be linked lists because otherwise add and remove degenerates to O(n) due to array reallocation.

## Maps

#### Contiguous Sorted and Unsorted Maps

The primary operations on a contiguous map are Search, Insert, and Delete. The efficiencies for each operation are as follows:

1. Operation: Unsorted:  ,    Sorted
2. Search:        O(n)            O(log n) 
3. Insert:           O(1)            O(n)
4. Delete:          O(1)            O(n)

However, when a map is sorted, Insert and Delete degenerate to O(n) because you may have to insert elements within an array and move others to make room.

#### Hash Maps

Hash maps implement an associative array data type that maps keys to values. A hash map uses a hash function to compute an index into an array from which the desired value can be found. However, most all hash functions are imperfect and thus can cause collisions in operations. In this case, there are several techniques that can be employed to handle this. The primary technique is that of Separate Chaining with linked lists where each slot in the hash table is the head of a linked list and when collisions happen, you just place the colliding element into the list. This degenerates insertion, deletion, and removal to O(n) to search the entire array.

A good hash function and implementation should have the basic requirement that the resultant indeces should have a uniform distribution of hashes. This is why hash functions are often based off of the size of the table itself such that a function can be easily chosen to base the function off of. Having a function be evenly distributed over powers of two is much easier than having it be evenly distributed over odd numbered sizes.

The efficiencies for a Hash Table are as follows:

Algorithm      Average     Worst Case

Space              O(n)               O(n)

Search             O(1)               O(n)

Insert                O(1)               O(n)

Delete               O(1)               0(n)

Again, Hash tables are incredibly useful for static data, especially when memory is not an issue, when the size of the table is known ahead of time, and when the number of entries is large. However, when the table is small and the entires high, the likelihood of O(n) lookup times shoots up and the performance degenerates to that more closely resembling unsorted lists. Dynamic resizing can also be an issue. This is why large hash tables are preferred over small ones: to avoid frequent resizing. 

#### Multimaps

Multimaps are a special implementation of a Map where single keys can be associated with multiple values. In practice this simply looks like:

​	MultiMap<X, Collection<Y>> 

It is very useful when you want to associate many values with a key through the *interface* of a Map. When you call get() on the multimap, you get the Set of associated values back from the Map.

## Stacks

#### Implementation

A stack is a LIFO or *Last in First Out* container data structure. Stacks are simple to implement and very efficient. In general, stacks are the right data structure to use when retrieval order does not matter. Stacks are useful in simulating recursion to avoid stack overflow.

The two stack operations are:

 	1. Push - pushes the new item onto the top of the stack
		2. Pop - takes the first item off the top of the stack

Stacks are generally implemented in singly linked lists.

## Queues

### Implementation

A queue is a FIFO or *First in First Out* container data structure. Queues are used to process data in a particular order. Queues are often used in polling operations, message passing programs, and schedule management.

The two queue operations are:

1. Enqueue - adds an element to the back of the queue
2. Dequeue - removes the element at the front of the queue.

### Dequeues

Dequeues are also known as double-ended queues. In a dequeue, both sides of the queue can act as the head or the tail and support both the enqueue and dequeue operations. Note that because the Dequeue exhibits properties from both a queue and a stack, ordering is up to the programmer to maintain. Dequeues can be implemented using either a doubly linked list or a circular array such that O(1) removal and addition can be achieved. 

Dequeues can be used in parity matching and interprocessor job scheduling to great effect.

### Priority Queues

Priority queues are extremely important data structures. They can be efficiently implemented using Heaps. Priority queues act just like normal queues but with the special quality that when an item is inserted into the queue, it is shifted inside the queue to some position based upon some numerical priority. High priority items end up closer to the front of the queue where they will be processed faster and visa versa for lower priority items. It is common practice to implement a comparator for a complex type that is entered into the queue:

A Comparator uses positive, negative, or zero values to direct the user of the comparator where to place the comparable item:

```
public class PlayerComparator implements Comparator<Player> {
    @Override
    public int compare(Player a, Player b){
        return (a.ranking - b.ranking);
    }
}
```

Another way to implement comparison is within the comparable class:

```
public class Player implements Comparable<Player> {
    @Override
    public int compareTo(Player other) {
        return (this.ranking - other.ranking);
    }
}
```

Efficiency concerns change when a queue becomes a priority queue. If you were to enqueue a new element into a priority queue, you would have to start at the back of the queue and iterate over its elements to find the spot in which the new element belong, potentially reaching the front. This is an O(n) operation. However, if the priority queue is implemented with a heap, the insertion becomes O(log n) because of how elements are inserted into heaps. However, this will always mean that removal of the max or min prioritized value is O(1).

Priority queues have many uses including OS scheduling, Djikstra's, Prims, Huffman Codes, A * Search, and Heap Sort.

### Heaps

A heap is a specialized binary tree data structure that specifies the heap properties:

1. If it is a min heap, each parent is less than both of its child nodes
2. If it is a max heap, each parent is greater than both of its child nodes.

The basic and most useful operation of the heap is the findMin or findMax data structure because it is O(1). Heaps are in sorted order and thus can be treated as priority queues.

#### Heap Implementation

Heaps can be implemented in a very space efficient way with arrays. If the min or max is stored at the 0th index then its left child will be stored at index 1 and its right child will be stored at index 2. This can be generalized as **for any node at index n, its left child will be stored at index *2n + 1* and its right child will be stored at *2n + 2***. 

##### Heap Insertion

Insertion is done as follows: The new node is placed at the end of the array, and iteratively swapped with its parent until the heap property is restored.

##### DeleteMin

The minimum element in the heap is found at the root of the tree. To delete it, first remove it and replace it with the last element in the heap. Then take that root and iteratively swap it with the relevant child until the heap property is restored.

##### Heap Efficiencies

findMin         deleteMin        insert         merge

  O(1)             O(log n)         O(log n)         O(n)

## Trees

### Traversals

#### Inorder Traversal

An inorder traversal is a traversal that traverses the entire left subtree of each node before traversing any right subtree. In practice this just looks like visiting the left most node in each subtree before visiting the right side.

For a binary tree:

```
public List<Node> inOrderTraverse(Node treeNode, List<Node> collector){
	if(treeNode == null) {
        return collector;
	}

    if(treeNode.left != null) {
        inOrderTraverse(treeNode.left, collector);
    }
    
    collector.add(treeNode);
    
    if(treeNode.right != null) {
        inOrderTraverse(treeNode.right, collector);
    } 
    
    return collector;
}
```

#### Preorder Traversal

A preorder traversal is a traversal that visits each root, then visits the left subtree and then the right subtree in that order.

For a binary tree:

```
public List<Node> preOrderTraverse(Node treeNode, List<Node> collector) {
    if(treeNode == null){
        return collector;
    }
    
    collector.add(treeNode);
    
    if(treeNode.left != null) {
        preOrderTraverse(treeNode.left, collector);
    }
    
    if(treeNode.right != null) {
        preOrderTraverse(treeNode.right, collector);
    } 
    
    return collector;
}
```

#### Postorder Traversal

A postorder traversal is a traversal that visits both subtrees before the root.

For a binary tree:

```
public List<Node> preOrderTraverse(Node treeNode, List<Node> collector) {
    if(treeNode == null){
        return collector;
    }
    
    if(treeNode.left != null) {
        preOrderTraverse(treeNode.left, collector);
    }
    
    if(treeNode.right != null) {
        preOrderTraverse(treeNode.right, collector);
    } 
    
    collector.add(treeNode);
    
    return collector;
}
```



## Binary Search Trees

A Binary Search Tree, or BST is a tree that meets the following conditions:

1. The tree has a single root node
2. Each node may have at max two children, a left node and a right node
3. Each node in the tree has a value that is comparable
4. Each left node is lesser in value than its parent node
5. Each right node is greater in value than its parent node

The basic operations supported by binary trees are:

1. Search
2. Traversal
3. Insertion
4. Deletion

#### Searching a Binary Tree

Searching a binary tree always starts at the root. If the search key is the value at the node being considered, return it. Otherwise, if the value is lesser than the current node then proceed down the left side of the tree. If the value of the search key is greater than the value of the current node then proceed down the right side of the tree

Search in O(h) time, where h is the height of the tree:

```
public Node search(Node n, Item key){
    if(n == null) {
        return null;
    }
    
    if(n.value == key){
        return n;
    }
    
    if(n.value < key){
        search(n.left, key);
    } else {
        search(n.right, key);
    }
}
```

#### Finding the Minimum or Maximum Element in the Tree

By definition, the minimum valued node in the tree lies at the leftmost node in the tree. The same is true for the maximally valued node but it lies at the rightmost node in the tree.

Find the minimum in a BST:

```
public Node findMin(Node n) {
    if(n == null) {
        return null;
    }
    
    Node min = n;
    while(min.left != null) {
        min = min.left;
    }
    
    return min;
}
```

#### Tree Traversal

Binary trees make it easy to list their items with a simple algorithm. If you use an in-order traversal, the values in the traversal will be returned in sorted order

An O(n) in-order tree traversal:

```
void List<Node> traverse(Node n, List<Node> inOrder) {
    if(n != null) {
        traverse(n.left);
        inOrder.add(n);
        traverse(n.right);
    }
    
    return inOrder;
}
```

#### Insertion and Deletion

Insertion is very straightforward: recursively traverse until you run into a NULL value. This is the place for the new Node. Simply make the parent of the Null the parent of the new Node.

Deletion is a little trickier. There are three cases for deletion. If the deleted Node was a leaf, simply clearing the reference to it will do. If the Node to be deleted has a single child, all that needs to be done is to link the child of the doomed Node to the doomed Node's parent. This maintains the ordering. If there are two children, things get a little more complicated. The immediate successor should be the leftmost node of the right subtree. Then the children must be rearranged to accommodate the structure. A simple answer to this problem is to remove the doomed Node and reinsert all of its children into the top of the BST. Problem solved.

### Binary Search Tree Performance

Binary trees have a special property that makes them efficient to search. Their height h will only ever be [log n], thus making search O(log n). This is very fast. However, this property does not always hold true. It is directly dependent on the order in which the elements are inserted. The most degenerated BST is one where the elements were inserted in sorted order, which means that each Node has only one child. This makes the height h equal to n. Thus in the worst case, search is O(n). This is solved by balancing the tree, which is a whole topic unto itself.

## Red Black Trees

Red Black trees are a kind of self balancing binary search tree. Each node carries an extra bit to affect the ordering of nodes in the tree such that the tree remains at approximately the same height.

The properties of a Red-Black tree are:

1. Each node is either red or black
2. The root is black
3. All leaves NIL are black. NIL is used to represent second class non Node leaves
4. If a node is red then both of its children are black
5. Every path from a given node to any of its descendant NIL nodes contains the same number of black nodes

Red Black trees are the fastest and most efficient of trees. Space is O(n) and Search, Insert, and Delete are O(log n) in the worst case.

## General Knowledge

#### Infix, Postfix, and Prefix Expressions

These are three different but equivalent ways of writing expressions:

1. Infix Notation:      X + Y
2. Prefix Notation:   + X  Y
3. Postfix Notation:  X  Y +

### Program Stack and Heap





