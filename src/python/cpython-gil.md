# [Technical Reference Pages](../index.md)

## The GIL

*All about CPython's Global Interpreter Lock*

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

* [what is the GIL?](cpython-gil.md#what-is-the-gil)
* [why do we need a GIL?](cpython-gil.md#why-do-we-need-a-gil)
* [costs and benefits](cpython-gil.md#costs-and-benefits)
* [effect and perception](cpython-gil.md#effect-and-perception)
* [working around the GIL](cpython-gil.md#working-around-the-gil)
* [past attempts to remove the GIL](cpython-gil.md#past-attempts-to-remove-the-gil)
* [a GIL-free future?](cpython-gil.md#a-gil-free-future)

### What is the GIL?

* “Global Interpreter Lock”
* Low-level mutex shared by all threads in the process
* Ensures only 1 Python thread is executing *Python* code at a time
    * Acquired at beginning of eval loop
    * Released and re-acquired every few times through the eval loop
    * Optionally released / re-acquired (manually) in C code
* Guards against internal races
    * internal shared resources
    * objects

https://docs.python.org/3/c-api/init.html#thread-state-and-the-global-interpreter-lock

### Why Do We Need a GIL?

Prevent race conditions:
* on internal CPython state
* on objects

### Costs and Benefits

Costs:
* multi-core parallelism of *Python* code
* ???

Benefits:
* cheaper than fine-grained locks
* lower contention for global resources
* simpler eval loop implementation
* simpler object implementation
* simpler C-API implementation

### Effect and Perception

Who does it really affect?
* users with threaded, CPU-bound *Python* code (relatively few people)
* basically no one else

Why?  C implementation releases the GIL around IO and CPU-intensive code.

So why does the GIL get such a bad wrap?
* lack of understanding
* experience with other programming languages
* haters gonna hate

### Working Around the GIL

C-extension modules:
* rewrite CPU-bound code in C
* release the GIL around that code

### Past Attempts to Remove the GIL

* ???
* ???
* Gilectomy
* other Python implementations
    * unladen swallow
    * ...

### A GIL-free Future?

...

See [Improving the C-API](capi-improvements.md) and [Subinterpreters and Multi-core Parallelism](multicore-subinterpreters.md).

---

See a typo, error, or ommission?  [Open an issue](https://github.com/ericsnowcurrently/reference-pages/issues)
or [make a pull request](https://github.com/ericsnowcurrently/reference-pages/pulls).

Copyright:  Eric Snow
