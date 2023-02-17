---
title: LeetCode 2131. Longest Palindrome by Concatenating Two Letter Words
excerpt: Medium
tags:
  - Greedy
comments: true
---

## Problem

Can you solve this real interview question? Longest Palindrome by Concatenating Two Letter Words - You are given an array of strings words. Each element of words consists of two lowercase English letters.
Create the longest possible palindrome by selecting some elements from words and concatenating them in any order. Each element can be selected at most once.

Return the length of the longest palindrome that you can create. If it is impossible to create any palindrome, return 0.

A palindrome is a string that reads the same forward and backward.

 

Example 1:

Input: words = ["lc","cl","gg"] Output: 6 Explanation: One longest palindrome is "lc" + "gg" + "cl" = "lcggcl", of length 6. Note that "clgglc" is another longest palindrome that can be created.

Example 2:

Input: words = ["ab","ty","yt","lc","cl","ab"] Output: 8 Explanation: One longest palindrome is "ty" + "lc" + "cl" + "yt" = "tylcclyt", of length 8. Note that "lcyttycl" is another longest palindrome that can be created.

Example 3:

Input: words = ["cc","ll","xx"] Output: 2 Explanation: One longest palindrome is "cc", of length 2. Note that "ll" is another longest palindrome that can be created, and so is "xx".

 

Constraints:

1 <= words.length <= 105
words[i].length == 2
words[i] consists of lowercase English letters.

## Solution

### Case Study

There are two cases can occur. 1. word is a palindrome itself. 2. word is not a palindrome. In first case, we should focus on the number of palindrome words. The only one palindrome word whose number is odd can be used in the longest palindrome. All others palindrome words should be paired. In second case, all paired non-palindrome words can be used in the longest palindrome.

### Source Code

[<i class='fa fa-address-book' aria-hidden='true'></i> Read in Github](https://github.com/Byunk/LeetCode/blob/master/2131-longest-palindrome-by-concatenating-two-letter-words/2131-longest-palindrome-by-concatenating-two-letter-words.cpp){: .btn .btn--info}

```cpp
class Solution {
public:
    int longestPalindrome(vector<string>& words) {
        // Two cases should be considered
        // 1. symmetric case
        // The most frequent symmetric cases can be used on maximum length Palindrome
        // Then even numbers of remained symmetric cases can be used.
        // 2. unsymmetric case
        // only one case whose number of words is odd can be used
        int n = words.size();
        unordered_map<string, int> sym;
        unordered_map<string, int> unsym;
        int num_unsym = 0;

        for (int i = 0; i < n; i++) {
            string s = words[i];
            if (s[0] == s[1]) {
                if (sym.find(s) != sym.end()) {
                    sym[s]++;
                } else {
                    sym[s] = 1;
                }
            } else {
                string reversed = "";
                reversed += s[1];
                reversed += s[0];

                if (unsym.find(reversed) != unsym.end()) {
                    unsym[reversed]--;
                    if (unsym[reversed] == 0) {
                        unsym.erase(reversed);
                    }
                    num_unsym++;
                    continue;
                }

                if (unsym.find(s) != unsym.end()) {
                    unsym[s]++;
                } else {
                    unsym[s] = 1;
                }
            }
        }

        // symmetric case
        int num_sym = 0;
        int num_odd = 0;
        for (auto it : sym) {
            num_sym += it.second;
            if (it.second % 2 == 1) {
                num_odd++;
            }
        }

        if (num_odd > 1) {
            num_sym -= num_odd - 1;
        }

        return 2 * num_sym + 4 * num_unsym;
    }
};
```