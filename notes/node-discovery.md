## Questions

1. What are different methods of node discovery? What are their pros and cons?
2. What security threats should be worried about? When it comes to eclipse attacks, censorship resistance etc
3. Which methods are most suitable resource restricted devices?

## Notes

**EIP 1459: Node Discovery via DNS (Lange, Szilágyi)** (https://eips.ethereum.org/EIPS/eip-1459):

- Proposes using DNS as a discovery mechanism for bootstrap nodes in Ethereum. This allows for more (dynamic) bootstrap nodes vs embedding them in short client list.
- If joining DHT is difficult due to restricted restricted network, DNS alone can work, since DNS is available anywhere.
- Uses a merkle tree of node records, so a single signature of the root is sufficient to authenticate the whole node list.

Follow up question:
- When it comes to normal nodes, what are the advantages of using Discovery v5?
