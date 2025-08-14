---
title: creating a generic proto.Message in go
topics:
    - go
    - proto
    - generics
references: 
    - https://github.com/jbranchaud/til/blob/master/go/produce-the-zero-value-of-a-generic-type.md
    - https://sourcegraph.com/github.com/hashicorp/consul@9b9e3f929db68add7374bd8254f5959e153e117b/-/blob/internal/resource/decode.go?L47-54 
---

# creating a generic proto.Message in go

When creating the zero value of a generic type in go, one can use:

```go
func zero[T any]() T {
	return *new(T)
}
```

This can then be used as a valid nil return value, or to be assigned with data later (see [jbraunchaud](https://github.com/jbranchaud/til/blob/master/go/produce-the-zero-value-of-a-generic-type.md)).

If your generic is a `proto.Message` however this technique will not create a useable value. `Unmarshal` requires a mutable value, while newing a `proto.Message` creates a pointer to an interface. 

Instead to get the new zero value of a proto message use:

```go
func zero[T proto.Message]() T {
	var zero T
	data := zero.ProtoReflect().New().Interface().(T)
	return &data
}
```

Data can then be assigned with:

```go
var zero T
data := zero.ProtoReflect().New().Interface().(T)
if err := res.Data.UnmarshalTo(data); err != nil {
	return nil, err
}
return data, nil
```

See also [Hashicorp's implementation](https://sourcegraph.com/github.com/hashicorp/consul@9b9e3f929db68add7374bd8254f5959e153e117b/-/blob/internal/resource/decode.go?L47-54)
