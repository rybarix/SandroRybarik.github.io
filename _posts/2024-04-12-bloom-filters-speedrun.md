# Bloom filters speedrun

Bloom filters are **space-efficient**, **probabilistic** data structures for testing set membership, using multiple hash functions to map elements to bits in a bit array, allowing for false positives but not negatives, and not supporting deletion.

1. Prepare a bit array of length M
1. Have a N(1) number of hash functions
1. Take your set of items you want to check query against
1. Hash each item with hash functions and set the corresponding bit to 1
1. Now you have your bloom filter

To test if an item is in the set, do following:

1. hash item with all hash functions
1. check for each hash result if there is a set corresponding bit
1. if all bits are 1, the element is **probably**(2) in
1. if a single bit is 0, the element **not** in the set

There can be false positives with bloom filters.

Bloom filters aren't hash map replacement. It is efficient way to test presence of items in the set.

_Example use-case_

Password Strength Checking: Instead of maintaining a large set of all possible weak passwords, a bloom filter can be used to check if a password is likely to be weak. This approach significantly reduces the space required for storing all possible weak passwords.

{% include nlcard.html %}

---

(1) There is a [way to calculate optimal number](https://stackoverflow.com/a/22467497/14914881) of hash functions and other parameters
<br>(2) There can be false positives with bloom filters
