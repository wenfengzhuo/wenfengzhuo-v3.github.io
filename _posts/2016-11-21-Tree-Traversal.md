---
layout: post
comments: true
categories: algorithms
title: Binary Tree Traversal
---
> The traversal of a binary tree is pretty easy to implement and we already know that there are three ways of tree traversal:
> *preorder*, *inorder*, *postorder*. The traversal with recursion is trivial, so is there any way to travese a tree? This
> article will discuss different ways to traversal a tree based on my learning.




### Traverse tree recursively
It is pretty straightfoward to traverse a tree using recursion, and also it makes the code simple and easy to understand. The
following code snipets are the recursive versions of three ways of tree traversal.

Let's define the binary tree structure first:

```java
public class TreeNode {
  public int val;
  public TreeNode left;
  public TreeNode right;
}
```

Preorder traversal:

```java
public void preorder(TreeNode root, List<Integer> res) {
  if (root == null) {
    return;
  }
  res.add(root.val);
  preorder(root.left, res);
  preorder(root.right, res);
}
```

Inorder traversal:

```java
public void inorder(TreeNode root, List<Integer> res) {
  if (root == null) {
    return;
  }
  inorder(root.left, res);
  res.add(root.val);
  inorder(root.right, res);
}
```

Postorder traversal:

```java
public void postorder(TreeNode root, List<Integer> res) {
  if (root == null) {
    return;
  }
  postorder(root.left, res);
  postorder(root.right, res);
  res.add(root.val);
}
```

From the simple code above, we can clearly conclude that the recursive traversal of a binary tree is just following the natural
traversal order. For example, the definition of inorder traversal is that we first visit left subtree, and then current node, and
finally the right tree, and the recursive code directly reflects the definition.

### Traverse tree iteratively
How about we traverse a binary tree without recursion? First why are we asking this question. Sometimes, we might be given some very
unbalanced tree or even worse the tree is just like a linked list which means every node of the tree only has right child. See the
following tree. The direct impact of this input is that we might encounter _stackoverflow_ exception during the program. This is one
of the reason that motivates us to use other way to travese a tree.

```
1
 \
  2
   \
    3
     \
     ...
```

How about we traverse a binary tree without recursion? The answer is iterative traversal, although it might be a little bit more
complicated than recursion. In my understanding, __why it becomes complicated__ is because we now have to carefully maintain the intermediate
status of the traversal. We have to store some nodes in memory because they are not in the right order to be visted, and then we will
come back to visit it again. This is probably the reason things become complex.

How do we implement the iterative traversal? If we consider this question as a task to convert the recursive traversal to iterative
traversal, we will quickly know that we could use _stack_ to accomplish this. Why? Because using a stack is just mimicking the
recursion. For a recursion, actually the underline technique is using stack to store the intermediate status of the function. Now
the task becomes clear. The task is how to use stack to iteratively traverse tree which is just like traversing a tree recursively.

Let's take a look at the inorder traversal of a binary tree iteratively:

```java
public List<Integer> inorder(TreeNode root) {
  List<Integer> res = new ArrayList<>();
  Stack<TreeNode> stack = new Stack<>();
  TreeNode cur = root;
  while (cur != null || !stack.isEmpty()) {
    if (cur != null) {
      stack.push(cur); // -> inorder(cur.left, res), with this recursive function call, the cur will be stored in function stack
      cur = cur.left;
    } else {
      cur = stack.pop(); // -> res.add(cur.val), when inorder(cur.left, res) completes, the cur node will be popped from function stack
      res.add(cur.val);
      cur = cur.right; // -> inorder(cur.right, res)
    }
  }
  return res;
}
```

The comments in above code compare the recursive code with the iterative code. We can clearly see that what we actually did is that we maniplated the function call with our own code.

With the above understanding, we can easily implement the preroder traversal and inorder traversal.

Preorder traversal iteratively:

```java
public List<Integer> preorder(TreeNode root) {
  List<Integer> res = new ArrayList<>();
  Stack<TreeNode> stack = new Stack<>();
  TreeNode cur = root;
  while (cur != null || !stack.isEmpty()) {
    if (cur != null) {
      res.add(cur.val);
      stack.push(cur);
      cur = cur.left;
    } else {
      cur = stack.pop();
      cur = cur.right;
    }
  }
}
```

Postorder traversal iteratively:

```java
public List<Integer> postorder(TreeNode root) {
  List<Integer> res = new ArrayList<>();
  Stack<TreeNode> stack = new Stack<>();
  TreeNode cur = root;
  while (cur != null || !stack.isEmpty()) {
    if (cur != null) {
      stack.push(cur);
      cur = cur.left;
    } else if (stack.peek().right != null) {
      cur = stack.peek().right;
      stack.push(cur);
    } else {
      cur = stack.pop();
      res.add(cur.val);
      while (!stack.isEmpty() && stack.peek().right == cur) {
        cur = stack.pop();
        res.add(cur.val);
      }
      cur = stack.isEmpty() ? null : stack.peek().right;
    }
  }
  return res;
}
```    

### Morris Traversal
We are not ending up with using stack to traverse a binary tree. Could we avoid to use stack to traverse the tree? First agian, why are we asked this question? The main reason is that the stack will consume extra memory and another reason could be there is something we didn't use in binary tree which could help us to traverse a tree without a stack. Here we could use the right child link of a leaf node to point to its inorder successor. You may be curious how people come up with this solution, so please refer to [Threaded Binary Tree](https://en.wikipedia.org/wiki/Threaded_binary_tree).
During the traversal, actually we could store the inorder successor of a leaf node. When we finish the traversal of a leaf node, we could use this link to navigate its successor with O(1) time. See the picture below about how to store the link:

![Binary Tree Threaded](https://upload.wikimedia.org/wikipedia/commons/8/8b/Threaded_Binary_Tree.png).

Here is the code of inorder morris traversal:

```java
public List<Integer> morrisInorder(TreeNode root) {
  List<Integer> res = new ArrayList<>();
  TreeNode cur = root;
  while (cur != null) {
    if (cur.left != null) {
      TreeNode pre = cur.left;
      while (pre.right != null && pre.right != cur) {
        pre = pre.right;
      }
      if (pre.right == null) {
        pre.right = cur; // Link to its inorder successor
      } else {
        pre.right = null; // Unlink to make the tree unmodified
        res.add(cur.val);
        cur = cur.right;
      }
    } else {
      res.add(cur.val);
      cur = cur.right; // Right will always exists due to our previous linking
    }
  }
}
```
