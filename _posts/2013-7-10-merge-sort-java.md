---
layout: post
title: 归并排序（java）
category: blog
description: 归并排序的java实现
---

练习代码：

	public class MergeSort {
	    public static void mergeSort(int[] array, int from, int to) {
	        if (from < to) {
	            int middle = (int) Math.floor((to + from) / 2);
	            mergeSort(array, from, middle);
	            mergeSort(array, middle + 1, to);
	            merge(array, from, middle, to);
	        }
	    }
	    private static void merge(int[] array, int from, int middle, int to) {
	        int[] leftArray = new int[middle - from + 1];
	        int[] rightArray = new int[to - middle];
	        for (int i = 0; i < leftArray.length; i++) {
	            leftArray[i] = array[from + i];
	        }
	        for (int i = 0; i < rightArray.length; i++) {
	            rightArray[i] = array[middle + i + 1];
	        }
	        int idxL = 0;
	        int idxR = 0;
	        for (int idx = from; idx <= to; idx++) {
	            if (leftArray[idxL] <= rightArray[idxR]) {
	                array[idx] = leftArray[idxL];
	                idxL++;
	                if (idxL == leftArray.length) {
	                    copyRest(array, idx + 1, rightArray, idxR);
	                    return;
	                }
	            } else {
	                array[idx] = rightArray[idxR];
	                idxR++;
	                if (idxR == rightArray.length) {
	                    copyRest(array, idx + 1, leftArray, idxL);
	                    return;
	                }
	            }
	        }
	    }
	    /**
	     * 把oriArr数组中的内容从oIdex开始复制到desArr中，从dIdx开始复制
	     */
	    private static void copyRest(int[] desArr, int dIdx, int[] oriArr, int oIdx) {
	        for (; oIdx < oriArr.length; oIdx++) {
	            desArr[dIdx] = oriArr[oIdx];
	            dIdx++;
	        }
	    }
	    public static void main(String[] args) {
	        int[] a = { 44, 32, 15, 69, 1, 34, 25, 62, 3, 66, 8, 12, 22, 7, 9, -8 };
	        MergeSort.mergeSort(a, 0, a.length - 1);
	        for (int i = 0; i < a.length; i++) {
	            System.out.print(a[i] + " ");
	        }
	    }
	}

