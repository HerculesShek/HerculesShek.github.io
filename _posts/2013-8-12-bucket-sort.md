---
layout: post
title: 桶排序
category: blog
description: 桶排序，要求输入的数据都服从均匀分布 算法假设每个元素a[i]满足 0<=a[i]<1
---

又一线性时间的排序算法。对于线性排序算法的思考，可以看看编程珠玑第一章的内容，研究算法问题的时候要仔细分析好，看清楚问题的数据的结构和分布情况，桶排序要求输入服从均匀分布才发挥其性能

代码：

	package com.xingzhe.bucketsort;
	import java.util.ArrayList;
	/**
	 * 桶排序，要求输入的数据都服从均匀分布 算法假设每个元素a[i]满足 0<=a[i]<1
	 *
	 * @author Hercules
	 *
	 */
	public class BucketSort {
	    public static void bucket_sort(double[] a) {
	        // 获取长度
	        int length = a.length;
	        // 此处需要建立一个长度为length的临时数组，每个数组中存放链表，由于java没有泛型数组，故用ArrayList来代替
	        ArrayList<ArrayList<Double>> b = new ArrayList<ArrayList<Double>>();
	        // 把“数组”的每个元素都初始化为空链表
	        for (int i = 0; i < a.length; i++) {
	            b.add(new ArrayList<Double>());
	        }
	        // 把待排序的数组中的每个元素插入到临时数组中
	        for (int i = 0; i < a.length; i++) {
	            b.get((int) Math.floor(length * a[i])).add(a[i]);
	        }
	        // 对每个链表进行插入排序
	        for (int i = 0; i < a.length; i++) {
	            insert_sort(b.get(i));
	        }
	        // 把临时数组中的每个链表的值取出来合并
	        int ai = 0;
	        for (int i = 0; i < b.size(); i++) {
	            if (b.get(i).size() == 0) {
	                continue;
	            }
	            for (int j = 0; j < b.get(i).size(); j++) {
	                a[ai++] = b.get(i).get(j);
	            }
	        }
	    }
	    /*
	     * 针对链表的插入
	     */
	    private static void insert_sort(ArrayList<Double> array) {
	        if (array.size() == 0)
	            return;
	        for (int j = 1; j < array.size(); j++) {
	            double key = array.get(j);
	            int i = j - 1;
	            while (i >= 0 && array.get(i) > key) {
	                array.set(i + 1, array.get(i));
	                i--;
	            }
	            array.set(i + 1, key);
	        }
	    }
	    public static void main(String[] args) {
	        double[] a = new double[] { 0.25, 0.36, 0.25, 0.65, 0.18, 0.48, 0.27,
	                0.29, 0.24, 0.75 };
	        bucket_sort(a);
	        for (int i = 0; i < a.length; i++) {
	            System.out.println(a[i]);
	        }
	    }
	}

