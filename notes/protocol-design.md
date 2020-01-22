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

**Spec-ulation Keynote, Clojure Conj (Rich Hickey, 2016)** (https://www.youtube.com/watch?v=oyLBGkS5ICk):

- Talk on what Clojure Spec is "about", but lessons very general. About what we require and what we provide, how to think about change, and reasoning at the right level of granularity.
- Decomposing 'change': Change isn't a thing, instead we do one of (1) Grow (require less, provide more) (2) Break (require more, provide less, change random stuff) or (3) Fix (fix bug, make faster, fewer dependencies, etc).
- Don't break things: It is broken, terrible for consumers, and there is no need. Instead, grow and accrete. If you hate something, make a new name for it, e.g. `new-ns/foo` or `old/ns/foo2`.
- Applies at each layer: functions (args; ret/effect); namespace (collection of names); artifacts. E.g. a namespace is just a collection or set of names.
- Semver is broken, doesn't tell you much: Semver "docare.dontcare.dontcare"; lack of granularity about what is different. Doesn't allow two things to co-exist.
- Version requirements: Chronological ordering; rules out strictly SHA-based, which is great for specific version of code and allows code based tooling to do fine-granular testing and only include part of runtime you need (advanced). On local dev vs open dev - alphas still ok.
- Compatibility as a requirement for success; long-lasting. Not pulling the rug out from under people.
- Maven as a good example, just immutable collection of stuff, no one going to remove version X from there. Bringing FP to the library level.

Follow up questions:
- If we have a protocol `foo` and "bump" version to two, how is this different from calling it a new name "foo2"? It is a new name. It appears to me that the main litmus test is one of "can two version coexist or not?". There's also something about intent here, but there's more here. E.g. protocol negotiation / services and which versions they support.

- What does this look like for classical APIs like Unix, Java, HTML, etc?
- How do we want to think about this in terms of protocol negotiation?

Further reading:
- C - Interface and Implementation book - anything there?
- Look at Unix, HTTP/HTML, Java API and versions
- E.g. http://man7.org/linux/man-pages/man2/syscalls.2.html
- Collective Code Construction Contract (C4) https://rfc.zeromq.org/spec:22/C4/
- The End of Software Versions http://hintjens.com/blog:85
