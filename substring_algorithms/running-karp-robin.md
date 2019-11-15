# Running Karp Robin

## Prerequisite

Before we come to this algorithm the code must be tokenized see our page on tokenization before this to understand what we mean!

## Algorithm

Given a text _txt\[0 ... n-1\]_ and a pattern _pat\[0 .. m-1\]_ we want to print all occurrences of _pat\[\]_ in _txt\[\]_

```text
Input:  txt[] = "THIS IS SOME TEXT AND IT INCLUDES TEXT"
        pat[] = "TEXT"
Output: Pattern found at index 13
        Pattern found at index 34
```

#### Hashes

RKR use a hash value of the pattern with the hash value of current substring of text, if the hash values match then we do an individual matching of characters.

The following needs to be hashed then:

* The pattern
* All the strings of text of length m

Due to this we need to implement a _rolling hash function_ 

```text
hash(txt[s+1 .. s+m]) = rehash(txt[s + m], hash(txt[s .. s+m-1]))
NOTE: This must occur in O(1)
```

The suggested function calculates an integer value ie "a" = 1, "b" = 2, ..

```text
hash(txt[s+1 .. s+m]) = (d(hash(txt[s .. s+m-1])–txt[s]*h) + txt[s + m]) mod q
hash(txt[s .. s+m-1]) : Hash value at shift s.
hash(txt[s+1 .. s+m]) : Hash value at next shift (or shift s+1)
d: Number of characters in the alphabet
q: A prime number
h: d^(m-1)
```

> This is simple mathematics, we compute decimal value of current window from previous window. For example pattern length is 3 and string is “23456” You compute the value of first window \(which is “234”\) as 234. How how will you compute value of next window “345”? You will do \(234 – 2_100\)_10 + 5 and get 345.

#### [Algorithm](http://courses.csail.mit.edu/6.006/spring11/rec/rec06.pdf)

```python
function RabinKarp(string s[1..n], string pattern[1..m])
    hpattern := hash(pattern[1..m]);
    for i from 1 to n - m + 1
        hs := hash(s[i..i+m-1])
        if hs = hpattern
            if s[i..i+m-1] = pattern[1..m]
                return i
    return not found
```



