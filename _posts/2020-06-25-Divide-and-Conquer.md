---
layout: post
title: "Divide-and-Conquer"
date: 2020-06-25 12:32:36 -0400
categories: Algorithm Python
excerpt_separator: <!--more-->
---

The divide-and-conquer (DC) strategy solves a problem by 

1.  Breaking the problem into subproblems that are themselves smaller instances of the same type of problem (”divide”), 
2.  Recursively solving these subproblems (”conquer”), 
3.  Appropriately combining their answers (”combine”)

<!--more-->

## The maximum-subarray problem

Problem statement: 

Input: 

​	an array A[1...n] of (positive/negative) numbers. 

Output: 

1.  Indices $i$ and $j$ such that the subarray $A[i...j]$ has the greatest sum of any nonempty contiguous subarray of A, and 

2.  the sum of the values in $A[i...j]$. 

**Note:** Maximum subarray might not be unique, though its value is, so we speak of a maximum subarray, rather than the maximum subarray.

Algorithm Solve by Divide-and -Conquer

-   Generic problem: 

-   -   Find a maximum subarray of $A[low...high]$
    -   with initial call: *low* = 1 and *high* = n 

-   DC strategy: 

	1.  Divide  $A[low...high]$ into two subarrays of as equal size as possible by finding the midpoint *mid*
    2.  Conquer: 
        *   finding maximum subarrays of $A[low...mid]$ and $A[mid + 1...high]$
		*	finding a max-subarray that crosses the midpoint 
    3.	Combine: returning the max of the three

-   Correctness: This  strategy works because any subarray must either lie entirely in one side of midpoint or cross the midpoint. 

Python:

```python
def maxSubArray(nums):
    def help_maxSubArray(l,low,high):
        print(low,high)
        if high==low:	# base case: only one element
            return low,high,l[low]
        else:
            #divide
            mid=math.floor((low+high)/2)
            print(low,mid,high)
            #conquer
            left_low,left_high,left_sum=help_maxSubArray(l,low,mid)
            print('left checked')
            right_low,right_high,right_sum=help_maxSubArray(l,mid+1,high)
            print('right checked')
            x_low,x_high,x_sum=MaxXingSubArray(l,low,mid,high)
            print('xross checked')
            #combine
            if left_sum>=right_sum and left_sum>=x_sum:
                return left_low,left_high,left_sum
            elif right_sum>=left_sum and right_sum>=x_sum:
                return right_low,right_high,right_sum
            else:
                return x_low,x_high,x_sum

    def MaxXingSubArray(l,low,mid,high):
        #find max-subarray of l[i...mid]
        left_sum=-math.inf
        _sum=0
        for i in range(mid,low-1,-1):
            _sum += l[i]
            if _sum>left_sum:
                left_sum=_sum
                max_left=i
        #find max-subarray of l[mid+1...j]
        right_sum=-math.inf
        _sum=0
        for j in range(mid+1,high+1):
            _sum += l[j]
            if _sum>right_sum:
                right_sum=_sum
                max_right=j
        #Return the indices i and j and the sum of two subarrays
        return max_left,max_right,left_sum+right_sum
    
    low,high,out_sum=help_maxSubArray(nums,0,len(nums)-1)
    print(low,high,out_sum)
    return out_sum
```

