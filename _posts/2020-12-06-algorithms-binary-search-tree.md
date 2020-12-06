---
date: 2020-12-06T11:00:00.000Z
layout: post
title: Algorithms - Binary Search Tree in kotlin
subtitle: Brief review of binary search trees with kotlin examples
description: Brief review of binary search trees with kotlin examples
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
  

>_"A binary search tree (BST) is a rooted binary tree whose internal nodes each store a key greater than all the keys in 
the node's left subtree and less than those in its right subtree."_ -- [Wikipedia](https://en.wikipedia.org/wiki/Binary_search_tree)

One of the biggest **advantages of Binary Search Trees** is the performance of lookup, addition and removal of data nodes, each 
comparison skips the half of the remaining tree, so the operation usually is **O(log n)**

Here is **an example** of Binary Search Tree:

![](/assets/img/uploads/binary-search-tree.jpg)

## Let's code a binary search tree

The fist step it's going to be the data structure. It's pretty similar to the one we code in the previous post about 
<a href="/algorithms-working-with-trees/" target="_blank">binary tree traversals</a>.

```kotlin
data class BinarySearchTreeNode(
        var data:Int,
        var left: BinarySearchTreeNode? = null,
        var right: BinarySearchTreeNode? = null
){
   // ...
}
```

As you can see is basically the same as simple binary trees, the main difference is going to be how we **insert, search 
and delete** values from the tree.

## Insert values in the tree   

The insertion function is a simple recursive one, there are 4 cases:

- New item is **lower or equals** than the current node data:
    - If left exist --> insert the value in left child (recursion)
    - Else, create a new left child with the value (finish condition)
- New item is **greater** than the current node data: 
   - If left exist --> insert the value in left child (recursion)
   - Else, create a new right child with the value (finish condition)



```kotlin
fun insert(value:Int){
    if(value <= data && left != null){
        left!!.insert(value)
    }else if (value <= data){
        left = BinarySearchTreeNode(value)
    }else if(value> data && right != null){
        right!!.insert(value)
    }else{
        right = BinarySearchTreeNode(value)
    }
}
```


## Search/contains value in the tree

In the following **kotlin** snippet we are going to implement the `contains` function, because our tree stores only Integer values.
If you want to store complex objects, is not difficult to modify the function to return the data of searched value instead
 a boolean, but you need to store only comparable objects with proper implementation of equals function.
 

```kotlin
fun contains(value:Int) : Boolean{
    if (value < data && left !=null) {
        return left!!.contains(value)
    }
    if(value > data && right!=null) {
        return right!!.contains(value)
    }
    return value==data
}
```

## Removing nodes from Binary Search Tree

Probably the most complex operation because if we need to remove a node with children we should reorder the tree.
We need to consider the following 3 scenarios:

1. The removed value is `lower` than the current node:
    - **if left child exist**, we use the **recursion** to remove value from the left child
    - **else**, value does not exist in tree, return false.
2. Removed value is `greater` than the current node:
    - **if right child exist**, we use the **recursion** to remove value from the left child
    - **else**, value does not exist in tree, return false.
3. Removed value is `equals` to the current node data:
    - Depending on the existing children we should remove or reorder _(see code comments)_ 

    
```kotlin
fun remove(value:Int, parent:BinarySearchTreeNode) : Boolean{
    // Value lower than current node data
    if (value<data && left!=null){
        return left!!.remove(value,this)
    }else if(value<data){
        return false
    // Value greater than current node data
    }else if(value>data && right != null){
        return left!!.remove(value,this)
    }else if (value>data){
        return false
    // Value equals the greater node data
    }else {
        // No Children, current node is parents LEFT node
        if (left == null && right==null && this == parent.left) {
            parent.left = null
        // No Children, current node is parents RIGHT node
        }else if (left == null && right==null && this==parent.right){
            parent.right = null
        // ONLY LEFT child, current node is parents LEFT node
        }else if(left!=null && right==null && this == parent.left){
            parent.left = this.left
        // ONLY LEFT child, current node is parents RIGHT node
        }else if(left!=null && right== null && this == parent.right){
            parent.right = this.left
        // ONLY RIGHT child, current node is parents LEFT node
        }else if (right!=null && left==null && this == parent.left) {
            parent.left = this.right
        // ONLY RIGHT child, current node is parents RIGHT node
        }else if(right!=null && left==null && this == parent.right) {
            parent.right = this.right
        // BOTH Children exists, replace current data with min value from right 
        }else{
            data = right!!.min()
            right!!.remove(data,this)
        }
        return true
    }
}

fun min() : Int = if (left==null) data else left!!.min()
```

You can find the complete code of this example in this
<a href="https://github.com/JaimeDeArcos/kotlin-algorithms/blob/main/trees/src/main/kotlin/es/jaimedearcos/algorithms/tree/data/BinarySearchTreeNode.kt" target="_blank">
GitHub repository
</a> 

