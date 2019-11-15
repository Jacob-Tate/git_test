# Running Karp Robin - Greedy String Tiling

## Terms

#### maximal - match

* TODO: Define

#### tile

* TODO: Define

#### minimum - match - length

* TODO: Define

### Greedy algorithm for string tiling

This algorithm like most greedy algorithms involves multiple passes, each pass having two phases.

1. _Scanpattern_ phase
   * All _maximal-matches_ of a certain size or greater are collected
     * size = _search-length_
2. Markstring phase
   * _maximal-matches_ are taken, one at a time starting with the longest and tested to see whether they have occurred in a sibling tile. If not a tile is created by marking the two substrings.

When all _maximal-matches_ have been dealth with, a new, smaller search length is chosen

```text
search-length s := inital-search-length
stop := false
do
    largest-match := scanpattern(s)
    if largest-match > 2 * s then s := largest-match
    else
        markstring(s)
        if s > 2 * minimum-match-length then s := s / 2
        else if s > minimum-match-length then s:= minimum-match-length
        else stop := true
while stop == false
```

{% hint style="info" %}
In this Greedy algorithm we use _scanpattern_ to implement Running Karp-Rabin
{% endhint %}

#### How we can expand on this

The basis of Karp-Robin is as follows

* If a string of length _s_ starting at _t_ has a hash value
  * The hash value for a string of length _s_ starting at _t + 1_ can be calculated using a simple recurrence relation
* If the length of the patter string is $$|P|$$ 
  * The hash value of every sub-string of length $$|P|$$ from the text string is compared with the hash-value of the pattern string
* If two hash values are identical
  * The pattern and text substrings are compared element-by-element

The improvement made in the Running Karp-Robin version are as follows

* Instead of a single hash for the entire pattern string
  * A hash value is created for each unmarked substring of length s of the pattern string
    * $$P_p ... P_{p+s-1}$$ in the range $$1...|P|_{-s}$$ 
  * Hash values are also created for each unmarked substring of the length _s_ in the text string
* Each hash-value for the pattern string is then compared with the hash values for the _text string_ for those pairs of pattern and text hash values found to be equal
  * These hash matches have a potential to be text matches now
* A hash table of the _text string_ Karp-Rabin hash values is used to reduce from $$O(n^2)$$ to $$O(n)$$ 
  * Rather than scanning the entire text string for the matching hash-values the pattern is itself hashed and a hash table search returns the starting positions of all text substrings of the length s with the same hash values.

{% hint style="info" %}
After a successful match of both the Karp-Rabin hash values and the actual substring the element-by-element matching must occur to prevent cases of collisions
{% endhint %}

#### How this is implemented

```text
starting at the first unmarked token of T, for each unmarked T[t] do
    if distance to next tile <= s then
        advance t to first unmarked token after the next tile
    else
        create the KR hash-value for substring T[t] ... T[t+s-1] and add to the hashtable
    end
end

starting at the first unmarked token of P, for each unmarked P[p] do
    if distance to next tile <= s then
        advance p to first unmarked token after next tile
    else
        create the KR hash-value for substring P[p] ... P[p+s-1]
        check hashtable for hash of KR hash-value
        
        foreach hash-table entry with equal hashed KR hash-value do
            k := s
            
            while P[p+k] = T[t+k] AND unmarked(P[p+k]) AND unmarked(T[t+k]) do
                k := k + 1 // extend match until a non-match or element marked
            end
            
            if k > 2 * s then
                return k
            else
                record new maximal-match
            end
        end
    end
end

return length of longest maximal-match
```

{% hint style="info" %}
The structure we use to record the maximal matches is in this case a double-linked-list of queues where each queue records maximal-matches of the same length and the list of queues is ordered by decreasing length. A pointer can also be kept to the queue onto which the most recent maximal match was appended because there is a high change that the next maximal-match will be a similar length to the last therefore will be appended nearby
{% endhint %}

#### Markstrings

```text
starting at the top queue, while there is a non-empty queue do
    if the curent queue is empty then
        drop to next queue // corresponding to smaller maximal-matches
    else
        remove match(p, t, L) from qeueu // assume the length of matches in the current queue is L
        
        if match not occluded then
            if for all [j:0 ... s-1], P[p+j] = T[t+j] then // match is not a hash collision
                for j := 0 to L - 1 do
                    mark_token(P[p+j])
                    mark_token(T[t+j])
                end
                length-of-tokens-tiled := length-of-tokens-tiled + L
            end
        else if L - L[occluded] >= s then // The unmarked part remaining of the maximal-match
            replace unmarked portion to list of queues
        end
    end
end
```

{% hint style="info" %}
The test of whether maximal-match is really a match has been deffered to markstring from scanpattern where it normally resides
{% endhint %}

Worst case is in this case $$O(n^3)$$ with n being the sum of the length of the input strings. The being noted these cases are extremely rare meaning the cure-fitting complexity fits to $$O(n^{1.12})$$ 

