---
title:  "Performance of lists in Java"
date: "2019-05-12"
excerpt: "Comparing performances of different list data structures in Java"
---
# Introduction
In this post I will compare performances of 3 commonly used list data structure in java -- Linked List, Array List, Vector.
# Java List<E> interface
The [List&lt;E&gt;](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) interface is a member of Java Collections framework. It is an ordered collection of objects which provides user to control where in the list each element is inserted. There are multiple implementations of List<E> interface but we will focus on [ArrayList&lt;E&gt;](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), [LinkedList&lt;E&gt;](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) and [Vector&lt;E&gt;](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html).
# Setup
I am running my code in a ```MacBook Pro (Retina, 13-inch, Mid 2014)```, ```2.6 GHz Intel Core i5``` and ```8 GB 1600 MHz DDR3``` RAM. I am using IntelliJ IDE to run and debug my Java code. We will compare add, remove and sort operations since they are the most common operations performed on a list.

For the purpose of this experimentation, we assume that ```Math.random()``` takes constant time and use it to generate integers to be added to the list.

```java
    public static Integer LIMIT = 100000;
    private static void addNumbersToList(List<Integer> list) {
        for(int i = 0; i < LIMIT; i++) {
            list.add(new Integer((int)Math.random()));
        }
    }
```

We also use ```Math.random()``` to generate integers to be removed from the list. This also emulates the real world scenario where the element to be removed from the list is not in the list (The worst case scenario for removal of an element from a list).

```java
    public static Integer LIMIT = 100000;
    private static void removeNumbersFromList(List<Integer> list) {
        for(int i = 0; i < LIMIT; i++) {
            list.remove(new Integer((int)Math.random()));
        }
    }
```

For sorting we would use [List.sort(Comparator<? super E> c)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#sort-java.util.Comparator-) method. Following is the driver code

```java
    // Create 3 lists
    List<Integer> linkedList = new LinkedList<>();
    List<Integer> arrayList = new ArrayList<>();
    List<Integer> vector = new Vector<>();

    // Add elements for lists
    long start = System.currentTimeMillis();
    addNumbersToList(linkedList);
    long end = System.currentTimeMillis();

    start = System.currentTimeMillis();
    addNumbersToList(arrayList);
    end = System.currentTimeMillis();

    start = System.currentTimeMillis();
    addNumbersToList(vector);
    end = System.currentTimeMillis();

    // sort list elements
    start = System.currentTimeMillis();
    linkedList.sort(Integer::compareTo);
    end = System.currentTimeMillis();

    start = System.currentTimeMillis();
    arrayList.sort(Integer::compareTo);
    end = System.currentTimeMillis();

    start = System.currentTimeMillis();
    vector.sort(Integer::compareTo);
    end = System.currentTimeMillis();

    // remove elements from the list
    start = System.currentTimeMillis();
    removeNumbersFromList(linkedList);
    end = System.currentTimeMillis();

    start = System.currentTimeMillis();
    removeNumbersFromList(arrayList);
    end = System.currentTimeMillis();

    start = System.currentTimeMillis();
    removeNumbersFromList(vector);
    end = System.currentTimeMillis();
```

Since my java code isn't the only process running on my computer, we run it 3 times and do the final comparisons

# Comparison
### Add operation
ArrayList seems to be the winner in add operation. While adding in linked list involves traversing to the end then adding the element, in arraylist it is just getting the last index and it in array almost constant time unless when the array needs resizing.

<img src="img/addOperation.png" align="middle" height="400" />

### Remove operation
Linked list is a clear winner by wide margins here. Remove operation for linked list involves searching the element in list ```O(n)``` and manipulating pointers to remove element from list ```O(1)```. While in Array list removal involves searching the element to be removed and moving all the elements one space ahead.
<img src="img/removeOperation.png" align="middle" height="400" />

### Sort operation
Linked list is performing poorly here, since swapping element also involves pointer manipulations of the nodes in list.
<img src="img/sortOperation.png" align="middle" height="400" />

# Conclusion
1. LinkedList should be chosen if your use case involves addition and removal of elements.
2. ArrayList should be chosen if your use case needs elements to be sorted frequently.
