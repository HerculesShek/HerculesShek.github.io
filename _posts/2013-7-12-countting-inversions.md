---
layout: post
title: 计算逆序对的算法，算法导论2-4
category: blog
description: 计算逆序对的算法，算法导论2-4
---

代码：

	public class CounttingInversions {
	    public static int getInversions(int[] a) {
	        return mergeCount(a, 0, a.length - 1);
	    }
	    private static int mergeCount(int[] a, int from, int to) {
	        if (from < to) {
	            int middle = (int) Math.floor((from + to) / 2);
	            int inL = mergeCount(a, from, middle);
	            int inR = mergeCount(a, middle + 1, to);
	            int sum = merge(a, from, middle, to);
	            return inL + inR + sum;
	        }
	        return 0;
	    }
	    private static int merge(int[] array, int from, int middle, int to) {
	        int inversionCount = 0;
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
	                    return inversionCount;
	                }
	            } else {
	                array[idx] = rightArray[idxR];
	                inversionCount += leftArray.length - idxL;
	                idxR++;
	                if (idxR == rightArray.length) {
	                    copyRest(array, idx + 1, leftArray, idxL);
	                    return inversionCount;
	                }
	            }
	        }
	        return inversionCount;
	    }
	    private static void copyRest(int[] desArr, int dIdx, int[] oriArr, int oIdx) {
	        for (; oIdx < oriArr.length; oIdx++) {
	            desArr[dIdx] = oriArr[oIdx];
	            dIdx++;
	        }
	    }
	    public static void main(String[] args) {
	        int[] a = { 4, 3, 2, 1, 0, -1, -2, -3 };
	        System.out.println(CounttingInversions.getInversions(a));
	    }
	}

