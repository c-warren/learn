---
title: Key lookup performance in go's context
topics:
    - go
references: 
    - groxx
    - https://cs.opensource.google/go/go/+/refs/tags/go1.25.0:src/context/context.go
---

# Key lookup performance in go's context

There are multiple ways to operate on a context, each of which creates a new context with a reference to its parent (the original context):
    - WithCancel, which adds cancellation to the context
    - WithValue, which adds a key:value pair to the context
    - and many more

This means value lookups within a context behave in the same way as a linked list (e.g O(n)). 
With very large sets of key:value pairs stored on the context:
    - Key misses will visit every Context
    - Some key hits will visit almost every context

For high performance workloads storing information in the context should be carefully considered. 
