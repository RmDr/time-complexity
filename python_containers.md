## Python version.
All the text below is about python3. However, the differences between python2 and python3 time complexity are minor.

## Terminology
Great explanation of words "average" and "amortized" (rus. "учётная", "амортизированная") in russian can be found in Yandex Data School online materials: [part1](https://yandexdataschool.ru/edu-process/courses/algorithms#item-1) and [part2](https://yandexdataschool.ru/edu-process/courses/algorithms#item-2).

Note, that I define an average time complexity for the function *f(arg, seed)* as a function

    n -> max_{arg \in Args_n} mean_{seed in Seeds} Time(f(arg, seed)), 
where

*Args_n* is a set of all possible arguments of length *n*,

*Seeds* is a set of all possible seeds,

*Time(f(arg, seed))* is a computation time for *f* with arguments *arg* and a seed *seed*.

# List

Based on python source code 
[github/cpython/Include/listobject.h](https://github.com/python/cpython/blob/master/Include/listobject.h)
and
[github/cpython/Objects/listobject.c](https://github.com/python/cpython/blob/master/Objects/listobject.c).

### Implementation

List is implemented as a vector of pointers to list elements ([definition of PyListObject](https://github.com/python/cpython/blob/master/Include/listobject.h#L23)).

### Time complexity

**n** is a lenght of the given list __x__.

**i** is some integer number (index).

**ob** is some python object.

**obs** is an iterable container with python objects, **k** = len(obs).

| Operation   | Usual case     | Worst case | Comment |
| ----------  | :----------:   | :--------: | :-:  |
| x.append(ob)     | O(1) amortized | O(n)       |     |
| x.extend(obs)    | O(k) amortized | O(n)       |     |
| x.insert(ob)     | -              | O(n)       |     |
| x.remove(ob)     | -              | O(n)       |     |
| x.pop()          | O(1) amortized | O(n)       |     |
| x.clear()        | -              | O(n)       |     |
| x.index(ob)      | -              | O(n)       |     |
| x.count(ob)      | -              | O(n)       |     |
| x.sort()         | -              | O(nlogn)   | adaptive, stable, natural mergesort    |
| x.reverse()      | -              | O(n)       | [list_reverse_impl](https://github.com/python/cpython/blob/master/Objects/listobject.c#L2422) -> [reverse_slice](https://github.com/python/cpython/blob/master/Objects/listobject.c#L1001)    |
| x.copy()         | -              | O(n)       |     |
| x[i]             | -              | O(1)       |     |


# Set and frozenset

Based on python source code
[github/cpython/Include/setobject.h](https://github.com/python/cpython/blob/master/Include/setobject.h)
and
[github/cpython/Objects/setobject.c](https://github.com/python/cpython/blob/master/Objects/setobject.c).

### Implementation

Set and frozenset are implemented as a hashtable ([definition of PySetObject](https://github.com/python/cpython/blob/master/Include/setobject.h#L42)). The PySetObject data structure is shared by set and frozenset objects.

#### About lookup 
Almost every operation with set uses lookup of the key in the hashtable. Lookup consists of several hash-probes. After each probe we inspect consecutive nearby entries ([LINEAR_PROBES](https://github.com/python/cpython/blob/master/Objects/setobject.c#L49) = 9).

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

# Dict

Based on python source code
[github/cpython/Include/dictobject.h](https://github.com/python/cpython/blob/master/Include/dictobject.h)
and
[github/cpython/Objects/dictobject.c](https://github.com/python/cpython/blob/master/Objects/dictobject.c).

### Implementation

Dict is implemented as a hashtable ([definition of PyDictObject](https://github.com/python/cpython/blob/e4dcbbd7f4ac18d01c0ec85f64ae98b8281ed403/Include/dictobject.h#L23)). The implementation is about the same as for set and frozenset.

### Time complexity

**n** is a lenght of the given dict **x**.

**k** is len(**other**).

**key** is some hashable python object.

**value** is some python object.

| Operation                            | Usual case             | Worst case | Comment |
| -------------------                  | :----------:           | :--------: | :-:  |
| x.copy()                             | -                      | O(n)       ||
| x[key] = value                       | O(1) amortized, average| O(n)       ||
| x.pop(key)                           | O(1) average           | O(n)       ||
| x.clear()                            | -                      | O(n)       ||
| key in x                             | O(1) average           | O(n)       ||
| x.poopitem()                         | O(1) average           | O(n)       |uses [lookdict_index](https://github.com/python/cpython/blob/e4dcbbd7f4ac18d01c0ec85f64ae98b8281ed403/Objects/dictobject.c#L2944), changes dict|
| x.clear()                            | -                      | O(n)       |[PyDict_Clear](https://github.com/python/cpython/blob/e4dcbbd7f4ac18d01c0ec85f64ae98b8281ed403/Objects/dictobject.c#L1612)    |
