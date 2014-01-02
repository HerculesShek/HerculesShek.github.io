---
layout: post
title: DataStructure之最大优先队列的java实现
category: blog
description: java实现的最大优先队列
---

以下是本人以**堆**实现的最大优先队列的java代码，最小优先队列与此思想一致

	/**
	 * DataStructure——最大优先队列
	 * @author Hersules
	 *
	 */
	public class MaxPriorityQuene {
	    private int[] data;
	    private int heap_size;
	    private int capacity;
	    public MaxProorityQuene(int[] a) {
	        this.data = a;
	        this.heap_size = capacity = data.length;
	        build_max_heap(this.data);
	    }
	    public boolean heap_insert(int value) {
	        if (heap_size < capacity) {
	            heap_update_by_index(heap_size,value);
	            heap_size++;
	            return true;
	        }
	        return false;
	    }
	    public int heap_maximum() {
	        return this.data[0];
	    }
	    public int heap_extract_max() throws HeapUnderflowException {
	        if (heap_size < 1)
	            throw new HeapUnderflowException("heap underflow");
	        int max = this.data[0];
	        data[0] = data[heap_size - 1];
	        heap_size--;
	        max_heapify(data, 0);
	        return max;
	    }
	    public void heap_update_by_index(int index, int newValue) {
	        if (data[index] > newValue) {
	            data[index] = newValue;
	            max_heapify(data, index);
	        } else {
	            while (index > 0 && newValue > data[index]) {
	                data[index] = data[parent(index)];
	                index = parent(index);
	            }
	            data[index] = newValue;
	        }
	    }
	    private int parent(int index) {
	        return (int) (Math.floor((index + 1) / 2) - 1);
	    }
	    // 建立一个最大堆,采用自底向上的方式把数组array转换为最大堆
	    public void build_max_heap(int[] a) {
	        int from = (int) Math.floor(heap_size / 2);
	        for (int i = from; i >= 1; i--) {
	            max_heapify(a, i - 1);
	        }
	    }
	    /*
	     * 让堆a中的i结点上的值“堆化”——就是让i结点上的值是以i为根节点的子树的最大值
	     */
	    public void max_heapify(int[] a, int i) {
	        int l = 2 * (i + 1) - 1;
	        int r = 2 * (i + 1);
	        int largest = i;
	        if (l < heap_size && a[l] > a[largest]) {
	            largest = l;
	        }
	        if (r < heap_size && a[r] > a[largest]) {
	            largest = r;
	        }
	        if (largest != i) {
	            Tool.swap(a, i, largest);
	            max_heapify(a, largest);
	        }
	    }
	}
	class HeapUnderflowException extends Exception {
	    private static final long serialVersionUID = 1L;
	    public HeapUnderflowException() {
	        super();
	    }
	    public HeapUnderflowException(String message) {
	        super(message);
	    }
	    public HeapUnderflowException(String message, Throwable cause) {
	        super(message, cause);
	    }
	    public HeapUnderflowException(Throwable cause) {
	        super(cause);
	    }
	}
