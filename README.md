# unbuiltins

[![PyPI version](https://badge.fury.io/py/unbuiltins.svg)](https://badge.fury.io/py/unbuiltins)

Thoroughly tested and MyPy compatible constants and functions you'd expect to 
find in the Python [builtins](https://docs.python.org/3/library/builtins.html).

## Install

*requires python 3.8 or higher*

```bash
pip install unbuiltins
```

## Usage 

```python
from unbuiltins import *
```

### Missing

Like the constant `None`, `Missing` can be used for e.g. optional function 
arguments:

```python
def say(value: Missable[str] = Missing):
    if value is Missing:
        print('*silence*')
    else:
        print(value)
```

### frozendict

The builtins include `frozenset`, but `frozendict` is nowhere to be found 
(see [PEP 416](https://www.python.org/dev/peps/pep-0416/)).

`frozendict` is basically an immutable and hashable `dict`:

```
assert frozendict(spam='spam') == dict(spam='spam')
assert {frozendict(): 'empty'}[frozendict()] == 'empty'
```

It accepts generic type arguments and supports the merge (`|`) operator, 
like `dict` since Python 3.9:

```python
def add_eggs(value: frozendict[str, str]) -> frozendict[str, Union[str, int]]: 
    return value | dict(eggs=6)

assert 'eggs' in add_eggs(frozendict(spam='spam')) 
```

### first

Behaves identical to `next`, but works for all iterables:

```python
assert first([42, 666, 69]) == 42
assert first([], default='empty') == 'empty'
assert first(iter(dict(eggs=6))) == 'eggs'
```

### filternone

Return those items of `iterable` that are not `None`:

```python
assert list(filternone(['', None, 0, False])) == ['', 0, False]
```


### either

Returns the first argument that is not `None` or `Missing`:

```python
assert either(None, None, False) is False
```
