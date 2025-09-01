# DESIGN CONSISTENT HASHING

To achieve horizontal scaling, it is important to distribute requests/data efficiently and evenly
across servers. Consistent hashing is a commonly used technique to achieve this goal. But
first, let us take an in-depth look at the problem.

## Rehashing problem

n cache servers
common way to balance load is to use hash method:
`serverIndex=hash(key)%N` {N is size of server pool}

there is a key, key has a hash, hash%4

## key | hash | hash % 4

key0 | 18358617 | 1
key1 | 26143584 | 0

To fetch the server where a key is stored, we perform the modular operation `f(key) % 4`
`hash(key0) % 4 = 1` means a client must contact `server 1` to fetch the cached data.

### serverIndex = hash % 4

| Server Index | 0          | 1          | 2          | 3          |
| ------------ | ---------- | ---------- | ---------- | ---------- |
| **Servers**  | server 0   | server 1   | server 2   | server 3   |
| **Keys**     | key1, key3 | key0, key4 | key2, key6 | key5, key7 |

works when size of server pool is fixed, data distribution is fixed.

but problems arrives on scaling.

ex:
if server 1 goes offline, the size of the server pool becomes 3. Using
the same hash function, we get the same hash value for a key. But applying modular
operation gives us different server indexes because the number of servers is reduced by 1.

| key  | Hash     | hash % 3 |
| ---- | -------- | -------- |
| key0 | 18358617 | 0        |
| key1 | 26143584 | 0        |
| key2 | 18131146 | 1        |
| key3 | 35863496 | 2        |
| key4 | 34085809 | 1        |
| key5 | 27581703 | 0        |
| key6 | 38164978 | 1        |
| key7 | 22530351 | 0        |

### serverIndex = hash % 3

### Server Index Distribution (after `hash % 3`)

| Server Index | 0                                  | 1                    | 2        |
| ------------ | ---------------------------------- | -------------------- | -------- | -------- |
| **Servers**  | server 0                           | server 1             | server 2 | server 3 |
| **Keys**     | **key0**, key1, **key5**, **key7** | key2, **key4**, key6 | **key3** |

most keys are redistributed, not just the ones originally stored in the
offline server (server 1).
means if server1 goes down, client connect to wrong server to fetch data -> cache misses

solution: **Consistent
hashing is an effective technique to mitigate this problem.**

## Consistent Caching

Consistent hashing is a special kind of hashing such that when a
hash table is re-sized and consistent hashing is used, only k/n keys need to be remapped on
average, where k is the number of keys, and n is the number of slots. In contrast, in most
traditional hash tables, a change in the number of array slots causes nearly all keys to be
remapped

## Hash Space & Ring

there is a hash function f, output range is: x0,x1,x2,x3....xn
SHA-1 : 0 to 2^160-1
x0=0 .... xn=2^160-1

by collection both ends we get a ring

## Hash Server

Using the same hash function f, we map servers based on server IP or name onto the ring.

```mermaid
graph LR
    A[server 0] --- B[server 1]
    B --- C[server 2]
    C --- D[server 3]
    D --- A
```

## Hash Keys

cache keys are hashed into hash ring

```mermaid
graph LR
    %% Define servers
    S0((s0))
    S1((s1))
    S2((s2))
    S3((s3))

    %% Define keys
    K0((k0))
    K1((k1))
    K2((k2))
    K3((k3))

    %% Connect ring (servers + keys)
    S0 --- K0
    K0 --- S1
    S1 --- K1
    K1 --- S2
    S2 --- K2
    K2 --- S3
    S3 --- K3
    K3 --- S0

    %% Legends
    %% Servers: s0=server0, s1=server1, s2=server2, s3=server3
    %% Keys: k0=key0, k1=key1, k2=key2, k3=key3
```

## Server Lookup

To determine which server a key is stored on, we go clockwise from the key position on the
ring until a server is found

```mermaid
graph LR
    %% Servers
    S0((s0)):::server0
    S1((s1)):::server1
    S2((s2)):::server2
    S3((s3)):::server3

    %% Keys
    K0((k0))
    K1((k1))
    K2((k2))
    K3((k3))

    %% Ring connections (directed)
    S0 --> K1
    K1 --> S1
    S1 --> K2
    K2 --> S2
    S2 --> K3
    K3 --> S3
    S3 --> K0
    K0 --> S0

    %% Style for servers
    classDef server0 fill:#b19cd9,stroke:#333,stroke-width:2px;
    classDef server1 fill:#00bfff,stroke:#333,stroke-width:2px;
    classDef server2 fill:#ff69b4,stroke:#333,stroke-width:2px;
    classDef server3 fill:#ffa500,stroke:#333,stroke-width:2px;

    %% Legend (servers)
    subgraph Legend_Servers[Servers]
      LS0[server 0]:::server0
      LS1[server 1]:::server1
      LS2[server 2]:::server2
      LS3[server 3]:::server3
    end

    %% Legend (keys)
    subgraph Legend_Keys[Keys]
      LK0[k0 = key0]
      LK1[k1 = key1]
      LK2[k2 = key2]
      LK3[k3 = key3]
    end
```

## Add a Server

adding a new server will only require redistribution of a
fraction of keys.

after a new server 4 is added, only key0 needs to be redistributed
before it was on s0, but after onto s4

```graph LR
    %% Servers
    S0((s0)):::server0
    S1((s1)):::server1
    S2((s2)):::server2
    S3((s3)):::server3
    S4((s4)):::server4

    %% Keys
    K0((k0))
    K1((k1))
    K2((k2))
    K3((k3))

    %% Ring connections
    S0 --- K1
    K1 --- S1
    S1 --- K2
    K2 --- S2
    S2 --- K3
    K3 --- S3
    S3 --- K0
    K0 --- S4
    S4 --- S0

    %% Extra arrows (movement)
    K0 --> S4
    S4 -.-> X((❌)) -.-> S0

    %% Style for servers
    classDef server0 fill:#b19cd9,stroke:#333,stroke-width:2px;
    classDef server1 fill:#00bfff,stroke:#333,stroke-width:2px;
    classDef server2 fill:#ff69b4,stroke:#333,stroke-width:2px;
    classDef server3 fill:#ffa500,stroke:#333,stroke-width:2px;
    classDef server4 fill:#228b22,stroke:#333,stroke-width:2px;

    %% Legend (servers)
    subgraph Legend_Servers[Servers]
      LS0[server 0]:::server0
      LS1[server 1]:::server1
      LS2[server 2]:::server2
      LS3[server 3]:::server3
      LS4[server 4]:::server4
    end

    %% Legend (keys)
    subgraph Legend_Keys[Keys]
      LK0[k0 = key0]
      LK1[k1 = key1]
      LK2[k2 = key2]
      LK3[k3 = key3]
    end
```

## Remove a Server

When a server is removed, only a small fraction of keys require redistribution with consistent
hashing.

```mermaid
graph TB
    subgraph Ring
        k0((k0)) --> s0
        s0 --> k1((k1))
        k1 --> s1
        s1 --> k2((k2))
        k2 --> s2
        s2 --> k3((k3))
        k3 --> s3
        s3 --> k0
    end

    s1 --X--> s2
```

when server 1 is removed, only key1 must be remapped to server 2.
The rest of the keys are unaffected.

## 2 issues

basic steps are:

- Map servers and keys on to the ring using a uniformly distributed hash function.
- To find out which server a key is mapped to, go clockwise from the key position until the
  first server on the ring is found.

2 problems are:

1. Uneven partition sizes – When servers are added or removed, the hash space between adjacent servers (the partition) becomes imbalanced. Some servers may end up with very small partitions, while others get very large ones.

```mermaid
graph TB
    subgraph Ring
        s3 --> s0
        s0 --> s1
        s1 --X--> s2
        s2 --> s3
    end

    s1 -.-> s0
    s1 -.-> s2

```

2. Non-uniform key distribution – Depending on how servers are positioned on the ring, keys may cluster on certain servers. This can leave some servers overloaded while others store little or no data.

```mermaid
graph TB
    subgraph Ring
        s0 --> s1
        s1 --X--> s2
        s2 --> s3
        s3 --> s0
    end

    s1 --> s2
    s1 --> s0

```

## Virtual Nodes

A virtual node refers to the real node, and each server is represented by multiple virtual nodes
on the ring

```mermaid
graph TB
    subgraph Ring
        s0_0 --> s1_0
        s1_0 --> s0_1
        s0_1 --> s1_1
        s1_1 --> s0_2
        s0_2 --> s1_2
        s1_2 --> s0_0
    end

    s0_0 -->|s0| s0_1
    s0_1 -->|s0| s0_2
    s0_2 -->|s0| s0_0

    s1_0 -->|s1| s1_1
    s1_1 -->|s1| s1_2
    s1_2 -->|s1| s1_0

```

To find which server a key is stored on, we go clockwise from the key’s location and find the
first virtual node encountered on the ring.

```marmaid
graph TB
    subgraph Ring
        s0_2 -- s1_0
        s1_0 -- s0_0
        s0_0 -- k0
        k0 -- s1_1
        k0 --> s1_1
        s1_1 -- s0_1
        s0_1 -- s1_2
        s1_2 -- s0_2
```

as virtual nodes incrwease distribution of keyts becomes more balanced
cause standard deviation decreases

for 100-200 virtual nodes deviation is between 5% (200 virtual nodes) and 10%
(100 virtual nodes) of the mean.

## Find Affected Keys

When a server is added or removed, a fraction of data needs to be redistributed

```mermaid
graph TB
    subgraph Ring
        s3 -- k0
        k0 -- s4
        k0 -- s4
        s4 -- s0
        s0 -- k1
        k1 -- s1
        s1 -- k2
        k2 -- s2
        s2 -- k3
        k3 -- s3
```

when server 4 is added onto the ring. The affected range starts from s4 (newly
added node) and moves anticlockwise around the ring until a server is found (s3). Thus, keys
located between s3 and s4 need to be redistributed to s4.

```mermaid
graph TB
    graph TD
    subgraph "Diagram 1: Initial State with Failure"
        k0_1[k0/key0]
        k1_1[k1/key1]
        k2_1[k2/key2]
        k3_1[k3/key3]
        s0_1[s0/server0]
        s1_1[s1/server1]
        s2_1[s2/server2]
        s3_1[s3/server3]
        s4_1[s4/server4]

        k0_1 --> s4_1
        s4_1 --> s0_1
        s0_1 -.->|FAILED| s1_1
        s1_1 --> k1_1
        k1_1 --> s1_1
        s1_1 --> s2_1
        s2_1 --> k2_1
        k2_1 --> s2_1
        s2_1 --> s3_1
        s3_1 --> k3_1
        k3_1 --> s3_1
        s3_1 --> k0_1
    end

    subgraph "Diagram 2: After Recovery"
        k0_2[k0/key0]
        k1_2[k1/key1]
        k2_2[k2/key2]
        k3_2[k3/key3]
        s0_2[s0/server0]
        s1_2[s1/server1 - FAILED]
        s2_2[s2/server2]
        s3_2[s3/server3]

        k0_2 --> s3_2
        s3_2 --> s0_2
        s0_2 --> k3_2
        k3_2 --> s2_2
        s2_2 --> k2_2
        k2_2 --> s2_2
        s2_2 --> k1_2
        k1_2 --> s0_2
    end

    classDef server0 fill:#d8b4fe
    classDef server1 fill:#06b6d4
    classDef server2 fill:#ec4899
    classDef server3 fill:#fb923c
    classDef server4 fill:#22c55e
    classDef failed fill:#ef4444,stroke:#dc2626,stroke-width:3px
    classDef key fill:#f3f4f6,stroke:#374151

    class s0_1,s0_2 server0
    class s1_1 server1
    class s1_2 failed
    class s2_1,s2_2 server2
    class s3_1,s3_2 server3
    class s4_1 server4
    class k0_1,k0_2,k1_1,k1_2,k2_1,k2_2,k3_1,k3_2 key
```

When a server (s1) is removed as shown in Figure 5-15, the affected range starts from s1
(removed node) and moves anticlockwise around the ring until a server is found (s0). Thus,
keys located between s0 and s1 must be redistributed to s2.

# Referance material

[Consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing)

[Consistent Hashing](https://tom-e-white.com/2007/11/consistent-hashing.html)

[Dynamo: Amazon's Highly Available Key-value Store](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

[Cassandra - A Decentralized Structured Storage System](http://www.cs.cornell.edu/Projects/ladis2009/papers/Lakshman-ladis2009.PDF)

[How Discord Scaled Elixir to 5,000,000 Concurrent Users](https://blog.discord.com/scaling-elixir-f9b8e1e7c29b)

[CS168: The Modern Algorithmic Toolbox Lecture #1: Introduction and Consistent Hashing](http://theory.stanford.edu/~tim/s16/l/l1.pdf)

[Maglev: A Fast and Reliable Software Network Load Balancer](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/44824.pdf)
