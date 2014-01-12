---
layout: post
title: 基于计数排序的基数排序
category: blog
description: java实现
---

基数排序用作多个关键字排序时很有帮助，必须使用一个稳定的内部算法作为支持
代码：

	/**
	 * 基数排序 需要一个内部的 稳定的 算法作为支持
	 *
	 * @author Hercules
	 *
	 */
	public class RadixSort {
	    /**
	     *
	     * @param arr
	     *            待排序的整数数组，要求都是正整数，且位数都一致
	     * @param digit
	     *            每个元素的位数 比如10位
	     */
	    public static int[] radixSort(int[] arr, int digit) {
	        int[] res = arr;
	        for (int i = 1; i <= digit; i++) {
	            res = countingSort(res,i);
	        }
	        return res;
	    }
	    /**
	     * 把数组arr中的每一个数字在第digitIndex上进行计数排序
	     * @param arr
	     * @param digitIndex
	     * @return
	     */
	    private static int[] countingSort(int[] arr, int digitIndex) {
	        int[] b = new int[arr.length];
	        // 1、先找出数组中的最大值
	        int largest = 0;
	        for (int i = 0; i < arr.length; i++) {
	            int digit = getNumberOnDigitIndex(arr[i], digitIndex);
	            if (digit > largest)
	                largest = digit;
	        }
	        // 2、初始化计数的数组c
	        int[] c = new int[largest + 1];
	        for (int i = 0; i < c.length; i++) {
	            c[i] = 0;
	        }
	        // 3、计数待排序数组中每个元素的出现的个数
	        for (int i = 0; i < arr.length; i++) {
	            c[getNumberOnDigitIndex(arr[i], digitIndex)]++;
	        }// 现在c[m]就表示值m出现的次数
	        // 4、统计
	        for (int i = 1; i < c.length; i++) {
	            c[i] += c[i - 1];
	        }// 现在c[m]就表示 小于等于m的值 出现的次数
	        // 5、排序
	        for (int j = arr.length - 1; j >= 0; j--) {
	            int digit = getNumberOnDigitIndex(arr[j], digitIndex);
	            b[c[digit] - 1] = arr[j];
	            c[digit]--;
	        }
	        return b;
	    }
	    /**
	     * 返回数字a在第digitIndex位上的数字，比如3875652第4位是5
	     * @param a
	     * @param digitIndex
	     * @return
	     */
	    private static int getNumberOnDigitIndex(int a, int digitIndex) {
	        int temp = a;
	        for (int i = 0; i < digitIndex - 1; i++) {
	            temp = temp / 10;
	        }
	        return temp % 10;
	    }
	    /**
	     * testClient
	     * @param args
	     */
	    public static void main(String[] args) {
	        int[] a = new int[]{154,254,264,589,123,145,964,785,563,458};
	        int[] b = radixSort(a,3);
	        for (int i = 0; i < b.length; i++) {
	            System.out.println(b[i]);
	        }
	    }
	}

