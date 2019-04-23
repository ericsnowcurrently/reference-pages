# [Technical Reference Pages](../index.md)

## Threading in CPython

*How threading works, particularly in CPython*

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

* [what is threading?](cpython-threading.md#what-is-threading)
* [hardware and OS threads](cpython-threading.md#hardware-and-os-threads)
* [hazards and mitigations](cpython-threading.md#hazards-and-mitigations)
* [Python threads](cpython-threading.md#python-threads)
* [the GIL](cpython-threading.md#the-gil)
* [use cases and alternatives](cpython-threading.md#use-cases-and-alternatives)

### What is Threading?

...

### Hardware and OS Threads

...

### Hazards and Mitigations

#### Race Conditions

...
* https://docs.google.com/presentation/d/1_qbtSCAS9KhxVH77np106D0gq1wjHUxrVFHgZuxBupc#slide=id.g5683b3a755_0_213
* https://docs.google.com/presentation/d/1_qbtSCAS9KhxVH77np106D0gq1wjHUxrVFHgZuxBupc#slide=id.g5683b3a755_0_352

#### Deadlock

TBD

#### Headaches

TBD

### Python Threads

...

What Happens When a Python Thread is Started?
1. create a `PyThreadState` under current interpreter
1. create an OS thread
1. associate the OS thread ID with the `PyThreadState`
1. start up the eval loop
1. finalize the Python thread when the eval loop stops
1. exit the OS thread

### The GIL

See [The GIL](cpython-gil.md).

* “Global Interpreter Lock”
* only one eval loop at a time
* guards internal state and objects
* work around it via the C-API

### Use Cases and Alternatives

* async / await
* multi-processing

---

See a typo, error, or ommission?  [Open an issue](https://github.com/ericsnowcurrently/reference-pages/issues)
or [make a pull request](https://github.com/ericsnowcurrently/reference-pages/pulls).

Copyright:  Eric Snow
