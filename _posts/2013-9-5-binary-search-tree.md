---
layout: post
title: 二叉搜索树 java版
category: blog
description: java实现的二叉搜索树
---

二叉搜索树的基本版本：

	public class BinarySearchTree {
	    private TreeNode root;// 根节点
	    public TreeNode getRoot() {
	        return root;
	    }
	    public void setRoot(TreeNode root) {
	        this.root = root;
	    }
	    // 中序遍历
	    public void inorder_tree_walk() {
	        inorder_tree_walk(this.root);
	    }
	    // 前序遍历
	    public void preorder_tree_walk() {
	        preorder_tree_walk(this.root);
	    }
	    // 后序遍历
	    public void postorder_tree_walk() {
	        postorder_tree_walk(this.root);
	    }
	    // 查找
	    public TreeNode tree_search(int k) {
	        return tree_search(this.root, k);
	    }
	    // 返回最小值
	    public TreeNode tree_minimum() {
	        return tree_minimum(this.root);
	    }
	    // 或得最大值
	    public TreeNode tree_maximum() {
	        return tree_maximum(this.root);
	    }
	    // 获取节点t的后继节点
	    public TreeNode tree_successor(TreeNode t) {
	        if (t.rightChild != null)
	            return tree_minimum(t.rightChild);
	        TreeNode y = t.parent;
	        while (y != null && t == y.rightChild) {
	            t = y;
	            y = y.parent;
	        }
	        return y;
	    }// 获取节点t的前驱节点
	    public TreeNode tree_predecessor(TreeNode t) {
	        if (t.leftChild != null)
	            return tree_maximum(t.leftChild);
	        TreeNode y = t.parent;
	        while (y != null && t == y.leftChild) {
	            t = y;
	            y = y.parent;
	        }
	        return y;
	    }
	    // 向树中插入元素
	    public void tree_insert(TreeNode z) {
	        TreeNode y = null;
	        TreeNode x = this.root;
	        while (x != null) {
	            y = x;
	            if (z.element < x.element)
	                x = x.leftChild;
	            else
	                x = x.rightChild;
	        }
	        z.parent = y;
	        if (y == null) {
	            this.root = z;
	        } else if (z.element < y.element) {
	            y.leftChild = z;
	        } else {
	            y.rightChild = z;
	        }
	    }
	    // 从树中删除元素
	    public void tree_delete(TreeNode z) {
	        if (z.leftChild == null) {
	            transplant(z, z.rightChild);
	        } else if (z.rightChild == null) {
	            transplant(z, z.leftChild);
	        } else {
	            TreeNode y = tree_minimum(z.rightChild);
	            if (y.parent != z) {
	                transplant(y, y.rightChild);
	                y.rightChild = z.rightChild;
	                y.rightChild.parent = y;
	            }
	            transplant(z, y);
	            y.leftChild = z.leftChild;
	            y.leftChild.parent = y;
	        }
	    }
	    private void transplant(TreeNode u, TreeNode v) {
	        if (u.parent == null) {
	            this.root = v;
	        } else if (u == u.parent.leftChild) {
	            u.parent.leftChild = v;
	        } else {
	            u.parent.rightChild = v;
	        }
	        if (v != null) {
	            v.parent = u.parent;
	        }
	    }
	    private void inorder_tree_walk(TreeNode t) {
	        if (t != null) {
	            inorder_tree_walk(t.leftChild);
	            System.out.println(t.element);
	            inorder_tree_walk(t.rightChild);
	        }
	    }
	    private void preorder_tree_walk(TreeNode t) {
	        if (t != null) {
	            System.out.println(t.element);
	            inorder_tree_walk(t.leftChild);
	            inorder_tree_walk(t.rightChild);
	        }
	    }
	    private void postorder_tree_walk(TreeNode t) {
	        if (t != null) {
	            inorder_tree_walk(t.leftChild);
	            inorder_tree_walk(t.rightChild);
	            System.out.println(t.element);
	        }
	    }
	    private TreeNode tree_search(TreeNode t, int k) {
	        if (t == null || k == t.element)
	            return t;
	        if (k < t.element)
	            return tree_search(t.leftChild, k);
	        else
	            return tree_search(t.rightChild, k);
	    }
	    private TreeNode tree_minimum(TreeNode t) {
	        while (t.leftChild != null)
	            t = t.leftChild;
	        return t;
	    }
	    private TreeNode tree_maximum(TreeNode t) {
	        while (t.rightChild != null)
	            t = t.rightChild;
	        return t;
	    }
	    class TreeNode {
	        private TreeNode parent;
	        private TreeNode leftChild;
	        private TreeNode rightChild;
	        private int element;
	        public TreeNode getParent() {
	            return parent;
	        }
	        public void setParent(TreeNode parent) {
	            this.parent = parent;
	        }
	        public TreeNode getLeftChild() {
	            return leftChild;
	        }
	        public void setLeftChild(TreeNode leftChild) {
	            this.leftChild = leftChild;
	        }
	        public TreeNode getRightChild() {
	            return rightChild;
	        }
	        public void setRightChild(TreeNode rightChild) {
	            this.rightChild = rightChild;
	        }
	        public int getElement() {
	            return element;
	        }
	        public void setElement(int element) {
	            this.element = element;
	        }
	    }
	}
