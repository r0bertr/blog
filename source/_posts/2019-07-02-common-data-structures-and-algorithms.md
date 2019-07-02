---
title: Common Data Structures and Algorithms
date: 2019-07-02 15:26:20
tags:
    - English
---

The simplest are also the most important.

## Arrays and Linked Lists

In almost every programming language, arrays are built-in data structures. An array arranges continuous space for objects, and is good at random access. However, arrays perform bad when meet insertions and deletions, and cost a lot to change their sizes.

With a linked list, insertions and deletions are very fast to perform, but one cannot access objects randomly in a linked list with $O(1)$ time complexity.

Many complex data structures are implemented based on these two simple data structures.

## Stacks

> In a **stack** data structure, all insertions and deletions of entries are made at one end, called the top of the stack. When we add an item to a stack, we say that we push it onto the stack, and when we remove an item, we say that we pop it from the stack. Note that the last item pushed onto a stack is always the first that will be popped from the stack. This property is called **last in, first out**, or **LIFO** for short.

Stacks are simple enough to be understood by only a few sentences. The most important feature of a stack is the **LIFO** property, which is very useful when a task about reversal need to be performed.

### Example: Reverse a List

When a series of objects need to be loaded and written out in reverse order, it\'s suitable to use a stack to accomplish the task. Consider a series of input as follow,

```
10 // The number of objects.
1,2,3,4,5,6,7,8,9,10 // The objects.
```

A list in reverse order should be output like,

```
10,9,8,7,6,5,4,3,2,1
```

At this time, a perfect solution is using a stack. After pushing all the numbers into the stack, the reverse order of the list is automatically produced by popping them out.

## Queues

> For computer applications, we similarly define a queue to be a list in which all additions to the list are mode at one end, and all deletions from the list are made at the other end. Queues are also called **first-in, first-out lists**, or **FIFO** for short.

As a good brother of stacks, queues are also easy to understand. It\'s **FIFO** property is extremely suitable to perform tasks with particular orders, especially when some tasks need to wait for some other tasks.

## Sorting

Sorting is a very facinating topic in the field of algorithms. It can be simple, like perform a selection sort on a list of integers, and suitable for newcommers to learn. It is also worth thinking because the mergesort and the quicksort are two of the most typical algorithms applying the divide-and-conquer method, which are widely used in plenty of tasks. It is extremely useful because it can reduce the time complexity of searching. It also provides judgements of performances of other algorithms. In conclusion, it is worthwhile to learn sorting as many times as you can and to remember it in your heart.

I am not going to introduce sorting algorithms with $O(n^2)$ time complexity like selection sort or bubble sort. I will main focus on quicksort and mergesort here.

### Mergesort

> In the first method, we simply chop the list into two sublists of sizes as nearly equal as possible and then sort them separately. Afterward, we carefully merge the two sorted sublists into a single sorted list.

The main procedure of mergesort is to divide the list into two sublists and perform mergesort on each sublist, repectively. Then the sublists will be merged to a sorted list. This process is recursive. When a sublist only contains one element, this sublist is automatically sorted and the combination begins immediately.

The following is a common recursive `C++` implementation to sort a `vector` of integers.

```c++
void recursiveMergeSort(vector<int> &nums, int left, int right) {
    if (left == right) return;

    // Sort the left sublist and the right sublist, respectively.
    int mid = left + (right - left) / 2;
    recursiveMergeSort(nums, left, mid);
    recursiveMergeSort(nums, mid + 1, right);

    // Merge the two sublists.
    int i = left, j = mid + 1;
    vector<int> tmp(right - left + 1);
    for (int k = 0; k < tmp.size(); k++) {
        if (i <= mid && j <= right) {
            if (nums[i] >= nums[j]) {
                tmp[k] = nums[j];
                j++;
            } else {
                tmp[k] = nums[i];
                i++;
            }
        } else if (i > mid) {
            tmp[k] = nums[j];
            j++;
        } else if (j > right) {
            tmp[k] = nums[i];
            i++;
        }
    }
    for (int k = 0; k < tmp.size(); k++) {
        nums[left + k] = tmp[k];
    }
}

void mergeSort(vector<int> &nums) {
    recursiveMergeSort(nums, 0, nums.size() - 1);
}
```

## References

Robert L. K. and Alexander J. R., _Data Structures and Program Design in C++_.
