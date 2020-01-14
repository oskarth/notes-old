## Questions

1. What are some classes of data representation?
2. How are they used in various domains?
3. What are their tradeoffs?
4. What are some special considerations when it comes to protocol over the wire and p2p systems?
5. How do we ensure designs are compatible, future proof, upgradable and composable?

## Reading

- [x] https://multiformats.io/multiaddr/
- [ ] Rich Hickey and Alan Kay on data (2016) https://news.ycombinator.com/item?id=11945869
- [ ] edn spec https://github.com/edn-format/edn
- [ ] TLV https://en.wikipedia.org/wiki/Type-length-value
- [ ] Internet Message format (TCP/IP based, HTTP etc) (2001) https://tools.ietf.org/html/rfc2822
- [ ] IP DoD standard (1980) https://tools.ietf.org/html/rfc760
- [ ] RLP wiki https://github.com/Ethereum/wiki/wiki/RLP
- [ ] RLP rationale site https://ethereum.stackexchange.com/questions/19092/why-was-rlp-chosen-as-the-low-level-protocol-encoding-algorithm
- [ ] Misc discussions on RLP and SSZ etc in Eth2 (i) https://github.com/ethereum/eth2.0-specs/issues/129 (ii) https://github.com/ethresearch/p2p/issues/15 (iii) https://github.com/ethereum/eth2.0-specs/issues/2 

- ipv6 fail upgrade case study
- protobuf rationale
- Worse is better? JSON, etc

## Notes

**Multiformat (site, fetched 2020)** (https://multiformats.io/):

- Reality: format have many different trade-offs, choices, let's accommodate that. Self-describing and future proof.
- Collection of formats with a shared set of requirements: must be in-band/self-describing; extensible/no lock in; compact; human-readable.
- Examples: multiaddr, multihash, multicodec, etc.

Follow up questions:
- Whats the maturity status of each of these and how are they used?

Further reading:
- IPFS specs
- IPLD specs
- Dat protocol

*A bit off-topic for node discovery, consider moving this.*

**Multiaddr (draft, fetched 2020)** (https://multiformats.io/multiaddr/):

- Current addresses not self-describing, i.e. is `127.0.0.1:9000` TCP? UDP? Something else? Instead, use self-describing network addresses.
- Uses recursive TLV (type-length-value) and has a human-readable representation and binary representation. Example: `/dns4/foo.com/tcp/80/http/bar/cat.jpg`
- Default list of codecs to be used for "type" in `multicodec` table

Further reading:
- multicodec and https://github.com/multiformats/multiaddr/blob/master/protocols.csv
- https://en.wikipedia.org/wiki/Type-length-value

Follow up questions:
- different ways of representing data, pros and cons? its own notes file, e.g. predefined (tcp), protobuf, ascii (http), clojure edn, etc.
