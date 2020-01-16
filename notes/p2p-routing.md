## Questions

1. How do we efficiently route content in a p2p network?
2. What are the trade-offs with respect to connectivity requirements, privacy, and so on?
3. How do various forms of relaying work?
4. What is the notion of "proof of trace" and where can we find it?

## Notes

**Kademlia: A peer-to-peer information system based on the xor metric (P Maymounkov, D Mazieres, 2002)** (https://pdos.csail.mit.edu/~petar/papers/maymounkov-kademlia-lncs.pdf):

- Distributed Hash Hable for P2P Networks that is efficient and provably consistent, using xor metric as distance metric.
- Each node, and key, has a 160-bit ID. These are stored in a Kademlia binary tree, where leaves are k-buckets of ids. Each node be connected to other subtrees to be able to reach the whole network.
- Nodes keep track of nodes and can cache data that is along its path. As a side effect of lookups, information about data location is also conveyed.
- Xor metric determines distance between two nodes, which allow us to get closer to our goal is lookup, O(logn).
- Each k-bucket acts as an LRU cache, and nodes uptime is lindy which is exploited by keeping old nodes around.
- The Kademlia protocol has 4 RPCs: ping, store, find_node, find_value

Further questions:
- What exactly is meant by provably consistent, given that it seems to be a probability based argument?
- Exactly what information is conveyed to whom in a lookup?
- How does Kademlia relate to Chord?

Illustration of Kademlia tree and connectivity (from paper):
![](../assets/kademlia1.png)

Illustration of Kademlia path lookup (from paper):
![](../assets/kademlia2.png)

*Landmark paper, need to study this further*
