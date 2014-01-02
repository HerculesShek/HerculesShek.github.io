---
layout: post
title: 插入排序的递归实现（java版）
category: blog
description: 插入排序
---

直接是代码，权作练习了！

	package com.xingzhe.demo;
 
	public class ReverseInsertion {
	    public static void insert(int[] a, int j) {
	        int key = a[j];
	        int i = j - 1;
	        while (i >= 0 && a[i] > key) {
	            a[i + 1] = a[i];
	            ii = i - 1;
	        }
	        a[i + 1] = key;
	    }
 
	    public static void reverse_insert_sort(int[] a, int n) {
	        if (n > 0) {
	            reverse_insert_sort(a, n - 1);
	            insert(a, n);
	        }
 
	    }
 
	    public static void main(String[] args) {
	        int[] a = { 2, 3, 1, 1, 2, 6, 7, 4, 7, 9, 8, 0, 5 };
	        reverse_insert_sort(a, a.length - 1);
	        for (int i = 0; i < a.length; i++) {
	            System.out.print(a[i] + ", ");
	        }
	    }
	} 
