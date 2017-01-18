##functools

###cmp_to_key(func)
    
    transform comparison function to key function, used for sorted(),min(),max()...
```python
implementation
def cmp_to_key(mycmp):
    """Convert a cmp= function into a key= function"""
    class K(object):
        __slots__ = ['obj']
        def __init__(self, obj):
            self.obj = obj
        def __lt__(self, other):
            return mycmp(self.obj, other.obj) < 0
        def __gt__(self, other):
            return mycmp(self.obj, other.obj) > 0
        def __eq__(self, other):
            return mycmp(self.obj, other.obj) == 0
        def __le__(self, other):
            return mycmp(self.obj, other.obj) <= 0
        def __ge__(self, other):
            return mycmp(self.obj, other.obj) >= 0
        __hash__ = None
    return K
```

###lru_cache(maxsize=128, typed=False)
```python
@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

>>> [fib(n) for n in range(16)]
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610]

>>> fib.cache_info()
CacheInfo(hits=28, misses=16, maxsize=None, currsize=16)
```
| attribute | info |
| --- | --- |
| **cache_info()** | get the cache info |
| **\_\_wrapped\_\_** | get the wrapped function |
| **cache_clear()** | clear cache |

```python
#lru algorithms in this function

PREV, NEXT, KEY, RESULT = 0, 1, 2, 3
root = []
root[:] = [root, root, None, None]
cache = {}

"""add element"""
"not exist"
last = root[PREV]
link = [last, root, key, result]
last[NEXT] = root[PREV] = cache[key] = link
full = (len(cache) >= maxsize)
"exist"
link_prev, link_next, _key, result = link
link_prev[NEXT] = link_next
link_next[PREV] = link_prev
last = root[PREV]
last[NEXT] = root[PREV] = link
link[PREV] = last
link[NEXT] = root
hits += 1

"""full"""
oldroot = root
oldroot[KEY] = key
oldroot[RESULT] = result
root = oldroot[NEXT]
oldkey = root[KEY]
oldresult = root[RESULT]
root[KEY] = root[RESULT] = None
del cache[oldkey]
cache[key] = oldroot

"""clear cache"""
cache.clear()
root[:] = [root, root, None, None]
hits = misses = 0
full = False
```
