# [Technical Reference Pages](../index.md)

## Improving the C-API

*Ways CPython's C-API can be improved, plus current projects to do so*

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

* [what is the c-api](capi-improvements.md#what-is-the-c-api)
* [the history of the c-api](capi-improvements.md#the-history-of-the-c-api)
* [the problems](capi-improvements.md#the-problems)
* [solutions and projects](capi-improvements.md#solutions-and-projects)
* [categories](capi-improvements.md#categories)
* [steve's proposal](capi-improvements.md#steves-proposal)
* [victor the superhero](capi-improvements.md#victor-the-superhero)
* [init and fini](capi-improvements.md#init-and-fini)

### What is the C-API?

...

### The History of the C-API

...

* the existing C-API has been fundamental to Python’s success
* growth in size and complexity over time (like all projects)
* early efforts to simplify (e.g. PEP 384 “Defining a Stable ABI”)
* recent efforts to categorize and hide
* growing consensus at recent language summits about problems
* focused effort by some core devs in the last 2 years

### The Problems

...

Example:
* getting rid of the GIL requires some low-level implementation changes
* parts of the public C-API expose some of the low-level details (e.g. refcounts)
* so...
* getting rid of the GIL requires breaking parts of the C-API

Note:  the same problem impacts efforts to improve CPython’s performance

Causes:
* internal details (e.g. struct members, low-level functions) not hidden
* “consenting adults” 
* internal or private API leaking out into the public API accidentally
* unintentional additions to the stable API

#### Thread Safety

...

See [The GIL](cpython-gil.md).

### Solutions and Projects

* someone has to care enough to do the work
* physically separate the categories of C-API
* more opaque structs
* [Python (C)FFI](https://mail.python.org/archives/list/capi-sig@python.org/thread/ZV3IQWW7XL6MHZAJARHW62WNZDCX3A4M/)
* break compatibility in a few places
* deprecate C-API in favor of [something like Cython (official)](https://mail.python.org/archives/list/capi-sig@python.org/thread/NSREULQDHSJ6SZTOY5YVSNX4U4AYJOI6/)
* maybe even completely replace the existing C-API
* ...

### Categories

* “internal”
    * very low-level CPython implementation details
    * “Do not touch!”
    * C-API:  `Include/internal/pycore_*.h`
* “private”
    * names with leading underscore and/or undocumented
    * “Use at your own risk!”
    * C-API:  `Include/*.h`
* “unstable”
    * the bulk of the public C-API; may change between major releases
    * “Go for it (but rebuild your extension each Python release)!”
    * C-API:  `Include/*.h` if `Py_LIMITED_API` not defined
* “stable”
    * will never change; binary compatible (i.e. memory layout)
    * “Worry-free!”
    * C-API:  `Include/*.h` if `Py_LIMITED_API` defined

Note:  `Include/cpython/*.h` helps facilitate the unstable API
Note:  `Include/python.h` gives you everything

...
Victor:
* https://mail.python.org/archives/list/capi-sig@python.org/thread/PJDSXPOHPPIZD5JAP4RWTRHOJFJ74VMO/
* https://mail.python.org/archives/list/capi-sig@python.org/thread/WS6ATJWRUQZESGGYP3CCSVPF7OMPMNM6/

### Steve's Proposal

https://mail.python.org/archives/list/capi-sig@python.org/thread/B2VDVLABM4RQ4ATEJXFZYWEGTBZPUBKW/

...

### Victor the Superhero

...

* https://mail.python.org/pipermail/python-committers/2016-March/003791.html
* https://vstinner.github.io/new-python-c-api.html
* https://pythoncapi.readthedocs.io/roadmap.html
* https://github.com/pythoncapi/pythoncapi
* https://mail.python.org/archives/list/capi-sig@python.org/thread/STTM62RG57IZ2EYDTJ7SVW32S45I4FRO/
* https://mail.python.org/archives/list/capi-sig@python.org/thread/ENRZNKIFVKK5JUELQRZKGW4WL6FYCBZ3/
* https://mail.python.org/pipermail/python-dev/2018-July/154814.html
* replace: https://mail.python.org/pipermail/python-dev/2018-November/155702.html
* re-org: https://mail.python.org/archives/list/capi-sig@python.org/thread/HS7WDUYFTT2MCAHQQM55UHPJLAVNYCWX/

### Init and Fini

...

Startup:
* [PEP 432 - Restructuring the CPython startup sequence](https://www.python.org/dev/peps/pep-0432/)
* [PEP 587 - Python Initialization Configuration](https://www.python.org/dev/peps/pep-0587/)

https://mail.python.org/pipermail/python-dev/2017-July/148656.html

---

See a typo, error, or ommission?  [Open an issue](https://github.com/ericsnowcurrently/reference-pages/issues)
or [make a pull request](https://github.com/ericsnowcurrently/reference-pages/pulls).

Copyright:  Eric Snow
