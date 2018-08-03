## Terminology
Great explanation of words "average" and "amortized" in russian can be found in Yandex Data School online materials: [part1](https://yandexdataschool.ru/edu-process/courses/algorithms#item-1) and [part2](https://yandexdataschool.ru/edu-process/courses/algorithms#item-2).

Note, that I define an average time complexity for the function *f(arg, seed)* as a function

    n -> max_{arg \in Args_n} mean_{seed in Seeds} Time(f(arg, seed)), 
where

*Args_n* is all possible arguments of length *n*,

*Seeds* is the set of all possible seeds,

*Time(f(arg, seed))* is a computation time for *f* with the arguments *arg* and the seed *seed*.

# List

Based on python source code 
[github/cpython/Include/listobject.h](https://github.com/python/cpython/blob/master/Include/listobject.h)
and
[github/cpython/Objects/listobject.c](https://github.com/python/cpython/blob/master/Objects/listobject.c).

### Realization

List is realized as a vector of pointers to list elements ([definition of PyListObject](https://github.com/python/cpython/blob/master/Include/listobject.h#L23)).

### Time complexity

**n** is a lenght of the given list __x__.

**k** is a lenght of an argument for the given operation (extend)

**i** is some integer number (index).

| Operation   | Usual case     | Worst case | Comment |
| ----------  | :----------:   | :--------: | :-:  |
| x.append    | O(1) amortized | O(n)       | x    |
| x.extend    | O(k) amortized | O(n)       | x    |
| x.insert    | -              | O(n)       | x    |
| x.remove    | -              | O(n)       | x    |
| x.pop       | O(1) amortized | O(n)       | x    |
| x.clear     | -              | O(n)       | x    |
| x.index     | -              | O(n)       | x    |
| x.count     | -              | O(n)       | x    |
| x.sort      | -              | O(nlogn)   | adaptive, stable, natural mergesort    |
| x.reverse   | -              | O(n)       | source code: list_reverse_impl -> reverse_slice    |
| x.copy      | -              | O(n)       | x    |
| x[i]        | -              | O(1)       | x    |


# Set and frozenset

Based on python source code
[github/cpython/Include/setobject.h](https://github.com/python/cpython/blob/master/Include/setobject.h)
and
[github/cpython/Objects/setobject.c](https://github.com/python/cpython/blob/master/Objects/setobject.c).

### Realization

Set and frozenset are realized as a hashtable ([definition of PySetObject](https://github.com/python/cpython/blob/master/Include/setobject.h#L42)). The PySetObject data structure is shared by set and frozenset objects.

### Time complexity

**n** is a lenght of the given set/frozenset **x**.

**k** is len(**other**).

**ob** is some hashable python object.

| Operation                            | Usual case             | Worst case | Comment |
| -------------------                  | :----------:           | :--------: | :-:  |
| x.isdisjoint(other)                  | O(k) average           | O(nk)      ||
| x.issubset(other)                    | O(n) average           | O(nk)      ||
| x.copy()                             | -                      | O(n)       ||
| x.union(other)                       | O(n + k) average       | O(nk)      ||
| x.symmetric_difference(other)        | O(n + k) average       | O(nk)      | first copy x, then symmetric_difference_update|
| x.symmetric_difference_update(other) | O(n + k) average       | O(nk)      | only for set    |
| x.intersection(other)                | O(min(n, k)) average   | O(nk)      ||
| x.intersection_update(other)         | O(min(n, k)) average   | O(nk)      | only for set, uses set_intersection    |
| x.difference(other)                  | O(min(n, k)) average   | O(nk)      |[not exactly min](https://github.com/python/cpython/blob/master/Objects/setobject.c#L1578)|
| x.difference_update(other)           | O(n + k) average       | O(nk)      | only for set    |
| x.add(ob)                            | O(1) amortized, average| O(n)       | only for set, for ob with length 1|
| x.remove(ob)                         | O(1) average           | O(n)       | only for set, for ob with length 1    |
| x.discard(ob)                        | O(1) average           | O(n)       | only for set, for ob with length 1    |
| x.pop()                              | O(1) average           | O(n)       | only for set    |
| x.clear()                            | -                      | O(n)       | only for set    |
| ob in x                              | O(1) average           | O(n)       | only for set, for ob with length 1    |
| x.clear()                            | -                      | O(n)       | only for set, [set_clear_internal](https://github.com/python/cpython/blob/master/Objects/setobject.c#L473)    |


