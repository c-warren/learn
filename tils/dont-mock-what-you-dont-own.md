---
title: isolate third party service testing
topics:
    - testing
    - software-architecture
references: 
    - https://8thlight.com/insights/thats-not-yours
---

# Isolate Third Party Service Testing

Don't mock what you don't own is a succinct summary of some of the reasons relying on mocking throughout your codebase can lead to:
- flaky, unreliable tests
- really poor readability of tests
- unneccesary overhead

I've seen all of these problems and more from not carefully considering where your unit test, integration test, and end to end test boundaries are. 
It proposes a simple heuristic:
> don't mock what you don't own

to identify where the unit<>integration test layer sits. 

Furthermore, its often possible to get bogged down in discussions about how you should test specific components if you don't have an overarching recommendation that keeps tests fast while ensuring they guarantee correctness. 

## Structuring Code to avoid over mocking

The proposed structure is similar to architectural patterns like MVC:
- isolate code that depends on an external service - e.g a database, a network API, etc. - in a wrapper class (sometimes called a repository, gateway, service)
- run integration tests for all code in the wrapper class
- mock the wrapper class inside your service layer

The caller should be completely isolated from the dependency:
- errors are not relied on
- data is mapped to a domain entity before making it to the caller 

This enables the caller to mock the wrapper class and write fast unit tests.
The wrapper class pushes the slower integration tests as far down the stack as possible - and only to the methods that are absolutely required for the application. 
All tests in the wrapper class then set-up their objects using the dependency - e.g a database test would look like:
```go
func TestUpdate(t *testing.T) error {
    defaultValue := "foo"
    // must create data using the actual db
    id, err := t.Db.Create("foo")
    value := t.Db.Get(id)
    Expect(value).To(Equal(defaultValue))
    updated, err := t.Db.Update(id, newValue)
    Expect(updated).To(Equal(newValue))
}
```

### Caveats

Sometimes its extremely hard to use an integration test to test whether or not a wrapper class performs some error handling logic correctly. 
For example, in a spanner wrapper library it could be hard to force a concurrent transaction rejection error without significant load. 
In these cases, if the behaviour is known/documented it can be useful to augment the wrapper class with unit tests to handle this logic - particularly if it is crucial to the application. 

In some cases it is prohibitively expensive to run tests against a live instance (e.g spanner). 
In these cases a trade-off between correctness and cost has to be made. 
