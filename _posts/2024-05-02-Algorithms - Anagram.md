---
layout: post
title: "Understanding Anagram Algorithms: Cracking the Puzzle of Reordered Words"
date: 2024-05-02
categories: [Algorithm]
tags: [Algorithm, Anagram, Python]
---

## **What is an Anagram?**

An anagram is a word or phrase formed by rearranging the letters of another word or phrase. The key characteristic of anagrams is that all the letters must be used exactly once. For example, "listen" and "silent" are anagrams of each other, as they contain the same letters rearranged in a different order.

## **Anagram Algorithm in Action**

One of the simplest and most effective ways to check for anagrams is by sorting the characters in each string and comparing the sorted results. Let's take a closer look at the Python implementation of this algorithm:

```python
def isAnagram(s: str, t: str) -> bool:
    sorted_s = ''.join(sorted(s))
    sorted_t = ''.join(sorted(s))
    if sorted_s == sorted_t:
        return True
    else:
        return False
```

## **The Hash Table Advantage**

Hash tables, also known as dictionaries in Python, offer a convenient way to store and retrieve key-value pairs with near-constant time complexity. By utilizing a hash table, we can efficiently track the frequency of each character in a string and compare the results to determine whether two strings are anagrams.

```python
def isAnagramHash(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False
    
    # Initialize hash table to track character frequencies
    hashMap = {'a': 0, 'b': 0, 'c': 0, 'd': 0, 'e': 0, 'f': 0, 'g': 0, 
    'h': 0, 'i': 0, 'j': 0, 'k': 0, 'l': 0, 'm': 0, 'n': 0, 'o': 0, 
    'p': 0, 'q': 0, 'r': 0, 's': 0, 't': 0, 'u': 0, 'v': 0, 'w': 0, 
    'x': 0, 'y': 0, 'z': 0}
    
    # Update character frequencies for string s
    for char in s:
        hashMap[char] += 1
        
    # Decrease character frequencies for string t
    for char in t:
        hashMap[char] -= 1

    # Check if all character frequencies are zero
    for value in hashMap.values():
        if value != 0:
            return False
        
    return True
```

## **Efficiency and Complexity Analysis**

Using a hash table to track character frequencies allows us to achieve linear time complexity O(n), where n is the length of the strings **`s`** and **`t`**. This is significantly more efficient than sorting-based approaches, especially for large strings. Additionally, the space complexity remains O(1) as the size of the hash table remains constant.