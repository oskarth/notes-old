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

**Thoughts on supporting accretion by the use of open maps**:

Spurred by discussion on optional fields and compatibility in https://github.com/vacp2p/specs/issues/92#issuecomment-576518815

To get compatibility, graceful upgrades - and generally keep sane - we want to support accretion. We don't want to change things, we want to make new things. Why?

Lets say we have two interpreters of data, A and B. We don't control them. Now we found a problem with our data, so we want to change it! What do we do? We want both A and B to be able to understand it.

Let's say our data is a list of items: `[version favorite_color current_address]`. Now let's say we stop caring about favorite color and instead care about pet name. The hasty way is to just change it, so it reads `[version pet_name current_address]`. We ship it and Bob updates. But Alice sees this data and doesn't understand it! They now get garbage, a favorite color called Garfield, huh? A more graceful way is to *accrete* and append this to the list: `[version favorite_color current_address pet_name]`. Now, Alice can make sense of the data, and they can ignore the pet name.

This, of course, assumes we are using dynamic, growable arrays. Alternatively, that we encode its length as the very first item. So even if doesn't know what a "Garfield" is (or it might just be a jumble of bytes), it can ignore it.

Now, after a while we might get tired of just appending stuff to a list. And Bob has moved on. He doesn't care about colors anymore, and he is tired of filling it in with `nil` all the time. Oops! A better design might have been to have an open map, where fields are optional. This is how JSON and Protobuf works. Now, if our data format doesn't support maps we can simulate them in a list by using association lists. This way our data looks like this `[version [ "favorite_color" favorite_color ] [ "address" address ] [ "pet_name" pet_name ]]`. Here we can specify whatever list of key-values we care about, as long as Alice and Bob are chill and know how to deal with data missing. They should be!

Let's say we made a mistake. We have to remove something, and not something that is optional and whatever, but something that something Alice really cares about! What do we do? We again, accrete, we make a new thing! This is why want a version number. So we can just bump it and say "here's this new thing I don't know if you know or care about it but here it is": `[version pet_name]`.

Further reading:
- Rich Hickey on https://www.youtube.com/watch?v=oyLBGkS5ICk and accretion
- edn design decisions and optionals
- General extensible data formats, length fields, protocol versions and so on
- Protobuf move to optional anywhere
- JSON schemaless and downsides for interpreters
- How is this done in live systems like Bitcoin and Bittorrent?
- IPv6 and HTTP and why they don't get deployed (per Moxie point re slow moving upgrades)
