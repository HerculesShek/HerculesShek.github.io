---
layout: post
title: 红黑树 java版
category: blog
description: java实现的红黑树
---

红黑树有5个性质，此处不赘述，直接是代码的实现：
其中前序和后序输出没有给出，把中序输出改一下即是


	public class RedBlackTree extends BinarySearchTree {
	    public RedBlackTree() {
	        NIL.setColor(Color.BLACK);
	        this.setRoot(this.NIL);
	    }
	    private final TreeNode NIL = new TreeNode();
	    @Override
	    public void inorder_tree_walk() {
	        inorder_tree_walk((TreeNode)this.getRoot());
	    }
	    private void inorder_tree_walk(TreeNode t) {
	        if (t != this.NIL) {
	            inorder_tree_walk((TreeNode)t.getLeftChild());
	            System.out.println(t.getElement());
	            inorder_tree_walk((TreeNode)t.getRightChild());
	        }
	    }
	    // 左旋操作
	    public void left_rotate(TreeNode x) {
	        TreeNode y = (TreeNode) x.getRightChild();
	        x.setRightChild(y.getLeftChild());
	        if (y.getLeftChild() != this.NIL)
	            y.getLeftChild().setParent(x);
	        y.setParent(x.getParent());
	        if (x.getParent() == this.NIL)
	            this.setRoot(y);
	        else if (x == x.getParent().getLeftChild())
	            x.getParent().setLeftChild(y);
	        else
	            x.getParent().setRightChild(y);
	        y.setLeftChild(x);
	        x.setParent(y);
	    }
	    // 右旋操作
	    public void right_rotate(TreeNode y) {
	        TreeNode x = (TreeNode) y.getLeftChild();
	        y.setLeftChild(x.getRightChild());
	        if (x.getRightChild() != this.NIL)
	            x.getRightChild().setParent(y);
	        x.setParent(y.getParent());
	        if (y.getParent() == this.NIL)
	            this.setRoot(x);
	        else if (y == y.getParent().getLeftChild())
	            y.getParent().setLeftChild(x);
	        else
	            y.getParent().setRightChild(x);
	        x.setRightChild(y);
	        y.setParent(x);
	    }
	    // 插入操作
	    public void tree_insert(TreeNode z) {
	        TreeNode y = this.NIL;
	        TreeNode x = (TreeNode) this.getRoot();
	        while (x != this.NIL) {
	            y = x;
	            if (z.getElement() < x.getElement())
	                x = (TreeNode) x.getLeftChild();
	            else
	                x = (TreeNode) x.getRightChild();
	        }
	        z.setParent(y);
	        if (y == this.NIL)
	            this.setRoot(z);
	        else if (z.getElement() < y.getElement())
	            y.setLeftChild(z);
	        else
	            y.setRightChild(z);
	        z.setLeftChild(this.NIL);
	        z.setRightChild(NIL);
	        z.setColor(Color.RED);
	        rb_insert_fixup(z);
	    }
	    // 插入操作后的修正
	    private void rb_insert_fixup(TreeNode z) {
	        while (((TreeNode) z.getParent()).color == Color.RED) {
	            if (z.getParent() == z.getParent().getParent().getLeftChild()) {// z的父节点是祖父节点的左孩子
	                TreeNode y = (TreeNode) z.getParent().getParent()
	                        .getRightChild();
	                if (y.color == Color.RED) {
	                    ((TreeNode) z.getParent()).color = Color.BLACK;
	                    y.color = Color.BLACK;
	                    ((TreeNode) z.getParent().getParent()).color = Color.RED;
	                    z = (TreeNode) z.getParent().getParent();
	                } else if (z == z.getParent().getRightChild()) {
	                    z = (TreeNode) z.getParent();
	                    left_rotate(z);
	                } else {
	                    ((TreeNode) z.getParent()).color = Color.BLACK;
	                    TreeNode ff = (TreeNode) z.getParent().getParent();
	                    ff.color = Color.RED;
	                    right_rotate(ff);
	                }
	            } else {// //z的父节点是祖父节点的右孩子
	                TreeNode y = (TreeNode) z.getParent().getParent()
	                        .getLeftChild();
	                if (y.color == Color.RED) {
	                    ((TreeNode) z.getParent()).color = Color.BLACK;
	                    y.color = Color.BLACK;
	                    ((TreeNode) z.getParent().getParent()).color = Color.RED;
	                    z = (TreeNode) z.getParent().getParent();
	                } else if (z == z.getParent().getLeftChild()) {
	                    z = (TreeNode) z.getParent();
	                    right_rotate(z);
	                } else {
	                    ((TreeNode) z.getParent()).color = Color.BLACK;
	                    TreeNode ff = (TreeNode) z.getParent().getParent();
	                    ff.color = Color.RED;
	                    left_rotate(ff);
	                }
	            }
	        }
	        ((TreeNode) this.getRoot()).color = Color.BLACK;
	    }
	    public void rb_transplant(TreeNode u, TreeNode v) {
	        if (u.getParent() == this.NIL)
	            this.setRoot(v);
	        else if (u == u.getParent().getLeftChild())
	            u.getParent().setLeftChild(v);
	        else
	            u.getParent().setRightChild(v);
	        v.setParent(u.getParent());
	    }
	    public void tree_delete(TreeNode z) {
	        TreeNode y = z;
	        Color y_original_color = y.color;
	        TreeNode x;
	        if (z.getLeftChild() == this.NIL) {
	            x = (TreeNode) z.getRightChild();
	            rb_transplant(z, (TreeNode) z.getRightChild());
	        } else if (z.getRightChild() == this.NIL) {
	            x = (TreeNode) z.getLeftChild();
	            rb_transplant(z, (TreeNode) z.getLeftChild());
	        } else {
	            y = (TreeNode) tree_minimum(z.getRightChild());
	            y_original_color = y.color;
	            x = (TreeNode) y.getRightChild();
	            if (y.getParent() == z)
	                x.setParent(y);
	            else {
	                rb_transplant(y, (TreeNode) y.getRightChild());
	                y.setRightChild(z.getRightChild());
	                y.getRightChild().setParent(y);
	            }
	            rb_transplant(z, y);
	            y.setLeftChild(z.getLeftChild());
	            y.getLeftChild().setParent(y);
	            y.setColor(z.color);
	        }
	        if (y_original_color == Color.BLACK)
	            rb_delete_fixup(x);
	    }
	    private void rb_delete_fixup(TreeNode x) {
	        while (x != this.getRoot() && x.color == Color.BLACK) {
	            if (x == x.getParent().getLeftChild()) {
	                TreeNode w = (TreeNode) x.getParent().getRightChild();
	                if (w.color == Color.RED) {
	                    w.color = Color.BLACK;
	                    ((TreeNode) x.getParent()).color = Color.RED;
	                    left_rotate((TreeNode) x.getParent());
	                    w = (TreeNode) x.getParent().getRightChild();
	                }
	                if (((TreeNode) w.getLeftChild()).color == Color.BLACK
	                        && ((TreeNode) w.getRightChild()).color == Color.BLACK) {
	                    w.color = Color.RED;
	                    x = (TreeNode) x.getParent();
	                } else if (((TreeNode) w.getRightChild()).color == Color.BLACK) {
	                    ((TreeNode) w.getLeftChild()).color = Color.BLACK;
	                    w.color = Color.RED;
	                    right_rotate(w);
	                    w = (TreeNode) x.getParent().getRightChild();
	                }
	                w.color = ((TreeNode) x.getParent()).color;
	                ((TreeNode) x.getParent()).color = Color.BLACK;
	                ((TreeNode) w.getRightChild()).color = Color.BLACK;
	                left_rotate((TreeNode) x.getParent());
	                x = (TreeNode) this.getRoot();
	            } else {
	                TreeNode w = (TreeNode) x.getParent().getLeftChild();
	                if (w.color == Color.RED) {
	                    w.color = Color.BLACK;
	                    ((TreeNode) x.getParent()).color = Color.RED;
	                    right_rotate((TreeNode) x.getParent());
	                    w = (TreeNode) x.getParent().getLeftChild();
	                }
	                if (((TreeNode) w.getLeftChild()).color == Color.BLACK
	                        && ((TreeNode) w.getRightChild()).color == Color.BLACK) {
	                    w.color = Color.RED;
	                    x = (TreeNode) x.getParent();
	                } else if (((TreeNode) w.getLeftChild()).color == Color.BLACK) {
	                    ((TreeNode) w.getRightChild()).color = Color.BLACK;
	                    w.color = Color.RED;
	                    left_rotate(w);
	                    w = (TreeNode) x.getParent().getLeftChild();
	                }
	                w.color = ((TreeNode) x.getParent()).color;
	                ((TreeNode) x.getParent()).color = Color.BLACK;
	                ((TreeNode) w.getLeftChild()).color = Color.BLACK;
	                right_rotate((TreeNode) x.getParent());
	                x = (TreeNode) this.getRoot();
	            }
	        }
	        x.color = Color.BLACK;
	    }
	    class TreeNode extends BinarySearchTree.TreeNode {
	        private Color color;
	        public Color getColor() {
	            return color;
	        }
	        public void setColor(Color color) {
	            this.color = color;
	        }
	    }
	    enum Color {
	        RED, BLACK
	    }
	}
