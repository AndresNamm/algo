---
layout: post
title:  "FIND N3 OCCURENCES"
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>



+ [BASED ON INTERVIEWBIT](https://www.interviewbit.com/problems/n3-repeat-number/)


# PROBLEM

~~~
You’re given a read only array of n integers. Find out if any integer occurs more than n/3 times in the array in linear time and constant additional space.

If so, return the integer. If not, return -1.

If there are multiple solutions, return any one.
~~~

# SOLUTION


+ In the solution we keep a datastructure called top2. You can see how this datastructure is implemented below in the code. Its quite simple. Tue top2 datastructure explanation in interviewbit:
    + When we encounter a new element, there are 3 cases possible :
        1. We don’t have 2 elements yet. So add this to the list with count as 1.
        2. This element is different from the existing 2 elements. As we said before, we have 3 distinct numbers now. Removing them does not change the answer. So decrement 1 from count of 2 existing elements. If their count falls to 0, obviously its not a part of 2 elements anymore.
        3. The new element is same as one of the 2 elements. Increment the count of that element.

## EXPLANATION


+ To understand the solution you need to handle maintaining the data in top2 rather as a removal-process.
+ Lets say in array there is an element **K** which is a top element and which occurs in the array more than $$\frac{n}{3}+1$$ times.
+ From this it follows that the next most occuring element can occur maximum $$\frac{n}{3}$$ times which means that there can be maximum
$$\frac{2n}{3}-1-\frac{n}{3}=\frac{n}{3}-1$$ unique combinations of three which is less than $$\frac{n}{3}+1$$ (The occurences of K)
+ **If some element occurs more than unique combos it must exist in top2 after passing through the array a it has been removed less from top2 than it has been inserted to top2**


## CODE


~~~python
class Solution:
    # @param A : tuple of integers
    # @return an integer
    def repeatedNumber(self, A):
        n= len(A)
        arr=A
        count1 = 0
        count2 = 0
        first = sys.maxsize
        second = sys.maxsize

        for i in range(0, n):  

            # if this element is
            # previously seen,  
            # increment count1.
            if (first == arr[i]):
                count1 += 1

            # if this element is
            # previously seen,  
            # increment count2.
            elif (second == arr[i]):
                count2 += 1

            elif (count1 == 0):
                count1 += 1
                first = arr[i]

            elif (count2 == 0):
                count2 += 1
                second = arr[i]

            # if current element is  
            # different from both
            # the previously seen  
            # variables, decrement
            # both the counts.
            else:
                count1 -= 1
                count2 -= 1

        count1 = 0
        count2 = 0

        # Again traverse the array
        # and find the actual counts.
        for i in range(0, n):  
            if (arr[i] == first):
                count1 += 1

            elif (arr[i] == second):
                count2 += 1


        if (count1 > n / 3):
            return first

        if (count2 > n / 3):
            return second

        return -1
~~~
