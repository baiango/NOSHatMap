# 🔥 NOSHatMap
This is a sorted [512-hashed array tree](https://en.wikipedia.org/wiki/Hashed_array_tree), it's a sorted squared vector with interpolation search that's intended to be used as a hash map, and replace my [NOSMap](https://github.com/baiango/NOSMap). It'll destroys any hash map in load factor because it's 100%.

NOSHatMap stores key-value pairs, 32-bit hashes, and indices shift only. Its design is more closer to a perfect hash function than a hash map because of its slow insertion and deletion time.

NOSHatMap doesn't have probing overhead, but it does have smaller searching overhead. It doesn't have resizing overhead that traditional hash maps have, which must rehash the whole hash map. But, it required all hash to be sorted. In NOSHatMap, the sort is delayed.

There's a thing called array if you want a 90% or higher load factor hash map. Or, design a hash map that only probes once 99% of the time.

# 🤭 Scalability overview
| Operation | Best-Case Time | Average Case Time | Worst-Case Time |
|---|---|---|---|
| **Sorted Insertion** (Insert at arbitrary position) | O(n) | O(n) | O(n) |
| **Sorted Deletion** (Delete from arbitrary position) | O(n) | O(n) | O(n) |
| **Search** (Binary interpolation search) | O(log log n) | O(log log n) | O(log n)[1] |
| **Sort** ([Sapient selection sort](https://en.wikipedia.org/wiki/Selection_sort)[3]) | O(n) | O(N2) | O(N2) |
| **Iteration** (Visit all elements) | O(n) | O(n) | O(n) |
| **Resizing** (Enlarging/shrinking array size)[2] | O(1) | O(√n) | O(n) |

1.	To slow NOSHatMap down to O(n), the attacker must make sure every hash for the element is the same hash. Otherwise, it's usually O(log n) in the worst case. ✅
2.	NOSHatMap will not copy or rehash existing elements when resizing ✅✅✅✅✅, it'll only allocate and deallocate vectors. The only exception for NOSHatMap is if its capacity is smaller than 512, then it'll copy existing elements to the resized leaf. 
3.	NOSHatMap is sorted with [Octal LSD radix sort](https://en.wikipedia.org/wiki/Radix_sort) and selection sort.
	- The prefix sum in the octal LSD radix sort is implemented with AVX2 XOR/cmpeq, movemask, and popcnt.
	- Selection sort is implemented with cmpeq, movemask, BSF, and permutevar.
