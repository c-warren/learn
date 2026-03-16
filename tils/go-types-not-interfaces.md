---
title: Produce types, accept interfaces
topics:
    - go
    - style
references: 
    - https://dev.to/shrsv/designing-go-apis-the-standard-library-way-accept-interfaces-return-structs-410k
    - Rob Pike
---

# Produce Structs, Accept Interfaces

It is a conventional idiom in golang that it is best to produce structs and accept interfaces. It is core to how go's standard library and core packages have been built. 

## Accepting Interfaces

Accepting an interface makes it easy to:
- mock the interface and use it to test your implementation, isolated from the underlying code
- satisfy the interface with multiple different implementations - e.g an in-memory db for unit tests, a lightweight db for integration test, etc. 
- Only consume the methods you need for your function, decreasing your reliance on an implementation

## Producing Types

Producing concrete types ensures that the receiver has a concrete implementation to work with. Consumers can:
- choose which methods are relevant to them and use them (via an interface)
