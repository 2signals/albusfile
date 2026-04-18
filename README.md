# albus

an experimental research project on privacy-preserving, local-first p2p transfer systems.

albus explores the design space of secure, ephemeral, and low-trust peer-to-peer communication in constrained local network environments.

---

## scope

albus is a **research prototype**, not a production-ready secure messaging or file transfer system.

it investigates:
- ephemeral secure channels
- lan-bound peer-to-peer communication
- post-quantum hybrid key exchange
- metadata reduction techniques in local networks

---

## threat model

albus operates under a **compromised local network assumption**.

### adversarial capabilities

the attacker may:

- passively observe all network traffic (packet capture, timing, size analysis)
- perform traffic correlation and metadata inference
- attempt man-in-the-middle attacks within the local network
- inject, replay, or modify packets (active network attacker)
- impersonate peers in discovery mechanisms (e.g. multicast-based discovery)

endpoint compromise (e.g. OS-level malware, keyloggers) is explicitly out of scope.

---

## security goals

albus aims to provide the following properties under the above threat model:

### confidentiality
- payloads remain confidential against passive and active network observers
- encryption is based on modern authenticated encryption schemes

### forward secrecy
- compromise of long-term keys does not reveal past session data
- session keys are ephemeral and destroyed after use

### identity minimization
- peer identities are ephemeral and session-scoped
- system avoids persistent device identifiers where possible

### metadata reduction (best-effort)
- attempts to reduce linkability of communication patterns
- does not claim complete resistance against traffic analysis

---

## cryptographic design

albus uses a hybrid key exchange approach:

- classical key exchange: X25519
- post-quantum key encapsulation: CRYSTALS-Kyber
- symmetric encryption: XChaCha20-Poly1305

key derivation follows a standard HKDF-based expansion model.

session keys are:
- derived per connection
- stored only in memory
- explicitly zeroized after use

---

## transport layer

- QUIC-based transport for multiplexed secure streams
- 0-RTT supported with replay-awareness considerations
- experimental multipath support

transport stack:
- QUIC protocol implementation via Quinn

limitations:
- 0-RTT usage is treated as optional and requires replay risk awareness
- multipath behavior may vary across platforms

---

## peer discovery

peer discovery is performed via local network multicast discovery (mDNS-based approach).

security considerations:
- discovery messages are not trusted by default
- peers must complete cryptographic handshake before interaction
- discovery layer is considered unauthenticated

---

## session protocol (high-level)

1. peer discovery (unauthenticated)
2. handshake initiation (ephemeral identity generated)
3. hybrid key exchange (X25519 + Kyber)
4. session key derivation (HKDF)
5. encrypted transport channel established
6. session termination → memory wipe of secrets

no persistent session state is stored on disk.

---

## privacy mechanisms

albus implements the following best-effort privacy techniques:

- ephemeral session identities
- sealed sender-style messaging model (receiver verifies origin via session context)
- optional traffic padding / chaffing experiments
- minimal logging and no persistent storage by design

note:
traffic analysis resistance is not guaranteed and remains an open research problem.

---

## memory safety and lifecycle

- implemented in rust for memory safety guarantees
- sensitive buffers are zeroized after use
- memory locking (mlock where available) is used to reduce swap exposure
- zero-copy design is used where feasible for performance and isolation

limitations:
- cannot guarantee complete absence of forensic traces at OS or hardware level

---

## architecture

- language: rust
- crypto: ring, zeroize
- transport: quinn (QUIC)
- discovery: mDNS-based service discovery

design principles:
- local-first communication
- ephemeral state
- minimal persistence
- explicit trust boundaries

---

## non-goals

albus does NOT attempt to provide:

- anonymity against global adversaries
- resistance against compromised endpoints
- perfect traffic analysis resistance
- regulatory compliance guarantees
- secure identity management system for long-term authentication

---

## research status

this project is in active research and prototyping phase.

its goal is to explore:
- practical limits of hybrid post-quantum key exchange in local networks
- secure ephemeral P2P session design
- trade-offs between performance and privacy in LAN-only systems

---

## stack

- engine: rust (systems programming language)
- transport: quinn (QUIC)
- crypto:
  - XChaCha20-Poly1305
  - X25519
  - CRYSTALS-Kyber
  - HKDF
- utilities:
  - zeroize
  - mDNS service discovery

---

## philosophy

albus is built for exploring a simple question:

> what is the minimal secure and private communication system that can exist entirely within a local network without persistent trust?

---
