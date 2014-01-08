---
layout: post
title: 动态规划之矩阵链乘法问题
category: blog
description: 动态规划之矩阵链乘法的Python代码的实现
---

将穷举所有情况的指数级运行时间降为上限为n^3
代码：

	#-*- coding: utf-8 -*-
	import sys, time
	def gk(i,j):
	    return str(i)+','+str(j)
	def matrix_chain_order(p):
	    n = len(p)-1
	    m, s = {}, {}
	    for i in xrange(1, n+1):
	        m[gk(i, i)] = 0
	    for l in xrange(2, n+1):
	        for i in xrange(1, n-l+2):
	            j = i+l-1
	            m[gk(i, j)] = sys.maxint
	            for k in xrange(i, j):
	                q = m[gk(i, k)]+m[gk(k+1, j)]+p[i-1]*p[k]*p[j]
	                if q<m[gk(i, j)]:
	                    m[gk(i, j)] = q
	                    s[gk(i, j)] = k
	    return m, s
	def get_optimal_parens(s, i, j):
	    res = ''
	    if i == j:
	        return "A"+str(j)
	    else:
	        res += "("
	        res += get_optimal_parens(s, i, s[gk(i, j)])
	        res += get_optimal_parens(s, s[gk(i, j)]+1, j)
	        res +=  ")"
	        return res
	def main():
	    p = [30,35,15,5,10,20,25,5,16,34,28,19,66,34,78,55,23]
	    m, s = matrix_chain_order(p)
	    print '总共的乘法次数是：', m[gk(1, len(p)-1)]
	    print '矩阵链乘法的方案是', get_optimal_parens(s, 1, len(p)-1)
	if __name__ == '__main__':
	    b = time.time()
	    main()
	    print 'total run time is:', time.time()-b

结果为：

	>>>
	总共的乘法次数是： 84115
	矩阵链乘法的方案是 ((A1(A2(A3(A4(A5(A6A7))))))((((((((A8A9)A10)A11)A12)A13)A14)A15)A16))
	total run time is: 0.0369999408722

朴素的递归算法是：

	#-*- coding: utf-8 -*-
	import sys, time
	''' 计算第i到第j个矩阵链相乘的乘法次数 '''
	def recursive_matrix_chain(p, i, j):
	    if i == j:
	        return 0
	    m = sys.maxint
	    for k in xrange(i, j):
	        q = recursive_matrix_chain(p, i, k) + recursive_matrix_chain(p, k+1, j) + p[i-1]*p[k]*p[j]
	        if q < m:
	            m = q
	    return m
	def main():
	    p = [30,35,15,5,10,20,25,5,16,34,28,19,66,34,78,55,23]
	    print recursive_matrix_chain(p, 1, len(p)-1)
	if __name__ == '__main__':
	    b = time.time()
	    main()
	    print 'total run time is:', time.time()-b

结果为：

	>>>
	84115
	total run time is: 11.9000000954

这种朴素递归的方式的时间运行复杂度是2^n，指数级的，因为这个问题满足动态规划的要求：最优子结构，子问题无关且重叠，所以这种朴素的递归方式可以用加入备忘的方式来实现动态规划的算法：

	#-*- coding: utf-8 -*-
	import sys, time
	gk = lambda i,j:str(i)+','+str(j)
	MAX = sys.maxint
	def memoized_matrix_chain(p):
	    n = len(p)-1
	    m = {}
	    for i in xrange(1, n+1):
	        for j in xrange (i, n+1):
	            m[gk(i, j)] = MAX
	    return lookup_chain(m, p, 1, n)
	def lookup_chain(m, p, i, j):
	    if m[gk(i, j)] < MAX:
	        return m[gk(i, j)]
	    if i == j:
	        m[gk(i, j)] = 0
	    else:
	        for k in xrange(i, j):
	            q = lookup_chain(m, p, i, k) + lookup_chain(m, p, k+1, j) + p[i-1]*p[k]*p[j]
	            if q < m[gk(i, j)]:
	                m[gk(i, j)] = q
	    return m[gk(i, j)]
	def main():
	    p = [30,35,15,5,10,20,25,5,16,34,28,19,66,34,78,55,23]
	    print memoized_matrix_chain(p)
	if __name__ == '__main__':
	    b = time.time()
	    main()
	    print 'total run time is:', time.time()-b

结果为：

	>>>
	84115
	total run time is: 0.0209999084473


