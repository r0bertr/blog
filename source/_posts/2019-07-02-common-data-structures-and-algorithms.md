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

## Recursion

> Recursion is the name for the case when a function invokes itself or invokes a sequence of other functions, one of which eventually invokes the first function again.

Recursion is a good way to fast accomplish a specific group of tasks like factorials. However, any recursive program can be rewritten to a iterative program with higher performance. Therefore, in modern software development, it is not a good idea to use recursion directly, but it is still worthwhile to learn because it provides a good method for us to find out answers of some complicated questions.

There are two main situations under which we may let recursion in. The first one is that we can write a formula which specify the relationship between the current state and one or a few previous states. Besides, an initial state is specified. For example, the factorials can be written like

$$
n! = \begin{cases}
1 &n = 0 \\
n(n - 1)! & n > 0
\end{cases}
$$

$n = 0$ is the initial state and when $n > 0$, the result is computed by $(n - 1)!$, a same problem but with a smaller scale.

### Example: The Towers of Hanoi

If someone would like to teach recursion, he or she must introduce this typical problem -- The Towers of Hanoi. This is a question that almost every one knows but, not all people can write down the solution immediately.

It is not so obvious that this problem is under the first situation, but we still can write down a formula here. Let `m(n, s, d, t)` represents \"the steps move `n` disks from tower `s` to tower `d` using `t` as a temporary tower.\" Therefore, the formula can be written as

$$
m(n, s, d, t) = \begin{cases}
\text{do nothing} & n = 0 \\
m(n - 1, s, t, d) + m(1, s, d, t) + m(n - 1, t, d, s) & n > 1
\end{cases}
$$

The following is a simple `C++` recursive program to accomplish the task.

```c++
void move(int n, int s, int d, int t) {
    if (n > 0) {
        move(n - 1, s, t, d);
        printf("move %d from %d to %d\n", n, s, d);
        move(n - 1, t, d, s);
    }
}
```

One more thing to say is that this program is terrible because every call produces two calls. Thus, with $n$ plates, the total number of calls is

$$
1 + 2 + 4 + \cdots + 2^{n - 1} = 2^n - 1
$$

Therefore, this is a program with $O(2^n)$ time complexity.

---

The second situation is that we need to do backtracking during the execution. In other words, we need to undo some operations that have already been done before. At this time, the structure of a recursive program is very useful. The next example is the typical Eight-Queens Puzzle, which will illustrate how the backtracking works.

### Example: The Eight-Queens Puzzle

Computers are good at enumerating. When they meet the Eight-Queens Puzzle, they simply try all possible positions row by row. Obviously, it is easy to reach a dead end and a backtracking need to be performed, so we use a recursive structure to accomplish the task.

The following is a simple `C++` recursive program to solve the puzzle.

```c++
// The board is a 8x8 array with 0 representing empty and 1 representing
// a queen.

// canPlace validates whether (row, col) can be placed a queen on the board.
bool canPlace(int board[8][8], int row, int col) {
    // Validate the row and the column.
    for (int i = 0; i < 8; i++)
        if (board[row][i] && i != col || board[i][col] && i != row)
            return false;

    // Validate the diagonal.
    for (int i = 0; i < 8; i++) {
        for (int j = 0; j < 8; j++) {
            if (board[i][j] && i != row && 
                (i + j == row + col || i - j == row - col))
                return false;
        }
    }

    return true;
}

// eightQueensPuzzle returns true when the rows >= row can be placed
// correctly on the board.
bool eightQueensPuzzle(int board[8][8], int row) {
    if (row == 8) return true;

    // Enumerate the row.
    for (int i = 0; i < 8; i++) {
        if (canPlace(board, row, i)) {
            board[row][i] = 1;
            if (eightQueensPuzzle(board, row + 1)) {
                // If the rows >= row + 1 can be placed correctly, since the
                // current place is valid, the puzzle is solved.
                break;
            } else {
                // The current place reaches a dead end, try the next place.
                board[row][i] = 0;
            }
        }
    }

    // If the current row has one queen placed, a true is returned.
    for (int i = 0; i < 8; i++) if (board[row][i]) return true;
    return false
}
```

Can you find all solutions of the Eight-Queens Puzzle?

## Searching

Every one knows the general idea of binary search. The problem is that when to use it to reduce the time complexity of a program. Here I would like to introduce a \"standard\" implementation of binary search. Note that even the general idea of binary search is the same, the actual times of comparison may still vary from implementations to implementations.

Here is the code of searching in a sorted `vector` of integers.

```c++
int binarySearch(vector<int> &nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right) {
        int mid = (left + right) / 2;
        if (nums[mid] == target) return mid;
        if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

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
