---
layout: post
title: 快速排序的java代码
category: blog
description: java实现的快速排序
---

### java快速排序的基本版：

	public class QuickSort {
	    public static void quick_sort(int[] a, int p, int r) {
	        if (p < r) {
	            int q = partion(a, p, r);
	            quick_sort(a, p, q - 1);
	            quick_sort(a, q + 1, r);
	        }
	    }
	    private static int partion(int[] a, int p, int r) {
	        int x = a[r];
	        int i = p - 1;
	        for (int j = p; j <= r - 1; j++) {
	            if (a[j] <= x) {
	                i++;
	                Tool.swap(a, i, j);
	            }
	        }
	        Tool.swap(a, i + 1, r);
	        return i + 1;
	    }
	    public static void main(String[] args) {
	        int[] a = new int[] { 2, 5, 4, 5, 4, 3, 23, 2, 3, 2, 3, 2, 34, 4, 23,
	                34, 5, 435, 12, 43, 5, 23, 4, 1243, 123 };
	        quick_sort(a, 0, a.length - 1);
	        for (int i = 0; i < a.length; i++) {
	            System.out.println(a[i]);
	        }
	    }
	}

### 下面是快速排序的随机化版本：

	/**
	 * 随机版本的快速排序
	 *
	 * @author Hercules
	 *
	 */
	public class RandomizedQuickSort {
	    public static void randomized_quick_sort(int[] a, int p, int r) {
	        if (p < r) {
	            int q = randomized_partion(a, p, r);
	            randomized_quick_sort(a, p, q - 1);
	            randomized_quick_sort(a, q + 1, r);
	        }
	    }
	    private static int randomized_partion(int[] a, int p, int r) {
	        int i = p + (int) (Math.random() * (r - p));
	        Tool.swap(a, i, r);
	        return partion(a, p, r);
	    }
	    private static int partion(int[] a, int p, int r) {
	        int x = a[r];
	        int i = p - 1;
	        for (int j = p; j <= r - 1; j++) {
	            if (a[j] <= x) {
	                i++;
	                Tool.swap(a, i, j);
	            }
	        }
	        Tool.swap(a, i + 1, r);
	        return i + 1;
	    }
	    public static void main(String[] args) {
	        int[] a = new int[] { 32, 25, 34, 95, 34, 173, 23, 12, 33, 142, 53, 32,
	                34, 34, 123, 34, 15, 435, 12, 43, 305, 23, 14, 1243, 123 };
	        randomized_quick_sort(a, 0, a.length - 1);
	        for (int i = 0; i < a.length; i++) {
	            System.out.println(a[i]);
	        }
	    }
	}
	
发现其他的快速排序的东西也还是很好玩的
