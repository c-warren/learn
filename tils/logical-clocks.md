---
title: Logical Clocks
topics:
    - time
    - distributed-systems
references: 
    - https://en.wikipedia.org/wiki/Lamport_timestamp
    - https://en.wikipedia.org/wiki/Vector_clock
---

# Logical Clocks

Logical clocks are a method to determine the order of events when two processes access a shared resource but do not have the ability to rely on a shared wall time. For example, if a system needs to store the ordering of events that have been processed in two different hosts, in two different clusters, in two different datacenters. 

The simplest logical clock is a monotonically increasing counter. This will work until you have two different processes acting on a shared resource with no serialization mechanism (e.g. an ACID database in a single region). 

## Types of Logical Clock

### Lamport Timestamps

Lamport Timestamps are a simple scalar implementation of a logical clock. Each process keeps a counter. Whenever a process handles an event (e.g. processing a request) it increments its counter, and the result is the event's timestamp: `counter := counter + 1`.

Whenever a process receives a message from another process, it sets its counter to the maximum of its own counter and the message's timestamp, plus one: `counter := max(local, received) + 1`.

This allows you to determine partial ordering of events within a distributed system. A lower timestamp means that an event may have happened first, or may have happened concurrently. 

### Vector Clocks

Vector clocks are an advanced version of a Lamport timestamp. Rather than a single counter, each process stores a vector with one counter (entry) for each independent process that is operating on the resource. Whenever a process handles an event it increments its own entry. Whenever a process receives a message it sets each entry in its vector to the maximum of its own value and the message's value, then increments its own entry. 

This allows a process to determine whether an event happened before, concurrently with, or after another event:
- A is before B if all entries in A are less than or equal to the matching entries in B and at least one is less than 
- A is after B if all entries in A are greater than or equal to the matching entries in B and at least one is greater
- A is equal to B if all entries in A are the same as B
- A is concurrent with B if none of the above are true

Vector clocks need to store significantly more information than a Lamport timestamp, and have unbounded storage growth with the number of unique processes. 

## When to use a Logical Clock 

Whenever a wall-clock timestamp is insufficient to determine the ordering of events you should use a logical clock. 

