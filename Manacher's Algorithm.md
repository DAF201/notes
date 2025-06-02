An algorithm that finds the longest palindromic substring in a string in O(n) time.

The main idea is to use the properties of a palindromic string to reduce redundant calculations.


1. Assume you are iterating through the string, and you have already found a palindromic substring centered at C with radius R. This palindrome spans the indices [C-R:C+R]
2. You continue iterating to another point i, which lies between C and C + R. Then, i will have a mirror point on the other side of C, specifically at index 2C - i
3. Since both i and its mirror are within the bounds of the known palindrome, the minimum palindromic radius at i must be at least the same as that of the mirror point (though possibly truncated by the right boundary).
4. You then attempt to expand the palindrome centered at i. If its new radius exceeds the current R, update C to i and R to the new boundary.

```python
def longestPalindrome(self, s: str) -> str:
    # Insert '#' between characters to ensure the string length is odd
    T = '^' + '#'.join(s) + '&'
    n = len(T)
    P = [0] * n       # P[i] stores the longest palindromic radius centered at index i
    C = 0             # Center of the current longest palindrome
    R = 0             # Right boundary of the current longest palindrome

    for i in range(n):
        mirror = 2 * C - i  # Mirror index of i with respect to center C

        if i < R:
            # Set a minimum starting radius. It's either from the mirror or limited by the current boundary.
            # If the mirror radius is too large, it might exceed the known palindrome boundary.
            P[i] = min(R - i, P[mirror])  

        # Try to expand the palindrome centered at i
        while i + P[i] + 1 < n and i - P[i] - 1 >= 0 and T[i + P[i] + 1] == T[i - P[i] - 1]:
            P[i] += 1

        # Update center and right boundary if the palindrome extends past R
        if i + P[i] > R:
            C = i
            R = i + P[i]

    # Find the maximum radius
    max_len = max(P)
    # Find the center index of the longest palindrome
    center_index = P.index(max_len)

    # Calculate the start index in the original string
    start = (center_index - max_len) // 2
    # Return the longest palindromic substring
    return s[start: start + max_len]
```
