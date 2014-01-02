---
layout: post
title: 二分查找
category: blog
description: 二分查找的java实现
---

 二分查找的两种实现方式

	public class BinarySearch {
	    public static int iterativeBinarySearch(int[] array, int v, int low, int high) {
	        while (low <= high) {
	            int mid = (int) Math.floor((low + high) / 2);
	            if (v == array[mid]) {
	                return mid;
	            } else if (v > array[mid]) {
	                low = mid + 1;
	            } else {
	                high = mid - 1;
	            }
	        }
	        return -1;
	    }
	    public static int recursiveBinarySerarch(int[] array, int v, int low, int high) {
	        if (low > high)
	            return -1;
	        int mid = (int) Math.floor((low + high) / 2);
	        if (v == array[mid])
	            return mid;
	        else if (v > array[mid])
	            return recursiveBinarySerarch(array, v, mid + 1, high);
	        else
	            return recursiveBinarySerarch(array, v, low, mid - 1);
	    }
	}

