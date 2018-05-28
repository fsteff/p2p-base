# p2p-base

This is a collection of ideas for a p2p library aimed for "social" p2p networks while being leightweight enough for IoT and trying to be as fast as possible - feel free to start an issue to discuss something! \
Parts of the following are borrowed from [libp2p](https://libp2p.io) and this library could actually be implemented on the top of it.\
The main reason I started this is the idea I call a [topic-swarm](###Topic-Swarms).

## Core

Basically it can be described as a friend-to-friend network, while being flexible with the word friend. Basically a peer distrusts any other peer unless it has reason to [trust](##Trust) it. This can be seen similar to human-to-human interaction - if two peers trust another they may give each other information, "believe" in this information, provide services, ...\
 \
The core/base needs to be small and simple, but easy to extend. 
Therefore the core handles only connections between the peers, starting with simple discovery (mdns), transports (tcp) and multiplexing multiple streams over one connection. Everything else is an [extension](##Extensions).

### Peers

A peer has a public-private key pair to identify itself, while a peer-id is the hash of the public key.
A peer may use temporary ids, depending on who it communicates with, mainly for privacy reasons.

## Trust

Every interaction needs a certain amount of trust (a score/value).
Sources of trust can be:

- previous interaction 
- manually added trust (by the user or the application)
- [Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust)
- a baseline, for example depending of the discovery mechanism (eg. a peer on the local network start with a higher trust)
- a way of earning trust by doing costly computations ([proof of work](https://en.wikipedia.org/wiki/Proof-of-work_system), [proof of space](https://en.wikipedia.org/wiki/Proof-of-space))

## Extensions

### Futher transports

- UDP
- HTTP(s)
- Websockets
- ...

### Further peer discovery mechanisms

- Tracker servers (= always availabile peer with optimistic level of trust)
- DHT (a bit slow...)
- Peer Exchange ([PEX](https://en.wikipedia.org/wiki/Peer_exchange))
- ...

### Topic-Swarms

For each given "discussion-topic", internally represented by a hash of 
any arbitrary data/string the peers share a common interest.
Every peer may broadcast messages to the swarm. Each message contains a timestamp of its creation and is signed by the peer. \
The peers accumulate these messages and forward them to new peers. How long a peer stores this data may depend on various factors, as trust of the sender, free memory, size, ...\
To avoid spam and DDoS, some sort of defense mechanism is necessary, some peers may even only trust known peers (that have a certain level of trust). \
This can be used in various ways, here some ideas:

- discovery of content (e.g. hashtags in a social network)
- discovery of peers
- bridging downtime of peers (e.g. for a chat app)

### Port forwarding

Needs very high trust, dangerous(!)