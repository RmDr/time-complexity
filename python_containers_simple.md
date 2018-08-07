For the extended version look [./python_containers.md](python_containers.md)

All time complexities are amortized and averaged if possible.

|                  | implementation     |add element                      | find element  | remove element         | indexing |
| ------           | --------------     |:---------:                      | :----------:  | :--------:             | :-----: |
|list              | dynamic array      |append: O(1), insert: O(n)       |O(n)           |pop: O(1), remove: O(n)  |O(1)|
|tuple             | array              |-                                |O(n)           |pop: O(1), remove: O(n)  |O(1)|
|set               | hashtable          |O(1)                             |O(1)           |O(1)                    |-|
|frozenset         | hashtable          |-                                |O(1)           |-                       |-|
|dict              | hashtable          |O(1)                             |O(1)           |O(1)                    |-|
|collections.deque | [doubly-linked list](https://github.com/python/cpython/blob/5799e5a84c78eac672e5f5f4f3fd2d903ba51a9d/Modules/_collectionsmodule.c#L24)              |append(left): O(1), insert: O(n) | O(n)          | O(n)                   | O(n)|
