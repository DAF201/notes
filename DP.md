# Dynamic Programming: Word Break Problem

**Dynamic Programming** is the idea of collecting and storing the results of subproblems of a large problem to speed up future processing and eventually solve the entire problem efficiently.

In this problem, you are given a string `s` and a list of words `d` as the word dictionary. You need to check whether the string `s` can be split into a combination of words only from the dictionary.

For example:

```python
"applepenapple", ["apple", "pen"]     →  True  
"catsandog", ["cats", "dog", "sand", "and", "cat"]     →  False
```

## Explanation

1. The idea is to solve the problem using bottom-up or top-down dynamic programming.
2. You start with an array `dp[i]`, which indicates whether the substring `s[0:i]` can be segmented.
3. For each possible break point `i`, try to find the next possible breaking point `j`, where:
    - `s[0:j]` can be broken (`dp[j] == True`), and
    - `s[j:i]` is in the dictionary.  
   If both conditions hold, then `s[0:i]` is also breakable.
4. If `dp[len(s)]` is `True`, return `True`.

Bottom-Up Solution

```python
def wordBreak(s: str, wordDict: list[str]) -> bool:
    # For faster lookup
    word_set = set(wordDict)
    n = len(s)

    # dp[i] indicates whether s[0:i] can be segmented
    dp = [False] * (n + 1)
    dp[0] = True  # Base case: empty string is segmentable

    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break

    return dp[n]
```

Top-Down Solution with Memoization

```python
# This approach uses recursion and memoization
def wordBreak(s: str, wordDict: list[str]) -> bool:
    word_set = set(wordDict)
    memo = {}

    # Check if the substring s[start:] can be segmented
    def dfs(start):
        if start == len(s):
            return True  # Reached the end successfully

        if start in memo:
            return memo[start]

        for end in range(start + 1, len(s) + 1):
            prefix = s[start:end]
            if prefix in word_set and dfs(end):
                memo[start] = True
                return True

        memo[start] = False
        return False

    return dfs(0)
```
