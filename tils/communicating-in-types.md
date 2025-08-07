---
title: communicating in types
topics:
    - types
    - software-design
references: 
    - https://gotopia.tech/sessions/3487/communicating-in-types
    - kris-jenkins
---

# Communicating in Types

This talk is fun, engaging, a general sale of the power of statically typed languages as well as some powerful type features "newer" languages are adopting from functional languages. 

## As Documentation

Types (can be) an extremely expressive, automatically updated form of documentation. 
If the types are well created (e.g not just 'string', 'int', etc.) they are an exact description of what you expect your domain to be, and how you expect it to behave. 
They are a unique form of documentation that talks both in machine language (when compiled) and in human language (to other humans) - something eng docs cannot do, and compiled code cannot do. 

### As Descriptors

Type can describe things:
```go
type Rectangle struct { ... }
```
or relationships:
```go 
type AreaFn func (...float64) float64 {
```
or context - in the example of the dependencies that a function requires to do its job. This is better expressed in the languages he references in the talk rather than go - but is an interesting concept.

## Compiler as an Assistant

Rather than thinking about the compiler as a barrier - or types themselves as a barrier - to speed of delivery, it can be helpful to think of it as an assistant. 
If you give the compiler the right information to work with, it will tirelessly check your work across the entire codebase every time you compile. 
It will also do this when future you or other people develop in the codebase - ensuring what you define as the valid "world" in your domain remains true. 

## Type Tips

### AND Types and OR Types

This was a new (to me) description of structs and enums. 
Structs can be considered AND types - a Rectangle is a Height AND a Width. 
```go
type Rectangle struct {
    Height float64
    Width float64
}
```

Enums can be considered OR types - a Shape is a Rectangle OR a Square. 
```rust
enum Shape {
  Rectangle,
  Square,
}
```

A powerful concept within some languages is the combination of the two - an OR type with multiple AND types inside.
```rust
enum Shape {
  Rectangle(f32, f32),
  Square(f32),
}
```

Examples in the talk:
- an event stream that takes many different event types with different payloads
- the response on an api which returns either a successful result or one of many different error types, each with its own payload
