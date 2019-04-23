# [Technical Reference Pages](../index.md)

## Subinterpreters and Multi-core Parallelism

*All about subinterpreters, especially how they can help with multi-core Python*

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

* [what are subinterpreters?](multicore-subinterpreters.md#what-are-subinterpreters)
* [the c-api](multicore-subinterpreters.md#the-c-api)
* [pep 554](multicore-subinterpreters.md#pep-554)
* [a new concurrency model](multicore-subinterpreters.md#a-new-concurrency-model)
* [limitations and deficiencies](multicore-subinterpreters.md#limitations-and-deficiencies)
* [improved isolation](multicore-subinterpreters.md#improved-isolation)
* [stop sharing the GIL!](multicore-subinterpreters.md#stop-sharing-the-gil)
* [future improvements](multicore-subinterpreters.md#future-improvements)

### What Are Subinterpreters?

...

* the initial interpreter in a process is called the “main” interpreter
    * it has certain responsibilities
* a “subinterpreter” is any other interpreter created within the runtime
* each subinterpreter is effectively isolated from each other
* CPython has supported subinterpreters for over 20 years in the C-API

### The C-API

...

### PEP 554

https://www.python.org/dev/peps/pep-0554/

PEP 554 proposes exposing them in a new stdlib module in 3.8 (or 3.9).

* new “interpreters” module:
    * create(), list_all(), etc.
    * Interpreter class
    * create_channel()
    * RecvChannel, SendChannel
* start minimally

initial restrictions:
* objects are not actually shared
* “shareable” objects limited to simple builtin immutable types
* only strings may be run (a la exec)

...

### A New Concurrency Model

...

* the isolation of processes with the efficiency of threads

### Limitations and Deficiencies

...

### Improved Isolation

...

### Stop Sharing the GIL!

...

* allow each interpreter to operate independently
* threads within an interpreter would still share a “GIL”
* shouldn’t require wide-spread changes
* no change to single-threaded (or single-interpreter) performance

Why hasn’t it been done already?
* no one has given much thought to subinterpreters until recently
* no one was interested in doing the work
* there were “good enough” alternatives
* scary???
* blockers...

Blockers:
* lingering bugs with subinterpreters and the runtime
* subinterpreters not exposed in the stdlib
* how to guard against races between interpreters?
* enough time to do the work!
* C globals  (╯°□°)╯︵ ┻━┻
    * “static globals”
    * (in the CPython code base)
        * static types, exceptions, singletons; free lists; caches
    * in extension modules
        * static types, exceptions, singletons; etc.
        * C globals in included shared libraries (e.g. [OpenSSL in cryptography](https://github.com/pyca/cryptography/issues/2299))
        * efforts to fix:
            * [PEP 3121](https://www.python.org/dev/peps/pep-3121/)
            * [PEP 384](https://www.python.org/dev/peps/pep-0384/)
            * [PEP 489](https://www.python.org/dev/peps/pep-0489/)
            * ([PEP 573](https://www.python.org/dev/peps/pep-0573/))
            * [PEP 575](https://www.python.org/dev/peps/pep-0575/)
            * ([PEP 579](https://www.python.org/dev/peps/pep-0579/))
            * ([PEP 580](https://www.python.org/dev/peps/pep-0580/))
            * Cython
            * Red Hat
            * Instagram
        * (type “slots”)

My Project:
* https://github.com/ericsnowcurrently/multi-core-python
* PEP 554
* resolve blockers
* move some runtime state into the interpreter state
    * including the GIL
* beneficial side effects
    * find bugs and deficiencies in runtime (e.g. init/fini)
    * motivation to fix them
    * clean-up in runtime implementation (incl. globals, C-API, header files)
    * reduce coupling between components in runtime implementation
    * encourage fewer static globals in C extension modules
    * (improve interpreter startup performance)
    * (improve object isolation (e.g. in memory))
    * ...

### Future Improvements

...

* address low-hanging fruit (e.g. tune implementation)
* make interpreter startup more efficient
* better memory sharing between interpreters
* share actual objects safely
* run more than just strings (e.g. functions)
* ...

---

See a typo, error, or ommission?  [Open an issue](https://github.com/ericsnowcurrently/reference-pages/issues)
or [make a pull request](https://github.com/ericsnowcurrently/reference-pages/pulls).

Copyright:  Eric Snow
