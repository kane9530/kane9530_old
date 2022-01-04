---
layout: post
title: (Sliding-Window) Longest substring with K distinct characters
tags: algorithms sliding-window 
toc: true
comments: true
analytics: true
---
# Problem statement

## Description

Given a string `S`, find the length of the longest substring `T` that contains at most `k` distinct characters. If there is no possible substring then print -1.

## Example 1 - Shorter S

> Input: S = "eceba" , k = 3
>
> Output: 4
>
> Explanation: T = "eceb" is the longest substring with k = 3 distinct characters: 'e', 'c' and 'b'. An example substring of length 4 that fails this condition is "ceba" which has 4 distinct characters. A

## Example 2 - Longer S

>Input: S = "aabacbebebe", k = 3
>
>Output: 7
>
>Explanation: T = "cbebebe" is the longest substring with k = 3 distinct characters: "c", "b", "e". An example of a substring of length 7 that fails this condition is "aabacbe" which has k = 4 distinct characters: "a", "b", "c", "e". An example of a substring of shorter length that meets the condtion is "abac" which has k = 3 distinct characters: "a", "b", "c".


## Example 3 - Edge case

>Input: S = "aaaa", k = 2
>
>Output: -1
>
>Explanation: There's no substring with k = 2 distinct characters.


# Solution
We will solve this problem with the sliding window (dynamically resizing) technique. 

We can identify that this is the technique of choice because:

1. Sliding windows are useful for tackling problems involving strings/lists/linked lists/arrays i.e. data structures which can be iterated sequentially, involving a contiguous sequence of elements.

2. Computation of some value that involves the words: *max, min, longest, shortest, average, contained*. 

3. Our problem asks us to find the *longest* substring, so the window has to be tunable (as opposed to the fixed size sliding window approach).
 
## Building blocks

1. Sliding window (int): `start_pointer` and `end_pointer` variables. The dynamically resizing sliding window can be implemented via the two pointers + criteria method. The index of `end_pointer` is controlled by an outer for loop, whereas the index of `start_pointer` is controlled by an inner while loop / if conditional clause that addresses a criteria. Both pointers *only* move rightwards such that the sliding window expands and shrinks like an undulating caterpillar. 

2. Hashmap (dict): The dictionary data structure will store information regarding the number of distinct characters. We can store either the number of character counts (See Method 2a) or the largest/most recent indices of each character (See Method 2b). Importantly, the hashmap is updated as the `start_pointer` moves rightwards. Thus, the logic of the update synchs the movement of the `start_pointer` with the criteria.  

3. Max_length (int): The `max_len` variable stores the solution of the problem. It is updated at each iteration of the outer for loop to retain the largest value so as to provide the *longest*  possibie substring.
 
## Strategy

Expansion phase - incrementing `end_pointer` 

With `start_pointer` and `end_pointer` initialised at the first character of `S`, the sliding window is expanded until we meet the condition *k = K*. This expansion is carried out by an outer for loop which increments the index of `end_pointer`, thus dragging it rightwards whilst the `start_pointer` is held in place. Once the condition is exceeded (*k = K+1*), we enter the shrinkage phase.

Shrinkage phase - incrementing `start_pointer`

In this phase, the sliding window is shrinked until we meet the condition *k = K*. This shrinkage is carried out by an inner *while loop* which increments the index of `start_pointer` thus dragging it rightwards. Once the condition is again reached (*k = K*), we return to the greedy expansion phase, with `start_pointer` at the newly incremented index position. 

Stopping condition: When the `end_pointer` tries to point to an index greater than the length of string `S`. This means that the entire string `S` has been traversed.

Return value: If condition is met, return `max_length`. If `max_length` = -1, this means that no substring has been found that meets the condition *k = K*. 

## Implementation

### Method 1: Brute force approach

Time complexity: *O(n<sup>3</sup>)*. 

The double for loops generates all the possible substrings at O(n<sup>2</sup>). To see this, notice that the total number of substrings is $n \choose 2 $ and thus 

$$ \frac{n(n+1)}{2} \approx \frac{n^{2}+1}{2} = O(n^{2}) $$ 

Then, an extra O(n) to count the number of distinct characters, leading to O(n<sup>2</sup> * n) = O(n<sup>3</sup>)

If a hashtable or dictionary is used, due to its constant access time, the problem reduces to O(n<sup>2</sup>).

Space complexity: *O(n)*

### Method 2a: Sliding window (Store char count)

```python
def longestKSubstr(self, s, k):
    start_pointer = 0 
    max_length = -1 # Setting default to -1 if no condition met
    mydict = {}

    # Outer for loop to increment end_pointer.
    # First, update the dictionary.
    # One can also use defaultdict from the Collections library. 
    
    for end_pointer, char in enumerate(s):
        if char not in mydict:
            mydict[char] = 1
        else:
            mydict[char] += 1
            
    # Inner while loop to increment start_pointer 
    # if condition is exceeded.
    
        while len(mydict) > k:
        
        # Remove character from dict if start_pointer
        # points to the only entry. Else, decrement by 1
        
            if mydict[s[start_pointer]] == 1:
                del mydict[s[start_pointer]]                
            else:
                mydict[s[start_pointer]] -= 1
            start_pointer += 1
            
 	# Only update max_length if condition is met
 	
        if len(mydict) == k:
            max_length = max(max_length, end_pointer - start_pointer + 1)
    return max_length

```

<figure>
<img src = "/assets/images/040122_longest_substring_k_distinct.gif">
<figcaption>
Gif: Animation illustrating the detailed steps of Method 2a (dynamically resized sliding window which stores the character counts in a dictionary). In this animation, S: 'Excellent', k = 3. 
</figcaption>
</figure>

### Method 2b: Sliding window (Store char index)

```python

def longestKSubstr(self, s, k):
    start_pointer = 0 
    max_length = -1
    mydict = {}
    
    for end_pointer, char in enumerate(s):
        # Always include the end_pointer index 
        mydict[char] = end_pointer
            
       	# If we exceed the condition
       	# Search for the smallest index and remove the character
       	# Note that for identical characters, 
       	# only the most recent character index is kept
       	
        if len(mydict) > k:
            min_left = min(mydict.values())
            start_pointer = min_left
            del mydict[s[start_pointer]]
            start_pointer += 1 #To move past the removed character.
            
        if len(mydict) == k:
            max_length = max(max_length, end_pointer - start_pointer + 1)
    return max_length
```

Time complexity: *O(n)*. Although there are two pointers, each pointer traverses the list only once. Hence, the number of operations has an upper bound of n + n = 2n and so the algorithm is O(n). Therefore, the sliding window approach only searches a subset of possible substrings.

Note that, for an algorithm to be O(n):

> In the case of algorithms that rely on list traversal it does not mean that they must traverse the list only once. It means they must traverse the list *at most a times and that a does not depend on the list length*. - Olivier Melançon, stackoverflow

In contrast, a double for loop involves a nested iteration with a pointer moving forwards and backwards, with the number of times dependent on the list length. This brute force approach checks for all substrings.

Space complexity: *O(1)*. We store at most k+1 characters in the hashmap. 

# Summary schematic
<figure>
<img src = "/assets/images/040122_schematic.png">
<figcaption>
Figure: Code/Schematic for dynamic resizing sliding window
</figcaption>
</figure>

# Useful resources

## Code it yourself
<a href = "https://practice.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1" target = "_blank"> Geeksforgeeks </a>

## Overview of sliding window technique

<a href = "https://www.youtube.com/watch?v=MK-NZ4hN7rs&ab_channel=RyanSchachte" target = "_blank"> https://www.youtube.com/watch?... </a>

<a href = "https://zengruiwang.medium.com/sliding-window-technique-360d840d5740" target = "_blank">
https://zengruiwang.medium.com/sliding-window-technique... </a>

## Other answers
<a href = "https://iq.opengenus.org/longest-substring-without-repeating-characters/" target = "_blank"> Reviewing 3 approaches </a>

<a href = "https://stackoverflow.com/questions/49638176/time-complexity-of-find-the-longest-substring-with-k-unique-characters" target = "_blank"> Why is the time complexity O(n) and not O(n<sup>2</sup>)? </a>


