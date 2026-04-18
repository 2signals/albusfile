# albusfile

an experimental research project on zero-knowledge, local-first p2p transfer.

- **transport:** quic (multiplexed, 0-rtt) with multi-path support.
- **security:** hybrid post-quantum kex (kyber + x25519) with perfect forward secrecy.
- **privacy:** sealed sender, traffic chaffing, and ephemeral identities.
- **architecture:** memory-locked, zero-copy, zero-allocation rust core.

## principles
- **shadow:** traffic obfuscation masks all activity as generic network noise.
- **stateless:** zero disk footprint. keys are destroyed immediately after use.
- **lockdown:** lan-only enforcement. prevents any data leakage outside local network.

## research & status
this is an experimental project exploring the boundaries of privacy-preserving p2p systems. 
it is currently in a research phase, focusing on hardened security and network optimization.

## stack
- **engine:** rust (100% safe code)
- **crypto:** ring + zeroize + xchacha20-poly1305
- **networking:** quinn (http/3) + mdns-sd

built for academic exploration of digital autonomy and performance.
