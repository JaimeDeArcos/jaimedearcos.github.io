---
date: 2020-11-24T11:00:00.000Z
layout: post
title: Algorithms - Binary tree traversals in kotlin
subtitle: A simple walkthrough of binary tree traversals algorithms with kotlin examples
description: A simple walkthrough of binary tree traversals algorithms with kotlin examples
image: /assets/img/uploads/rubiks-cube.jpg
optimized_image: /assets/img/uploads/rubiks-cube.jpg
category: algorithms
tags:
  - code
  - kotlin
  - binary-tree
  - algorithms
author: jaimedearcos
paginate: false
---
## Traversals :

> *"Tree traversal refers to the process of visiting (checking and/or updating) each node in a tree data structure, 
> exactly once. Such traversals are classified by the order in which the nodes are visited."* -— [Wikipedia](https://en.wikipedia.org/wiki/Tree_traversal)

There are 2 options: 

* **DFS** : Depth-First Search _“is an algorithm for traversing or searching tree data structure. One starts at the 
  root and explores as far as possible along each branch before backtracking.” -— [Wikipedia](https://en.wikipedia.org/wiki/Depth-first_search)
* **BFS** : Breadth-First Search *“is an algorithm for traversing or searching tree data structure. It starts at the tree root and explores the 
  neighbor nodes first, before moving to the next level neighbors."* -— [Wikipedia](https://en.wikipedia.org/wiki/Breadth-first_search)

## DFS - Depth-First Search

DFS algorithms start from the root and goes deep as possible until they find a leaf before backtracking and exploring another
path. The main 3 DFS types are:

* **Pre-Order**: *(NLR)* process node data, left child and finally right child
  ![](https://jaimedearcos-resources.s3-eu-west-1.amazonaws.com/blog/pre-order.gif)
* **In-Order**: *(LNR)* process left child, node data and finally right child
  ![](https://jaimedearcos-resources.s3-eu-west-1.amazonaws.com/blog/in-order.gif)
* **Post-Order**: *(LRN)* process left child, right child and finally node data 
  ![](https://jaimedearcos-resources.s3-eu-west-1.amazonaws.com/blog/post-order.gif)

### Kotlin Examples:

Usually the DFS algorithms are ease to implement with **recursion**.

**Binary tree**

```kotlin
data class BinaryTreeNode(
        val data:Int,
        val left:BinaryTreeNode? = null,
        val right:BinaryTreeNode? = null
)
```

**Preorder traversal**

```kotlin
fun preOrderTraversal(node: BinaryTreeNode?, 
                      operation: (BinaryTreeNode) -> Unit){
    if (node == null) return;
    // Visit node
    operation.invoke(node)
    // Traverse left
    preOrderTraversal(node.left,operation);
    // Traverse right
    preOrderTraversal(node.right,operation);
}
```

**InOrder traversal**

```kotlin
fun inOrderTraversal(node: BinaryTreeNode?, 
                     operation: (BinaryTreeNode) -> Unit){
    if (node == null) return;
    // Traverse left
    inOrderTraversal(node.left,operation);
    // Visit node
    operation.invoke(node)
    // Traverse right
    inOrderTraversal(node.right,operation);
}
```

**PostOrder traversal**

```kotlin
fun postOrderTraversal(node: BinaryTreeNode?, 
                       operation: (BinaryTreeNode) -> Unit){
    if (node == null) return;
    // Traverse left
    postOrderTraversal(node.left,operation)
    // Traverse right
    postOrderTraversal(node.right,operation)
    // Visit node
    operation.invoke(node)
}
```

## BFS - Breadth-First Search

![](https://jaimedearcos-resources.s3-eu-west-1.amazonaws.com/blog/bfs.gif)

For BFS algorithm we are going to use a queue: 

* Push root node to the queue
* While queue is not empty:

  * Poll element from queue and process data
  * If left child exist, add to the queue
  * If right child exist, add to the queue

```kotlin
   fun bfs(node: BinaryTreeNode, operation: (BinaryTreeNode) -> Unit){

        val queue = LinkedList<BinaryTreeNode>() as Queue<BinaryTreeNode>
        queue.offer(node)
        var current : BinaryTreeNode

        while(!queue.isEmpty()){
            current = queue.poll()
            operation.invoke(current)

            if (current.left != null){
                queue.offer(current.left)
            }
            if (current.right != null){
                queue.offer(current.right)
            }
        }
    }
```

You can find the complete code of this example in this <a href="https://github.com/JaimeDeArcos/kotlin-algorithms/tree/main/tree" target="_blank">GitHub repository</a>