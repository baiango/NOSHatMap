# 🔥 NOSHatMap
This is a sorted [512-hashed array tree](https://en.wikipedia.org/wiki/Hashed_array_tree), it's a sorted squared vector with interpolation search that's intended to be used as a hash map, and replace my [NOSMap](https://github.com/baiango/NOSMap). It'll destroys any hash map in load factor because it's 100%.

NOSHatMap stores key-value pairs and 32-bit hashes only. Its design is more closer to a perfect hash function than a hash map.

NOSHatMap doesn't have probing overhead, but it does have smaller searching overhead. It doesn't have resizing overhead that traditional hash maps have, which must rehash the whole hash map. But, it required all hash to be sorted. In NOSHatMap, this is lazy sorted.

There's a thing called array if you want a 90% or higher load factor hash map. Or, design a hash map that only probes once 99% of the time.

# 🤭 Scalability overview
| Operation | Best-Case Time | Average Case Time | Worst-Case Time |
|---|---|---|---|
| **Sorted Insertion** (Insert at arbitrary position) | O(n) | O(n) | O(n) |
| **Sorted Deletion** (Delete from arbitrary position) | O(n) | O(n) | O(n) |
| **Search** (Binary interpolation search) | O(log log n) | O(log log n) | O(log n)[1] |
| **Sort** (WikiSort[3]) | O(n) | O(n log n) | O(n log n) |
| **Iteration** (Visit all elements) | O(n) | O(n) | O(n) |
| **Resizing** (Doubling/shrinking array size)[2] | O(√n) | O(√n) | O(√n) |

1.	To slow NOSHatMap down to O(n), the attacker must make sure every hash for the element is the same hash. Otherwise, it's usually O(log n) in the worst case.
2.	NOSHatMap will not copy or rehash existing elements when resizing, it'll only allocate and deallocate vectors.
3.	The blocks are sorted with LSD radix sort rather than insertion sort.
