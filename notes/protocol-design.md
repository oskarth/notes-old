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

**The Transport Layer Security (TLS) Protocol Version 1.3 (RFC, 2018)** (https://tools.ietf.org/html/rfc8446):

- Overview: provides secure channel between two parties with confidentiality, authenticaiton, and integrity properties. Only assumes reliable, ordered stream. Consists of handshake protocol (hello and negotiation) and record protocol (ongoing traffic). 
- Protocol negotiation: it sets field `legacy_version` in `ClientHello` to a byte string corresponding to TLS 1.2. This means old clients sees it as a TLS 1.2 client. This is a c struct.
- Inside `ClientHello` there is also an extension field, with supported versions in it. TLS 1.3 clients can detect each other this way. Once 1.3 session is up it can not be re-negotiated.  
- 0-RTT: Inside `ClientHello` key material is also shared. This means traffic can start to be encrypted as soon as negotiation is done.

*A bunch of these are more about security and cryptography*:

Follow up questions:
- Regarding data representation, how do C structs compare to e.g. protobufs and what are their tradeoffs?
- How does support for many additional crypto construct works? E.g. I want to use ECC not RSA.
- Security: What are the downgrade attacks and 0-RTT replay concerns?
- Exactly when is the secure channel setup, at what point are the properties guaranteed?
- What about forward secrecy, and why / why is it not a design consideration?
  - At record layer it is
- What about traffic key/protocol? What does it do?
  - It provides forward secrecy and also "length concealment", where you can't tell content from padding
