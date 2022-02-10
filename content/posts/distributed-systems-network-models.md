+++
title = "Distributed Systems: System Models"
date = "2022-02-10T11:53:08+01:00"
author = "Solanke Abdulrahman"
authorTwitter = "Sarkk_" #do not include @
cover = ""
tags = []
keywords = []
description = ""
showFullContent = false
readingTime = true
summary = "System model in Distributed System"
+++


# 2.2 SYSTEM MODELS
- Two generals problem: A model of networks.  We assume that nodes are honest, but messages can get lost.
- Byzantine generals problem: A model of node behaviour. We assume that messages get delivered, but nodes can be dishonest / manipulated.

In real systems, both nodes and network might be faulty.

We want to capture assumptions in a system model consisting of:

- Network behaviour (e.g message loss)
- Node behaviour (e.g Crashes)
- Timing behaviour (e.g latency)

```python
class ServiceExcption(Exception):
    @staticmethod
    def hello(string):
        print(string)
```
## Network Behaviour
Assume bidi point-to-point communication between two nodes. We can have:
1. **Reliable** (perfect) links: A message is received if and only if it is sent.
2. **Fair-loss** links: Messages may be lost, duplicated or reordered. If you keep retrying, a message eventually gets through.
3. **Arbitrary** (Active adversary) links: A malicious adversary may interfere with messages (eavesdrop, modify, drop, spoof, replay).

**Network Partition** is when a link between nodes is interrupted for a finite amount of time.

Interesting thing with this model of network behaviour is that the links can be upgraded.

- Fair-loss link -> Reliable link: Using Retry + Deduplication logic.
- Arbitrary link -> Fair-loss link: Using TLS protocol. This can only be upgraded though, if the active adversary does not entirely stop the transmission of packets.


## Node Behaviour
Each node executes a specified algorithm, assuming one of the following:
1. **Crash-stop** (fail-stop): A node is faulty if it crashes (at any moment). After a crash, it stops executing forever.
2. **Crash-recovery** (fail-recovery): A node may crash at any moment, losing its in-memory state. It mar resume executing some other time.
3. **Byzantine** (fail-arbitrary): A node is faulty if it deviates from its algorithm. This could be crashing or  malicious behaviour.

A node that is not faulty is called **correct**. An assumption here is that a node does not know if another node is faulty, that can be handled by **Fault Detection** mechanisms.


## Synchrony (Timing) Behaviour
Assume one of the following for network and nodes:
1. **Synchronous**: Message latency no greater than a known upper bound. Nodes execute algorithm at a known speed.
2. **Partially Synchronous**: The system is asynchronous for some finite (but unknown) periods of time, synchronous otherwise.
3. **Asynchronous**:  Messages can be delayed arbitrarily. Nodes can pause execution arbitrarily. No timing guarantees at all.

Here are some violations of synchrony in practice:

**Violations of predictable latency**:

- Message loss requiring retry due to a network partition.
- Congestion/contention causing queueing.
- Network/route reconfiguration.

**Violations of predictable code execution speed**:

- OS scheduling issues e.g priority inversion.
- Stop-the-world GC pauses.
- Page faults, swap, thrashing.

**RTOS** provide scheduling guarantees, but most distributed systems do not use RTOS.


