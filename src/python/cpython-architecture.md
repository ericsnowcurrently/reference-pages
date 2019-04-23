# [Technical Reference Pages](../index.md)

## An Overview of CPython's Architecture

*The layers and components of CPython*

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

* [the layers](cpython-architecture.md#the-layers)
* [other key components](cpython-architecture.md#other-key-components)
* [GC and refcounting](cpython-architecture.md#gc-and-refcounting)
* [the eval loop](cpython-architecture.md#the-eval-loop)
* [the runtime's lifecycle](cpython-architecture.md#the-runtimes-lifecycle)
* [the codebase](cpython-architecture.md#the-codebase)

### The Layers

|     layer     |                  description                   |         C-API        |
| ------------- | ---------------------------------------------- | -------------------- |
| process       | the OS process                                 |                      |
| runtime       | everything Python-related in a process         | `PyRuntimeState`     |
| interpreter   | all Python threads and everything they share   | `PyInterpreterState` |
| Python thread | wrapper around OS thread with eval loop inside | `PyThreadState`      |
| call stack    | stack of eval loop instances                   | `PyFrameObject`      |
| eval loop     | execs the series of instructions in a code obj | `PyEval_EvalFrame()` |

Notes:
* "interpreter" is often conflated with the eval loop
* an eval loop instance is effectively a Python function call

#### Relationship Between Layers

```
process        1-1  runtime
runtime        1-n  interpreters
runtime        1-n  objects
interpreter    1-n  Python threads
Python thread  1-1  OS thread
Python thread  1-1  call stack
call stack     1-n  eval loops
```

Caveats:
* technically, each OS thread may have many Python threads associated with it
* we may move to supporting more than one runtime per process
* we will likely make objects per-interpreter instead of global

#### State At Different Layers

|  process ->  |     runtime ->      |  interpreter ->  | thread / stack / ceval |
| ------------ | ------------------- | ---------------- | ---------------------- |
| env vars     | GIL                 | sys module       | current frame          |
| sockets      | signal handlers     | modules          | stack depth            |
| file handles | `Py_AtExit()` funcs | atexit handlers  | “tracing”              |
| signals      | GC                  | fork handlers    | hook: trace            |
| TLS / TSS    | allocator (mem)     | hook: eval_frame | hook: profile |
| ...          | objects             | codecs           | current exception      |
|              | pending calls       |                  | context                |
|              | “eval breaker”      |                  | ...                    |
|              | ...                 |                  |                        |

### Other Key Components

* objects (and types)
    * what the eval loop (and C-API) operates on
    * every object has a type and every type is an object
    * C-API:  `PyObject` (and `PyTypeObject`)
* core builtin types and singletons (e.g. `dict`, `str`, `tuple`, `None`)
* builtin modules (e.g. `sys`, `builtins`) and stdlib
* C-API

### GC and Refcounting

* “garbage collection”:  how unused memory (objects) gets cleaned up
* “reference count”:  each Python object has a count of places used
* refcounts controlled (in C) via `Py_INCREF` and `Py_DECREF` macros
* (C) code that wants to keep an object alive, increments its refcount
* that code decrements the refcount when done with the object
* when refcount reaches 0, object is deleted (`__del__`) and memory is freed

### The Eval Loop

```
<set up>
for instruction in code object:
    <maybe side-channel stuff>
    <occasionally release & re-acquire the GIL>
    <execute next instruction>
```

#### Thread Safety

...

See [The GIL](cpython-gil.md).

### The Runtime's Lifecycle

TBD

### The Codebase

TBD

---

See a typo, error, or ommission?  [Open an issue](https://github.com/ericsnowcurrently/reference-pages/issues)
or [make a pull request](https://github.com/ericsnowcurrently/reference-pages/pulls).

Copyright:  Eric Snow
