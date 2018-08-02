
# List

### Realization

Based on python source code [github/cpython/Objects/listobject.c](https://github.com/python/cpython/blob/master/Objects/listobject.c)
and
[github/cpython/Objects/Include/listobject.h](https://github.com/python/cpython/blob/master/Include/listobject.h)

List is realized as a vector of pointers to list elements.

### Time complexity

**n** is a lenght of the given list.

**k** is a lenght of an argument for the given operation (extend)

| Operation  | Average case | Worst case | Amortized worst case | Comment |
| ---------- | :----------: | :--------: | :------------------: | :-:  |
| .append    | -            | O(n)       | O(1)                 | x    |
| .extend    | -            | O(n)       | O(k)                 | x    |
| .insert    | -            | O(n)       | -                    | x    |
| .remove    | -            | O(n)       | -                    | x    |
| .pop       | -            | O(1)       | O(1)                 | x    |
| .clear     | -            | O(n)       |                      | x    |
| .index     | -            | O(n)       | -                    | x    |
| .count     | -            | O(n)       | -                    | x    |
| .sort      | -            | O(nlogn)   | -                    | adaptive, stable, natural mergesort    |
| .reverse   | -            | O(n)       | -                    | source code: list_reverse_impl -> reverse_slice    |
| .copy      | -            | O(n)       | -                    | x    |
| [x]        | -            | O(1)       | -                    | x    |


