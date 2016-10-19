---
layout: post
title: Tight Words. 
tags: [COJ Dynamic Programming Algorithms ]
categories: [Analysis of Algorithms ]
image:
  background: triangular.png
---


# Tight Words Problem

Given an alphabet {0,1,...,k} with $0 \leq k \leq$. We define  a word of length $1 \leq k \leq n$, over this alphabet, to be tight if  the difference between any par of adjacent digits in the word is no major than 1. 

For this problem we will use dynammic programming in the way that we build a table from the bottom up, where we can look the previous answers. 

## Strategy: 

We will look at the ending words, the length of the word and the alphabet chosen. Let us define the following function: 

$$ \#TW(N,K,END)$$

As the number of tight words of length N, over and alphabet {0,1,...,k} (K the last letter), that ends in the letter END. 

## Preparation and  Base Cases 

There are some base cases that we need to look at first, in order to build our solution. 


### When the alphabet only has one letter "0"

```python
	for  n=1 to N+1: 
		dp[(n,0,0)]= 1 
``` 

Because there is only one word, which is produced by $0^*$

### Words of length 1 

```python
	for end =0 to 10 :
		for k = 0 to 10 :
			dp[(1,k,end)]= 1 if end <=k else 0 
```

## The main loop: 

Now we have just to  move along the other cases: 

Let us start by taking every word that ends on 0. Here we have two cases
either we put another zero at the end of the word or we put a 1. So we first take the case when ends in zero and if k is greater or equal to 1, we just add this quantity to that case. 

```python
	
	for n in xrange(2, N + 1):
       for k in xrange(0, K + 1):
           dp[(n, k, 0)] = dp[(n - 1, k, 0)]
           if 1 <= k:
               dp[(n, k, 0)] += dp[(n - 1, k, 1)]

```

*This is in the same loop.* 

Now we have to consider the endings if the end is withing the characters of the alphabet, we can either add the same letter, or add the previous one, and if we are not in the limit we can even add the next letter. 
```python 

	for end in range(1, k + 1):
        dp[(n, k, end)] = dp[(n - 1, k, end)] + \
            dp[(n - 1, k, end - 1)]
        if end + 1 <= k:
            dp[(n, k, end)] += dp[(n - 1, k, end + 1)]

````

At the end, we just need to consider the last extreme case. When we pick up an alphabet that goes all the way to the last character. In this case, similar to the first one, we can either add the same letter or add the previous one, because there are no more letters upfront. 

```python 
	if k == 9:
        dp[(n, k, 9)] = dp[(n - 1, k, 9)] + dp[(n - 1, k, 8)]


```

### Finalization: 

On this stage we just go for the different endings that our alphabet may have and sum up all of them: 

```total: 

	total= sum(dp[N,k,end] for end in range(0,k+1))

```



