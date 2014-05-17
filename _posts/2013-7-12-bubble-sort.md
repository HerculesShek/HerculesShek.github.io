---
layout: post
title: 冒泡排序
category: blog
description: 冒泡排序的java实现
---

代码：

	public class BubbleSort {
	    public static void sort(int[] a) {
	        for (int i = 1; i < a.length; i++) {
	            for (int j = a.length - 1; j >= i; j--) {
	                if (a[j] < a[j - 1]) {
	                    swap(a, j, j - 1);
	                }
	            }
	        }
	    }
	    private static void swap(int[] a, int j, int i) {
	        a[j] += a[i];
	        a[i] = a[j] - a[i];
	        a[j] = a[j] - a[i];
	    }
	    public static void main(String[] args) {
	        int[] a = { 2, 5, 4, 3, 9, 2 };
	        BubbleSort.sort(a);
	        for (int i = 0; i < a.length; i++) {
	            System.out.println(a[i]);
	        }
	    }
	}
