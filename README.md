[![Travis](https://img.shields.io/travis/Erotemic/ubelt.svg)](https://travis-ci.org/Erotemic/ubelt)
[![Pypi](https://img.shields.io/pypi/v/ubelt.svg)](https://pypi.python.org/pypi/ubelt)
[![Codecov](https://codecov.io/github/Erotemic/ubelt/badge.svg?branch=master&service=github)](https://codecov.io/github/Erotemic/ubelt?branch=master)


## What is UBelt?
UBelt is a "utility belt" of commonly needed utility and helper functions.
It is a migration of the most useful parts of `utool`
  (https://github.com/Erotemic/utool) into a minimal and standalone module.

The `utool` library contains a number of useful utility functions, however a
number of these are too specific or not well documented. The goal of this
migration is to slowly port over the most re-usable parts of `utool` into a
stable package.

In addition to utility functions `utool` also contains a custom doctest
  harness and code introspection and auto-generation features.
This port will contain a rewrite of the doctest harness.
Some of the code introspection features will be ported, but most
  auto-generation abilities will be ported into a new module that depends on
  `ubelt`.

## Available Functions:
This list of functions and classes is currently available. 
See the corresponding doc-strings for more details.

```
import ubelt as ub

ub.dict_hist
ub.dict_subset
ub.dict_take
ub.find_duplicates
ub.group_items
ub.map_keys
ub.map_vals
ub.readfrom
ub.writeto
ub.ensuredir
ub.ensure_app_resource_dir
ub.compress
ub.take
ub.flatten
ub.memoize
ub.NiceRepr
ub.NoParam
ub.CaptureStdout
ub.Timer
ub.Timerit
ub.ProgIter
ub.Cacher
```

A minimal version of the doctest harness has been completed.
This can be accessed using `ub.doctest_package`.

## Examples

Here are some examples of some cool features


### Caching
```python
>>> import ubelt as ub
>>> cfgstr = 'repr-of-params-that-uniquely-determine-the-process'
>>> cacher = ub.Cacher('test_process', cfgstr)
>>> data = cacher.tryload()
>>> if data is None:
>>>     myvar1 = 'result of expensive process'
>>>     myvar2 = 'another result'
>>>     data = myvar1, myvar2
>>>     cacher.save(data)
>>> myvar1, myvar2 = data
```


### Timing
```python
>>> import ubelt as ub
>>> timer = ub.Timer('Timer demo!', verbose=1)
>>> with timer:
>>>     prime = ub.find_nth_prime(40)
tic('Timer demo!')
...toc('Timer demo!')=0.0008s
```


### Robust Timing
Easily do robust timings on existing blocks of code by simply indenting them.
There is no need to refactor into a string representation or convert to a
single line.  With `ub.Timerit` there is no need to resort to the `timeit`
module!

The quick and dirty way just requires one indent.
```python
>>> import ubelt as ub
>>> for _ in ub.Timerit(num=200, verbose=2):
>>>     ub.find_nth_prime(100)
```

For more accurate timings or to incorporate an untimed setup phase, use the
loop variable as a context manager.  You can also access properties of the
`ub.Timerit` class to programmatically use results.
```python
>>> import ubelt as ub
>>> t1 = ub.Timerit(num=200, verbose=2)
>>> for timer in t1:
>>>     setup_vars = 100
>>>     with timer:
>>>         ub.find_nth_prime(setup_vars)
>>> print('t1.total_time = %r' % (t1.total_time,))
```


### Grouping

```python
>>> import ubelt as ub
>>> item_list    = ['ham',     'jam',   'spam',     'eggs',    'cheese', 'bannana']
>>> groupid_list = ['protein', 'fruit', 'protein',  'protein', 'dairy',  'fruit']
>>> result = ub.group_items(item_list, groupid_list)
>>> print(result)
{'dairy': ['cheese'], 'fruit': ['jam', 'bannana'], 'protein': ['ham', 'spam', 'eggs']}
```


### Dictionary Histogram

Find the frequency of items in a sequence
```python
>>> import ubelt as ub
>>> item_list = [1, 2, 39, 900, 1232, 900, 1232, 2, 2, 2, 900]
>>> hist = ub.dict_hist(item_list)
>>> print(hist)
{1232: 2, 1: 1, 2: 4, 900: 3, 39: 1}
```


### Dictionary Manipulation


Take a subset of a dictionary
```
>>> import ubelt as ub
>>> dict_ = {'K': 3, 'dcvs_clip_max': 0.2, 'p': 0.1}
>>> keys = ['K', 'dcvs_clip_max']
>>> subdict_ = ub.dict_subset(dict_, keys)
>>> print(subdict_)
{'K': 3, 'dcvs_clip_max': 0.2}
```


Take only the values
```python
>>> import ubelt as ub
>>> dict_ = {1: 'a', 2: 'b', 3: 'c'}
>>> keys = [1, 2, 3, 4, 5]
>>> print(list(ub.dict_take(dict_, keys, default=None)))
['a', 'b', 'c', None, None]
```


Change the values in a dict based on a function
```python
>>> import ubelt as ub
>>> dict_ = {'a': [1, 2, 3], 'b': []}
>>> newdict = ub.map_vals(len, dict_)
>>> print(newdict)
{'a': 3, 'b': 0}
```


### Keep track of loop progress
```python
>>> from ubelt.progiter import *  # NOQA
>>> def is_prime(n):
...     return n >= 2 and not any(n % i == 0 for i in range(2, n))
>>> for n in ProgIter(range(100), verbose=2):
>>>     # do some work
>>>     is_prime(n)
```
10000/10000... rate=13294.94 Hz, eta=0:00:00, total=0:00:00, wall=13:34 EST



## Installation:
```
pip install git+https://github.com/Erotemic/ubelt.git
```