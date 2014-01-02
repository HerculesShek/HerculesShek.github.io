---
layout: post
title: 插入排序代码回顾
category: blog
description: 插入排序
---

代码：

	public class InsertionSort {
	    public void sort(int[] array) {
	        for (int j = 1; j < array.length; j++) {
	            int key = array[j];
	            int i = j - 1;
	            while (i >= 0 && array[i] > key) {
	                array[i + 1] = array[i];
	                i--;
	            }
	            array[i + 1] = key;
	        }
	    }
	}



