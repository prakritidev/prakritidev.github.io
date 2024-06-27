---
title: '"Java - Collections (Basics)"'
draft: 
tags:
  - java
  - collections
  - streams
  - parallelProcessing
  - API
---
## How things are related in Collections?

Iterable <- Collection <-  List
                        <- Queue
                        <- Set

List -> ArayList, LinkedList, Vector <- Stack
Set -> HashSet, LinkedHashSet
SortedSet -> TreeSet


# What are collections where can we use them in real life (Doing small stuff)

Collections are a represents a groups of same objects. python has list, c++ has vector ...etc. 

## How to do processing on collections

#### How to traverse over collections ? 

```
ArrayList<String> myCollection = new ArrayList<>();  
names.add("Bleh");  
names.add("Mary");  
names.add("Bleh");
```

- For Each
```
for (String x: myCollection) {
	System.out.println(x);
}
```
- Iterator() 
```
for (Iterator<?> it = myCollection.iterator(); it.hasNext(); ){
	System.out.println(it.next());
}
```

## Collection Interface Bulk Operations

_Bulk operations_ perform an operation on an entire `Collection`. You could implement these shorthand operations using the basic operations, though in most cases such implementations would be less efficient. The following are the bulk operations:

- `containsAll` — returns `true` if the target `Collection` contains all of the elements in the specified `Collection`.
- `addAll` — adds all of the elements in the specified `Collection` to the target `Collection`.
- `removeAll` — removes from the target `Collection` all of its elements that are also contained in the specified `Collection`.
- `retainAll` — removes from the target `Collection` all its elements that are _not_ also contained in the specified `Collection`. That is, it retains only those elements in the target `Collection` that are also contained in the specified `Collection`.
- `clear` — removes all elements from the `Collection`.

