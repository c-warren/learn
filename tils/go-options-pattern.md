---
title: go options pattern
topics:
    - go
    - constructors
references: 
    - c-warren
---

# Go Options Pattern

Often when writing a package or library in Go you will want to export some struct types without exposing its implementation. 
A way to do this is to make the struct private to the package and export an initialization function (or constructor) for that type:

```golang
type counter struct {
    count int
    increment int
}

func (c *counter) Increment() {
    c.count += c.increment
}

func NewCounter() counter {
    return counter{}
}
```

When you want to customize the behaviour of that instance you have multiple options:
    - provide arguments to the New function
    - provide a struct of arguments to the New function
    - use the options pattern

The options pattern is a useful when:
    - you want to ensure backwards compatibility for the constructor signature
    - you have a large number of parameters, most of which don't need to be specified regularly 
    - have a lot of documentation to write for options and don't want a heavily cluttered parameterized struct or constructor documentation

## The opts pattern

The options pattern works by declaring a function type that operates on the concrete instance, modifying its internal properties. 
WithX functions are then written that return an Options function that can be provided to the constructor.
These WithX functions are then exposed to the consumer of the package to pass to the constructor in use.

```golang
...

type CounterOpt func(c *counter)

func NewCounter(opts ...CounterOpts) counter {
    c := &counter{}
    for _, o := range opts {
        o(c)
    }

    return c
}

func WithIncrement(inc int) CounterOpt {
    return func(c *counter) {
        c.increment = inc
    }
}
```

This can then be used from another package:
```
counter := NewCounter(
    WithIncrement(10),
);
```
