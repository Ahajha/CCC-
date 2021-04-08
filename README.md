# CCC++
Custom Containers for C++ Library. Various containers for various use cases.

## Current containers provided:
### cccpp::\[fs_\]\[offset_\]integral_list

This is an amalgamation of std::vector, std::list, and std::unordered_set, with many of the benefits of all of them, with a few small caviats. This container can only hold integral types, due to the way "random access" works here. The container is ordered, like a list, with constant-time access to the front and back, but no random access. The "random access" is not for the elements in terms of the order of the list, but rather for the elements themselves. For example, index 3 is not the 4th element in the list, but instead references the *number* 3. This provides constant time lookup, insertion, and removal of elements, similar to std::unordered_set. Also like std::unordered_set is the lack of support for duplicate values. An integral list has a unit of storage for each element in its range at all times, so it has both a 'size' (the number of elements in the list) and a 'length' (the number of unique elements that can be inserted).

An integral list can be thought of as a std::unordered_set of integral values, but with an order to the elements. It also does not require a hash for the elements.

There are 4 variants of integral lists:  
`cccpp::integral_list<T>`: Supports unsigned integral types only, dynamic size. Range is from 0 to a specified length.  
`cccpp::fs_integral_list<T, N>`: Supports unsigned integral types only, fixed size. Range is from 0 to a specified length.  
`cccpp::offset_integral_list<T>`: Supports signed and unsigned integral types, dynamic size. Both start and end index are specified.  
`cccpp::fs_offset_integral_list<T, N>`: Supports signed and unsigned integral types, fixed size. Both start and end index are specified.  
## Comparisons of containers from the C++ STL versus CCC++ containers:

|                      | ordered? | pop_front() | pop_back() | push_front() | push_back() | random access? |  lookup  | random insert* | random delete* | notes |
| :------------------- | :------: | :---------: | :--------: | :----------: | :---------: | :------------: | :------: | :------------: | :------------: | ----: |
| std::vector          |      yes |         N/A |       O(1) |          N/A |        O(1) |            yes |     O(n) |           O(n) |           O(n) |       |
| std::deque           |      yes |        O(1) |       O(1) |         O(1) |        O(1) |            yes |     O(n) |           O(n) |           O(n) |       |
| std::list            |      yes |        O(1) |       O(1) |         O(1) |        O(1) |             no |     O(n) |           O(1) |           O(1) |       |
| std::set             |       no |         N/A |        N/A |          N/A |         N/A |            N/A | O(log n) |           O(1) |           O(1) | sorted, but not ordered. requires comparison. no duplicates. |
| std::unordered_set   |       no |         N/A |        N/A |          N/A |         N/A |            N/A |     O(1) |           O(1) |           O(1) | requires hashing, equality. no duplicates.|
| cccpp::integral_list |      yes |        O(1) |       O(1) |         O(1) |        O(1) |             no |     O(1) |           O(1) |           O(1) | requires integral types. no duplicates. |

Notes:  
"ordered" is defined to be the concept of being able to treat the container like a sequence. For example, std::list is ordered, because if you append to the end repeatedly, then iterate over it, you will read the items in the same order your wrote them. This is not true for std::set, where the order of iteration will be in ascending order.  
"random insert/delete" is defined to be the action of having an arbitrary iterator somewhere in the middle of a container, and inserting an element in-place (which often shifts the element over one, often via emplace()) or removing (often via erase()) that element.
