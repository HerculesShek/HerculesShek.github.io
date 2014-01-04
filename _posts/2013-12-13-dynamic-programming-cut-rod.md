---
layout: post
title: 动态规划之钢锯切割问题
category: blog
description: 动态规划问题的典型代表
---

动态规划问题首先是分解原始问题为相同形式的子问题，是利用重复的子问题不再重新计算的算法。
钢锯切割是其中一个典型代表，子问题形式为左边不切割，右边的进行切割，首先是朴素的递归形式，时间复杂度是2的n次方，

代码1：

	#-*- coding: utf8 -*-
	import time
	'''钢条切割问题的自顶向下版本，直接进行递归
	   p是长度对应的价格表
	   n是现在的钢条长度
	   求解最大利润'''
	def CUT_ROD(p, n):
	    if n is 0:
	        return 0
	    q = 0
	    for i in xrange(1, n+1):
	        q = max(q, p[i]+CUT_ROD(p, n-i))
	    return q


	p = {0:0,1:1,2:5,3:8,4:9,5:10,6:17,7:17,8:20,9:24,10:30}
	def main():
	    n = 24
	    max_key = max(p.keys())
	    if n > max_key:
	        for i in xrange(max_key+1, n+1):
	            p[i] = 0        #原来的没有p[i]，否则报keyerror
	            p[i] = CUT_ROD(p, i)
	    print n, p[n]

	if __name__ == '__main__':
	    begin = time.time()
	    main()
	    print 'total run time', time.time()-begin
		
用时：

	>>>
	24 70
	total run time 35.2200000286

此方式效率看得出是很低的，有两种方式可以优化上面的方案：
加入备忘机制，就是中途将计算的子问题的最优解存储起来
代码2：

	#-*- coding: utf8 -*-
	import time
	''' “自顶向下”带备忘，是朴素递归带备忘的形式 top-down with memoization '''
	def MEMOIZED_CUT_ROD(p, n):
	    r = []      #用来存储长度为n的最大收益，就是所有子问题的解
	    for i in xrange(0, n+1):    # 0 to n  初始化
	        r.append(-1)
	    return MEMOIZED_CUT_ROD_AUX(p, n, r)
	def MEMOIZED_CUT_ROD_AUX(p, n, r):
	    if r[n]>=0:
	        return r[n]     #这里就是已经存在的长度为n的最大收益，如果存在则直接取出，不再计算
	    if n is 0:
	        q = 0
	    else:
	        q = -1
	        for i in xrange(1, n+1):    # 1 to n
	            q = max(q, p[i] + MEMOIZED_CUT_ROD_AUX(p, n-i, r))
	    r[n]=q
	    return q

	p = {0:0,1:1,2:5,3:8,4:9,5:10,6:17,7:17,8:20,9:24,10:30}
	def main():
	    n = 24
	    max_key = max(p.keys())
	    if n > max_key:
	        for i in xrange(max_key+1, n+1):
	            p[i] = 0        #原来的没有p[i]，否则报keyerror
	    print MEMOIZED_CUT_ROD(p, n)


	f __name__ == '__main__':
	    begin = time.time()
	    main()
	    print 'total run time', time.time()-begin
		
用时：

	>>>
	70
	total run time 0.0120000839233
	
看出优化是很有效的
第二种方式是自底向上的寻找最优解，更加简单而且执行速度更快：
代码3：

	#-*- coding: utf8 -*-
	import time
	''' 自底向上法 bottom-up method '''
	def BOTTOM_UP_CUT_ROD(p, n):
	    r = []          #用来存储长度为n的最大收益，就是所有子问题的最优解
	    for i in xrange(0, n+1):    # 0 to n 初始化
	        r.append(-1)
	    r[0] = 0    #长度为0， 收益为0
	    for j in xrange(1, n+1):
	        q = -1
	        for i in xrange(1, j+1):
	            q = max(q, p[i]+r[j-i])
	        r[j] = q
	    return r[n]
		
	p = {0:0,1:1,2:5,3:8,4:9,5:10,6:17,7:17,8:20,9:24,10:30}
	def main():
	    n = 24
	    max_key = max(p.keys())
	    if n > max_key:
	        for i in xrange(max_key+1, n+1):
	            p[i] = 0        #原来的没有p[i]，否则报keyerror
	    print BOTTOM_UP_CUT_ROD(p, n)
                                                                                                        
	if __name__ == '__main__':
	    begin = time.time()
	    main()
	    print 'total run time', time.time()-begin
		
用时：

	>>>
	70
	total run time 0.0110001564026

第二种方式更加简洁，且没有递归，因为一程序都有递归调用的最大深度的限制，两者渐进时间一致，但是因为少了递归，第二种方式的系数小一些

稍加修改上面的代码，使之不仅返回最大收益，而且打印出最优解

	#-*- coding: utf8 -*-
	import time
	''' 输出最优解的自底向上法 bottom-up method '''
	def EXTENDED_BOTTOM_UP_CUT_ROD(p, n):
	    r, s = [], []   # r用来存储长度为n的最大收益，就是所有子问题的最优解
	                    # s存储对长度为n的钢条切割的最优解对应的第一段钢条的切割长度
	                
	    for i in xrange(0, n+1):    # 0 to n 初始化
	        r.append(-1)
	        s.append(0)
	    r[0] = 0    #长度为0， 收益为0
	    for j in xrange(1, n+1):
	        q = -1
	        for i in xrange(1, j+1):
	            if q < p[i]+r[j-i]:
	                q = p[i]+r[j-i]
	                s[j] = i
	        r[j] = q
	    return r, s

	def PRINT_CUT_ROD_SOLUTION(p, n):
	    r, s = EXTENDED_BOTTOM_UP_CUT_ROD(p, n)
	    print 'max revenue is', r[n]
	    while n>0:
	        print s[n]
	        n = n-s[n]

	p = {0:0, 1:1, 2:5, 3:8, 4:9, 5:10, 6:17, 7:17, 8:20, 9:24, 10:30}
	def main():
	    n = 9
	    max_key = max(p.keys())
	    if n > max_key:
	        for i in xrange(max_key+1, n+1):
	            p[i] = 0        #原来的没有p[i]，否则报keyerror
	    PRINT_CUT_ROD_SOLUTION(p, n)
                
	if __name__ == '__main__':
	    begin = time.time()
	    main()
	    print 'total run time', time.time()-begin
		
输出：

	>>>
	max revenue is 25
	3
	6
	total run time 0.0220000743866
