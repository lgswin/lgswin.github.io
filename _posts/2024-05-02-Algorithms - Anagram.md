---
layout: post
title: "Understanding Anagram Algorithms: Cracking the Puzzle of Reordered Words"
date: 2024-05-02
categories: [Algorithms]
tags: [Algorithms, Anagram, Python]
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

+ Plus add

An anagram is a word or phrase that is formed by rearranging the letters of another word or phrase, typically using all the original letters exactly once. For example, the word "listen" can be rearranged to form the word "silent."

### Checking for Anagrams

To determine if two strings are anagrams of each other, you can use various methods:

1. **Sorting Method**:
    - Sort the characters of both strings and compare the sorted versions. If they are identical, the strings are anagrams.
2. **Counting Characters**:
    - Count the frequency of each character in both strings and compare the counts.

### Example Implementations

### Sorting Method

```python
def are_anagrams(str1, str2):
    return sorted(str1) == sorted(str2)

# Example usage
print(are_anagrams("listen", "silent"))  # Output: True
print(are_anagrams("hello", "world"))    # Output: False

```

### Counting Characters Method

```python
from collections import Counter

def are_anagrams(str1, str2):
    return Counter(str1) == Counter(str2)

# Example usage
print(are_anagrams("listen", "silent"))  # Output: True
print(are_anagrams("hello", "world"))    # Output: False

```

### Steps for the Counting Characters Method

1. **Create a frequency counter for each string**:
    - Use a dictionary or a `Counter` from the `collections` module to count the frequency of each character in both strings.
2. **Compare the frequency counters**:
    - If the frequency counters are identical, then the strings are anagrams.

### Detailed Example

Consider checking if "anagram" and "nagaram" are anagrams:

### Using the Sorting Method

```python
def are_anagrams(str1, str2):
    return sorted(str1) == sorted(str2)

print(are_anagrams("anagram", "nagaram"))  # Output: True

```

- Sorting both strings results in "aaagmnr".
- Since both sorted strings are identical, "anagram" and "nagaram" are anagrams.

### Using the Counting Characters Method

```python
from collections import Counter

def are_anagrams(str1, str2):
    return Counter(str1) == Counter(str2)

print(are_anagrams("anagram", "nagaram"))  # Output: True

```

- Counting the characters in "anagram" gives `{'a': 3, 'n': 1, 'g': 1, 'r': 1, 'm': 1}`.
- Counting the characters in "nagaram" gives `{'n': 1, 'a': 3, 'g': 1, 'r': 1, 'm': 1}`.
- Since the frequency counts are identical, "anagram" and "nagaram" are anagrams.


The time complexity of checking if two strings are anagrams depends on the method used. Let's analyze the time complexity for the two common methods: sorting and counting characters.

### Sorting Method

### Algorithm:

1. Sort both strings.
2. Compare the sorted strings.

### Time Complexity:

1. **Sorting**:
    - Sorting a string of length n typically takes O(nlogn) time.
        
        nn
        
        O(nlog⁡n)O(n \log n)
        
    - Since we need to sort both strings, this step takes 2×O(nlogn), which simplifies to O(nlogn).
        
        2×O(nlog⁡n)2 \times O(n \log n)
        
        O(nlog⁡n)O(n \log n)
        
2. **Comparison**:
    - Comparing two strings of length n takes O(n) time.
        
        nn
        
        O(n)O(n)
        

Therefore, the overall time complexity is:
O(nlog⁡n)+O(n)=O(nlog⁡n)O(n \log n) + O(n) = O(n \log n)O(nlogn)+O(n)=O(nlogn)

### Counting Characters Method

### Algorithm:

1. Count the frequency of each character in both strings.
2. Compare the frequency counts.

### Time Complexity:

1. **Counting Characters**:
    - Counting the characters in a string of length n takes O(n) time.
        
        nn
        
        O(n)O(n)
        
    - Since we need to count the characters in both strings, this step takes 2×O(n), which simplifies to O(n).
        
        2×O(n)2 \times O(n)
        
        O(n)O(n)
        
2. **Comparison**:
    - Comparing two frequency dictionaries (or counters) of size k (where k is the number of unique characters, typically a constant like 26 for lowercase English letters) takes O(k) time.
        
        kk
        
        kk
        
        O(k)O(k)
        
    - Since k is a constant, this comparison step takes O(1) time in practical scenarios.
        
        kk
        
        O(1)O(1)
        

Therefore, the overall time complexity is:
O(n)+O(1)=O(n)O(n) + O(1) = O(n)O(n)+O(1)=O(n)

### Summary

- **Sorting Method**: O(nlogn)
    
    O(nlog⁡n)O(n \log n)
    
- **Counting Characters Method**: O(n)
    
    O(n)O(n)
    

The counting characters method is more efficient with a linear time complexity O(n)O(n)O(n) compared to the sorting method's O(nlog⁡n)O(n \log n)O(nlogn). Therefore, for checking if two strings are anagrams, the counting characters method is generally preferred for its better performance, especially for larger strings.