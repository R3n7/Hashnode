## LC patterns - Sliding Window 1

Let's start this post with a Programming Question , How do u find average of all contigous subarrays of size 'k' in a given array.

To lay it out in a simple way,
Find the average of next k elements in the array.

The naive apporach:

we can have two for loops to go through the array.and find the average of the sum.

```py
arr = [1, 3, 2, 6, -1, 4, 1, 8, 2]
k=5
list1 =[ ]
for i in range(len(arr)-k+1):
    sum =0 
    for j in range(i,i+k):
        sum+=arr[j]
    list1.append(sum1/k)
print(list1)
```

This does the job but it is slow as there are two for loops.<br>
Now how can we optimize this?<br>
See the way the sum is calculated everytime. it is intialized as zero and then we add up the elements.let's take sum1,sum2.<br>
sum1 we will calculate the sum of first k elements and in sum2 will do the second k elements.<br>
```
SUM1 = 1+3+2+6-1
SUM2 = 3+2+6-1+4
```

What do we see common in both sums?<br>
the 3+2+6-1 in both of them . so we waste a lot of time calculating this twice.<br> 
Can we somehow reuse the `sum` we have calculated for the overlapping elements?<br>
So how we optimize this,<br>
		Dumroll...Sliding Window Technique!<br>
Imagine every subarray as a sliding window , so 1+3+2+6-1 is a window and the next one 3+2+6-1+4. Now we need to slide the window to the next one.<br>


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652616879933/4cnkIvdv6.png align="left")

How do we do that?<br>
Simple , Subtract the starting element of the window and add the next element to the window.<br>
Code:<br>
```py
winstart=0
winsum=0
for winend in range(len(arr)):
	sum+=arr[winend]
	if winend>=k-1:
		list1.append(winsum//k)
		winsum-=arr[winstart]
		winstart+=1
return list1
```

Time to solve some problems:<br>

<h3>Maximum Sum Subarray of Size K</h3>
<h4>Problem Statement</h4>
Given an array of positive numbers and a positive number ‘k’, find the maximum sum of any contiguous subarray of size ‘k’.

```
Input: [2, 1, 5, 1, 3, 2], k=3 
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
```

I will leave the bruteforce approach to you.<br>

Now the only Thing we will change is the average of the subarray to sum of the subarray and calculate the maxsum of the subarray.<br>
Code:
```py
winstart=0
winsum=0
for winend in range(len(arr)):
	winsum+=arr[winend]
	if winend >=k-1:
		maxsum = max(maxsum,winsum)
		winsum-=arr[winstart]
		winstart+=1
return maxsum
```

I will end this bit with a question.<br>
<h3> Smallest Subarray with a given sum</h3>
<h4>Problem Statement</h4>
Given an array of positive numbers and a positive number ‘S’, find the length of the smallest contiguous subarray whose sum is greater than or equal to ‘S’.
Return 0, if no such subarray exists.<br>

```
Input: [2, 1, 5, 2, 3, 2], S=7 
Output: 2
Explanation: The smallest subarray with a sum great than or equalto '7' is [5, 2].
```

Try This out!<br>
It has a similar approach to the above ones.i will share the solution to this in the next post.<br>
Hope You Learned Something New!!<br>
Join me on this journey to Conquer Leetcode patterns and DSA.<br> 
Useful links:<br>
[https://seanprashad.com/leetcode-patterns/](https://seanprashad.com/leetcode-patterns/)

