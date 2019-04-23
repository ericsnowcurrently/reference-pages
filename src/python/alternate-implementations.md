# [Technical Reference Pages](../index.md)

## Python Implementations

*A review of the various implementations of Python*

Author: [Eric Snow](../authors/ericsnowcurrently.md)


```
Caveat:  This reference is intended to be accurate.  I probably have some small
         details wrong but count on all the important things being correct.
         Feel free to leave a comment if you notice an error.
```
```
Caveat:  This reference is intended to be sufficiently complete, though not
         exhaustive (it isn't practical to try to cover every detail).  Where
         possible I'll point to other references that provide further detail.
         If you think I missed something then please leave a comment.
```


### Table of Contents

* [what is Python?](alternate-implementations.md#what-is-python)
* [the implementations](alternate-implementations.md#the-implementations)
* [cpython](alternate-implementations.md#cpython)
* [pypy](alternate-implementations.md#pypy)
* [micropython](alternate-implementations.md#micropython)
* [jython](alternate-implementations.md#jython)
* [ironpython](alternate-implementations.md#ironpython)
* [others](alternate-implementations.md#others)

### What is Python?

TBD

### The Implementations

|                     impl                   | born | active? | Python  |   lang    |    key    |
| ------------------------------------------ | ---- | ------- | ------- | --------- | --------- |
| [CPython](https://www.python.org/)         | 1989 |   yes   |   3.7   |    C      | reference |
| [PyPy](https://pypy.org/)                  |      |   yes   |   3.6   |  RPython  |    JIT    |
| [MicroPython](http://www.micropython.org/) |      |   yes   |  ~3.4+  |    C      | embedded  |
| [Jython](https://www.jython.org/)          |      |  ~yes   |   2.7   |   Java    |    Java   |
| [IronPython](https://ironpython.net/)      |      |  ~yes   |   2.7   |    C#     |    .NET   |
|                 ----                       |      |         |         |           |           |
| Unladen Swallow                            |      |   no    |         |    C      |   Clang   |
| ...                                        |      |   no    |         |           |           |

* web-oriented (e.g. [pyodide, batavia](https://github.com/ericsnowcurrently/pyweb/wiki/6-Prior-Art))?

See also: [PyPY w/ STM](http://doc.pypy.org/en/latest/stm.html).

Compatibility:

|    impl     |    C-API?     |
| ----------- | ------------- |
| CPython     |     yes       |
| PyPy        | CFFI, cpyext  |
| ...w/ STM   |               |
| MicroPython |     no?       |
| Jython      |     JyNI      |
| IronPython  |     yes?      |

Implementation Details:

|    impl     |  GIL? | refcount? |
| ----------- |  ---- | --------- |
| CPython     |   yes |    yes    |
| PyPy        |   yes |    no     |
| ...w/ STM   |   no  |    no     |
| MicroPython |  ~yes |    no     |
| Jython      |   no  |    no     |
| IronPython  |   no  |    no     |

### CPython

TBD

### PyPy

TBD

### MicroPython

TBD

### Jython

TBD

### IronPython

TBD

### Others

TBD

---

See a typo, error, or ommission?  [Open an issue](https://github.com/ericsnowcurrently/reference-pages/issues)
or [make a pull request](https://github.com/ericsnowcurrently/reference-pages/pulls).

Copyright:  Eric Snow
