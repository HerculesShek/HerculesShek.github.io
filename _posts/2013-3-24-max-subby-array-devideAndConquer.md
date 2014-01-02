---
layout: post
title: 分治法实现求解最大子数组
category: blog
description: 求解最大子数组问题的分治法实现及总结
---

直接上代码：

	package com.C2;
 
	public class MaxSubArray {
	    /**
	     * 该方法是整个算法的核心，就是最基本的处理单元 Nuclear Processing Unit （NPU）
	     * @param subArray 该方法接受一个 数组、下表low、high和middle 数据结构的实例
	     * @return a instance of ResultArray 返回一个下标元组，该元组指定了跨越中点的最大子数组的边界以及
	     *  这个最大子数组的和（此三者组成一个数据结构）
	     */
	    public static ResultArray find_max_crossing_subarray(SubArray subArray) {
	        int[] array = subArray.getArray();
	        //先从中间坐标向左进行遍历，顺序不能颠倒
	        int left_sum = Integer.MIN_VALUE;
	        int low = subArray.getMiddle();//这个low是来确定返回的最大子数组的左边边界坐标
	        int sum = 0;
	        for (int i = low; i >= subArray.getLow(); i--) {
	            sum += array[i];
	            if (sum > left_sum) {//一旦当前的和变得更大，则取代当前的和并且重置low的值
	                left_sum = sum;
	                low = i;
	            }
	        }
	        //再从中间坐标向左进行遍历
	        int right_sum = Integer.MIN_VALUE;
	        int high = subArray.getMiddle() + 1;//这个low是来确定返回的最大子数组的右边边界坐标
	        sum = 0;
	        for (int i = high; i <= subArray.getHigh(); i++) {
	            sum += array[i];
	            if (sum > right_sum) {//一旦当前的和变得更大，则取代当前的和并且重置high的值
	                right_sum = sum;
	                high = i;
	            }
	        }
	        //返回最大子数组的下标以及和
	        return new ResultArray(low, high, left_sum + right_sum);
	    }
	
	    /**
	     * 该方法是整个算法的调度中心，是出口和入口 Nuclear Scheduling Unit （NSU）
	     * @param tempArray 请记住，TempArray定义了问题的形式，它因此成为递归的接受者和被处理的中间数据结构
	     * @return a instance of ResultArray
	     */
	    public static ResultArray find_max_subarray(TempArray tempArray) {
	        if (tempArray.getLow() == tempArray.getHigh())
	            //如果当前问题是左右下标一样大，则直接返回，这是问题的基本情况，递归的出口，要首先判断
	            return new ResultArray(tempArray.getLow(), tempArray.getHigh(), tempArray.getArray()[tempArray.getLow()]);
	        else {
	            int middle = (tempArray.getLow() + tempArray.getHigh()) / 2; //计算中间坐标，取
	            ResultArray leftResultArray = find_max_subarray(new TempArray(
	                    tempArray.getLow(), middle, tempArray.getArray())); //左边递归
	            ResultArray rightResultArray = find_max_subarray(new TempArray(
	                    middle + 1, tempArray.getHigh(), tempArray.getArray()));    //右边递归
	            ResultArray crossingResultArray = find_max_crossing_subarray(new SubArray(
	                    tempArray.getLow(), tempArray.getHigh(), middle, tempArray.getArray()));    //跨界的最大数组
	            if (leftResultArray.getSum() >= rightResultArray.getSum()
	                    && leftResultArray.getSum() >= crossingResultArray.getSum())
	                return leftResultArray;
	            else if (rightResultArray.getSum() >= leftResultArray.getSum()
	                    && rightResultArray.getSum() >= crossingResultArray.getSum())
	                return rightResultArray;
	            else
	                return crossingResultArray;
	        }
	    }
	    /**
	     * 次数据结构归纳了要处理的问题的形式，该形式作为NSU的入口，也就是递归的形式 Original Question Form （OQF）
	     * @author xingzhe
	     */
	    static class TempArray {
	        int low;
	        int high;
	        int[] array;
 
	        public TempArray() {
	        }
 
	        public TempArray(int low, int high, int[] a) {
	            this.setLow(low);
	            this.setHigh(high);
		        this.setArray(a);
	        }
 
	        public int getLow() {
	            return low;
	        }
 
	        public void setLow(int low) {
	            this.low = low;
	        }
 
	        public int getHigh() {
	            return high;
	        }
 
	        public void setHigh(int high) {
	            this.high = high;
	        }
 
	        public int[] getArray() {
	            return array;
	        }
 
	        public void setArray(int[] array) {
	            this.array = array;
	        }
 
	    }
	    /**
	     * 该数据结构是问题的最终形式，就是用户要得到的数据机构 Result Form （RF）
	     * @author xingzhe
	     */
	    static class ResultArray {
	        int low;
	        int high;
	        int sum;
 
	        public ResultArray(int low, int high, int sum) {
	            this.setLow(low);
	            this.setHigh(high);
	            this.setSum(sum);
	        }
 
	        public ResultArray() {
	        }
 
	        public int getLow() {
	            return low;
	        }
 
	        public void setLow(int low) {
	            this.low = low;
	        }
 
	        public int getHigh() {
	            return high;
	        }
 
	        public void setHigh(int high) {
	            this.high = high;
	        }
 
	        public int getSum() {
	            return sum;
	        }
 
	        public void setSum(int sum) {
	            this.sum = sum;
	        }
 
	        @Override
	        public String toString() {
	            return "ResultArray [low=" + low + ", high=" + high + ", sum="
	                    + sum + "]";
	        }
 
	    }
	    /**
	     * 该数据结构是NPU的处理形式，NPU处理之后返回的一定是问题的最终形式  Processing Form（PF）
	     * @author xingzhe
	     */
	    static class SubArray {
	        int low;
	        int high;
	        int middle;
	        int[] array;
 
	        public SubArray(int i, int j, int k, int[] a) {
	            this.setLow(i);
	            this.setHigh(j);
	            this.setMiddle(k);
	            this.setArray(a);
	        }
 
	        public int getLow() {
	            return low;
	        }
 
	        public void setLow(int low) {
	            this.low = low;
	        }
 
	        public int getHigh() {
	            return high;
	        }
 
	        public void setHigh(int high) {
	            this.high = high;
	        }
 
	        public int getMiddle() {
	            return middle;
	        }
 
	        public void setMiddle(int middle) {
	            this.middle = middle;
	        }
 
	        public int[] getArray() {
	            return array;
	        }
 
	        public void setArray(int[] array) {
	            this.array = array;
	        }
 
	    }
 
	    public static void main(String[] args) {
	        int length = 20000000;
	        int[] array = new int[length];
	        int max = 500;
	        int min = -500;
	        for (int i = 0; i < length; i++) {
	            array[i] = (int) Math.round(Math.random() * (max - min) + min);//获取max和min之间的整数
	        }
	        TempArray t = new TempArray();
	        t.setArray(array);
	        t.setLow(0);
	        t.setHigh(length - 1);
	        System.out.println("\r\n"+find_max_subarray(t));
	    }
	}

