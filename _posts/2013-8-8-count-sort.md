---
layout: post
title: 计数排序
category: blog
description: java实现
---

典型的用空间换时间的线性算法，本算法要求待排序的数组元素都是非负整数，但是可以修改算法，使其适应浮点和负数的排序

代码：

	/**
	 * 计数排序 排序要求待排序的数组中存储的都是非负整数(0到k)，（理论上可以是任意数，不过需要
	 * 把负数变成正整数，把浮点数变成正整数，需要的时候可以自己修改）
	 *
	 * @author Hersules
	 */
	public class CountingSort {
	    public static int[] countingSort(int[] a) {
	        int[] b = new int[a.length];
	        // 1、先找出数组中的最大值
	        int largest = 0;
	        for (int i = 0; i < a.length; i++) {
	            if (a[i] > largest)
	                largest = a[i];
	        }
	        // 2、初始化计数的数组c
	        int[] c = new int[largest + 1];
	        for (int i = 0; i < c.length; i++) {
	            c[i] = 0;
	        }
	        // 3、计数待排序数组中每个元素的出现的个数
	        for (int i = 0; i < a.length; i++) {
	            c[a[i]]++;
	        }// 现在c[m]就表示值m出现的次数
	        // 4、统计
	        for (int i = 1; i < c.length; i++) {
	            c[i] += c[i - 1];
	        }// 现在c[m]就表示 小于等于m的值 出现的次数
	        // 5、排序
	        for (int j = a.length - 1; j >= 0; j--) {
	            b[c[a[j]] - 1] = a[j];
	            c[a[j]]--;
	        }
	        return b;
	    }
	}
