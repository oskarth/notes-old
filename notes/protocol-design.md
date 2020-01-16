## Questions

1. What makes a great protocol and how do we design one?
2. How can we be confident that our protocol works the way we expect it to?
3. How do we ensure modularity, flexibility and interoperability?
4. How do we make protocols user-friendly and more likely to be adapted?

N.B. This one might be too broad, but restricting topic to just "capabilities" seems too narrow.

Most immediate question is the following:

- How should Waku capabilities be designed and communicated, especially in the context of something like libp2p?

## Notes

**Protocols (libp2p site, fetched 2020)** (https://docs.libp2p.io/concepts/protocols/):

- Protocols are identified by a protocol id in libp2p using a path like structure, e.g. `/ipfs/ping/1.0`
- Protocol negotiation happens over a bidirection stream, and protocol ids are matched exactly and associated with a specific handler. Optionally, fuzzy matching can be used as a fallback for semver matching, or anything else.
- A list of protocol ids can be provided, in order of preference of version supported.
- Some basic protocols are included, such as `ping`, `identity`, `kad-dht`, and `circuit-relay`.
