---
layout: post
title: 算法导论5.1-3解答
category: blog
description: 习题练习
---

java代码：

	public class RandomOneOrZero {
	    public static int get() {
	        int a = 0;
	        int b = 0;
	        while (a == b) {
	            a = getBase();
	            b = getBase();
	        }
	        return a;
	    }
	    //这个函数是模拟的，每次运行的时候p的值是固定的但是假设对外是不可知的
	    private static int getBase() {
	        double p = 0.1;// 这里的p值每次运行的时候可以随意设置为0到1之间的数字
	        double ref = Math.random();
	        if (ref > p)
	            return 1;
	        return 0;
	    }
	}
	
算法的期望运行时间需要p值来确定
