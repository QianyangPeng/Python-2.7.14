# python-2.7.14-r
Randomized the iterating order of python 2 `dict` class.

The following functions are modified:
1. static PyObject * dictiter_iternextkey(dictiterobject *di)

2. static PyObject * dictiter_iternextvalue(dictiterobject *di)

3. static PyObject * dictiter_iternextitem(dictiterobject *di)

# setup
You can refer to this page for setup instructions.

https://devguide.python.org/setup/

It is recommended that to install cpython_r in another directory other than your default python3. You can use the `--prefix` option to set your installation directory during configuration.

```
./configure --prefix=/foo/bar
```

# example code
```python
d = {"a":1, "b":2, "c":3, "d":4, "e":5, "f":7, "g":6}
a = [{}, {}, {}, {}, {}, {}, {}]
for k in range(70000):
    for i,e in enumerate(d):
        if e in a[i]:
            a[i][e] += 1
        else:
            a[i][e] = 1 
for i in a:
    print(i)
''' 
python-2.7.14-r result:
{'d': 10173, 'a': 10035, 'b': 9867, 'f': 10082, 'e': 9885, 'c': 10011, 'g': 9947}
{'c': 9925, 'g': 9941, 'a': 10093, 'e': 10120, 'd': 9917, 'f': 10031, 'b': 9973}
{'g': 10076, 'c': 10003, 'e': 9897, 'a': 10102, 'f': 10021, 'd': 9947, 'b': 9954}
{'a': 9860, 'b': 10017, 'e': 9931, 'd': 10117, 'f': 9958, 'g': 10154, 'c': 9963}
{'e': 10123, 'f': 10004, 'd': 9842, 'a': 9837, 'c': 10039, 'g': 9966, 'b': 10189}
{'b': 10065, 'd': 10049, 'c': 10043, 'g': 9836, 'f': 9903, 'a': 10059, 'e': 10045}
{'f': 10001, 'e': 9999, 'b': 9935, 'a': 10014, 'g': 10080, 'd': 9955, 'c': 10016}
python-2.7.14 result:
{'a': 70000}
{'b': 70000}
{'c': 70000}
{'d': 70000}
{'e': 70000}
{'f': 70000}
{'g': 70000}
'''
```

```python
d = {"a":1, "b":2, "c":3, "d":4, "e":5, "f":7, "g":6}
a = [{}, {}, {}, {}, {}, {}, {}]
for k in range(70000):
    for i,e in enumerate(d.viewkeys()):
        if e in a[i]:
            a[i][e] += 1
        else:
            a[i][e] = 1 
for i in a:
    print(i)
''' 
python-2.7.14-r result:
{'a': 10040, 'c': 10082, 'b': 10010, 'e': 9937, 'd': 9971, 'g': 10047, 'f': 9913}
{'a': 10092, 'c': 10053, 'b': 10057, 'e': 10012, 'd': 9943, 'g': 9992, 'f': 9851}
{'a': 10121, 'c': 10023, 'b': 10170, 'e': 9830, 'd': 9962, 'g': 9934, 'f': 9960}
{'a': 9699, 'c': 10013, 'b': 9960, 'e': 10148, 'd': 10050, 'g': 10065, 'f': 10065}
{'a': 9974, 'c': 9938, 'b': 9796, 'e': 10022, 'd': 10026, 'g': 10071, 'f': 10173}
{'a': 10096, 'c': 10005, 'b': 9991, 'e': 10067, 'd': 9982, 'g': 9822, 'f': 10037}
{'a': 9978, 'c': 9886, 'b': 10016, 'e': 9984, 'd': 10066, 'g': 10069, 'f': 10001}
python-2.7.14 result:
{'a': 70000}
{'c': 70000}
{'b': 70000}
{'e': 70000}
{'d': 70000}
{'g': 70000}
{'f': 70000}
'''
```

```python
d = {"a":1, "b":2, "c":3, "d":4, "e":5, "f":7, "g":6}
a = [{}, {}, {}, {}, {}, {}, {}]
for k in range(70000):
    for i,e in enumerate(d.iterkeys()):
        if e in a[i]:
            a[i][e] += 1
        else:
            a[i][e] = 1 
for i in a:
    print(i)
''' 
python-2.7.14-r result:
{'a': 10040, 'c': 10082, 'b': 10010, 'e': 9937, 'd': 9971, 'g': 10047, 'f': 9913}
{'a': 10092, 'c': 10053, 'b': 10057, 'e': 10012, 'd': 9943, 'g': 9992, 'f': 9851}
{'a': 10121, 'c': 10023, 'b': 10170, 'e': 9830, 'd': 9962, 'g': 9934, 'f': 9960}
{'a': 9699, 'c': 10013, 'b': 9960, 'e': 10148, 'd': 10050, 'g': 10065, 'f': 10065}
{'a': 9974, 'c': 9938, 'b': 9796, 'e': 10022, 'd': 10026, 'g': 10071, 'f': 10173}
{'a': 10096, 'c': 10005, 'b': 9991, 'e': 10067, 'd': 9982, 'g': 9822, 'f': 10037}
{'a': 9978, 'c': 9886, 'b': 10016, 'e': 9984, 'd': 10066, 'g': 10069, 'f': 10001}
python-2.7.14 result:
{'a': 70000}
{'c': 70000}
{'b': 70000}
{'e': 70000}
{'d': 70000}
{'g': 70000}
{'f': 70000}
'''
```

# notice
In python 2 dictionary, only viewkeys(), viewvalues(), viewitems(), iterkeys(), itervalues(), iteritems() are using iterators, while keys(), values() and items() are not using iterators. So the iterating order of the latter functions will not be randomized.