---
layout: post
title: 线性时间的选择算法
category: blog
description: 找到数组a[p...r]中第i个小/大的元素的值
---

期望运行时间为Θ(n)的选择问题的算法，最坏则为Θ(n^2)

	public class RandomizedSelect {
	    /**
	     * 找到数组a[p...r]中第i个小的元素的值
	     * 注意：i的值必须在[1...r-p+1]之间，应定义异常处理，本处省略
	     * @param a
	     * @param p
	     * @param r
	     * @param i
	     * @return
	     */
	    public static int randomizedSelect(int[] a, int p, int r, int i) {
	        if (p == r)
	            return a[p];
	        int q = randomized_partion(a, p, r);
	        int k = q - p + 1;
	        if (k == i)
	            return a[q];
	        else if (i < k)
	            return randomizedSelect(a, p, q - 1, i);
	        else
	            return randomizedSelect(a, q + 1, r, i - k);
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
	        int[] a = new int[] { 1, 1, 1, 5, 6, 22, 44, 2, 11, 66 };
	        int res = randomizedSelect(a, 0, a.length - 1, 3);
	        System.out.println(res);
	    }
	}
