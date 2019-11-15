# Greedy String Tiling

## Prerequisite

Before we come to this algorithm the code must be tokenized see our page on tokenization before this to understand what we mean!

## Algorithm

Here is the pseudo code for [Greedy String Tiling ](https://page.mi.fu-berlin.de/prechelt/Biblio/jplagTR.pdf)and we will go over this later

{% code title="gst.pseudo" %}
```text
Greedy_String_Tiling(string a, string b)
{
    Tiles = {};
    
    do
    {
        MaxMatch = MinimumMatchLength;
        Matches = {};
        Forall UnmarkedTokens A_a in A
        {
            Forall UnmarkedTokens B_b in B
            {
                j = 0;
                 
                while(A_a+j == B_b+j &&
                      Unmarked(A_a+j) && Unmarked(B_b+j))
                    j++;
                
                if(j == MaxMatch)
                    Matches = Matches xor Match(a, b, j);
                else if(j > MaxMatch)
                {
                    Matches = {Match(a,b,j)};
                    MaxMatch = j;
                }
            }
        }
        Forall Match(a,b,MaxMatch) in Matches
        {
            For j = 0 ... (MaxMatch - 1)
            {
                Mark(A_a+j);
                Mark(B_b+j);
            }
            Tiles = Tiles (union) Match(a,b,MaxMatch);
        }
    } while(MaxMatch > MinimumMatchLength);
    
    return Tiles;
}
```
{% endcode %}

### Phase 1

The two strings are searched for the biggest contiguous matches. This occurs in the 3 nested loops

1. Iterates over all the tokens in string A
2. Compares this token T with every token in B
   1. If Identical
      1. Attempt to extend match as far as possible

This attempts to collect the set of all longest common substrings

### Phase 2

Marks all matches of maximal length from phase 1. This marking tells Phase 1 in subsequent iterations to ignore these matches. This guarentees that every token will only be used in one match

### Resultant

$$
sim(A,B) = \frac{2*coverage(tiles)}{|A| + |B|}
$$

$$
coverage(tiles) = \sum_{match(a,b,length)\in tiles}{length}
$$



