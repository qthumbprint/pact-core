# Q-Thumbprint: Software Development Roadmap & Contributor Guide

Welcome to the **Q-Thumbprint Initiative** development roadmap. This document outlines our engineering milestones to build the world's first trustless, decentralized cosmic timing standard anchored to galactic millisecond pulsars (MSPs).

If you are a distributed systems architect, radio astronomer, applied cryptographer, P2P systems engineer, or low-level firmware hacker, we need your help to build this open, un-jammable public temporal commons.

---

## Technical Stack & Domain Layout

To achieve our goals, our architecture is divided across several key domains:

| Layer | Primary Languages | Core Technologies |
| :--- | :--- | :--- |
| **1. Core Consensus** | Python, Rust | BFT Trimmed Mean, PACT Engine |
| **2. Almanac (DAN)** | Go, Rust | Shamir Secret Sharing, P2P Ledger Distribution |
| **3. Signal / DSP** | C++, CUDA, SystemVerilog | Coherent De-dispersion, Epoch Folding Arrays |
| **4. P2P Networking** | Go, Rust | Libp2p, GossipSub Topologies |
| **5. Thin Client** | Embedded C, Rust | ESP32, Raspberry Pi, Local PTP Disciplining |

---

## Development Phases & Milestones

### Phase 1: Core Engine Optimization & Infrastructure Setup
* **Status:** **100% COMPLETE**
* **Delivered Assets:**
    * **Consensus Engine Core:** Standalone execution logic (`pact_simulation.py`) modeling 35-microsecond residual threshold filters and Dynamic Byzantine Slicing (`f_trim`).
    * **Academic Validation:** Formal whitepaper detailing mathematical proofs and statistical blending margins.
    * **Foundational Alignment:** Core philosophical manifesto and licensing structure under the GNU General Public License v3 (GPLv3).
    * **State Lifecycle Specs:** Local node Cold-Start and Coarse/Fine clock acquisition state-machine profiles (`docs/Node_Acquisition_Lifecycle.md`).

---

### Phase 2: Tier 1 Distributed Almanac Network (DAN)
**Objective:** Build the automated ingestion, parsing, and threshold signing layer to aggregate and distribute the cosmic timing models (the Ephemerides) compiled by professional observatories.

    [ IPTA / NANOGrav Archives ] ──> Ingests raw historical .par files
                 │
                 ▼
       [ Almanac Compiler ]      ──> Generates unified JSON Almanac Package
                 │
                 ▼
     [ 15-Key Shamir Ceremony ]  ──> Cryptographically requires T=11 of 15 keys
                 │
                 ▼
       [ Public P2P Gossip ]     ──> Broadcasts signed Almanac down to Tier 2

* **Milestone 2.1: IPTA Data Scrapers & Almanac Compiler**
    * **Goal:** Programmatically monitor and ingest the public data releases of the International Pulsar Timing Array (IPTA) collaborations (including NANOGrav, EPTA, and PPTA).
    * **Tasks:** Write robust parsers to ingest astronomical `.par` configuration files and construct a lightweight, unified, and version-controlled binary payload (the *Global Almanac Package*) detailing target pulsar spin frequencies (`f`), spin-down rates (`f_dot`), and live Dispersion Measures (DM).
* **Milestone 2.2: Shamir Threshold Signature Scheme (TSS)**
    * **Goal:** Implement the decentralized threshold signature system (`n=15, f=4, T=11`) to prevent single-point ephemeris poisoning.
    * **Tasks:** Set up an off-chain secure key generation ceremony using Shamir's Secret Sharing. Develop a lightweight compilation service that assembles a globally valid cryptographic signature once any 11 out of 15 registered curator nodes verify the Almanac's data integrity.

---

### Phase 3: Local Node Firmware & Signal Processing (DSP)
**Objective:** Program the low-level signal capture loops running on physical Tier 2 receiver SDRs to extract clear pulse peaks without GPS dependencies.

* **Milestone 3.1: Inverse Propagation Coherent De-dispersion**
    * **Goal:** Correct the multi-millisecond signal smearing caused by free electrons in the interstellar medium (ISM).
    * **Tasks:** Write high-performance C++/CUDA blocks to apply coherent de-dispersion. The algorithm must use the current Almanac's Dispersion Measure (DM) to apply inverse frequency-dependent time delays to raw RF streams.
* **Milestone 3.2: Taylor-Expanded Rotational Phase Folding**
    * **Goal:** Fold millions of periodic pulses over integration windows without phase smearing.
    * **Tasks:** Implement the rotational phase equation (`phi(t)`) utilizing a Taylor series expansion to continuously adjust for pulsar spin-down decay:
      $$\phi(t) = \phi_0 + f(t - t_0) + \frac{1}{2}\dot{f}(t - t_0)^2$$
* **Milestone 3.3: Relativistic Barycentric Delay Bindings**
    * **Goal:** Translate predictions from the Solar System Barycenter (SSB) to the node's local Earth topocenter.
    * **Tasks:** Port or wrap headless, ultra-lightweight instances of standard astronomical timing engines (like TEMPO2 or PINT) to run directly inside the local embedded client, calculating Roemer, Shapiro, and Einstein delays in real-time.
* **Milestone 3.4: Acquisition State Machine Logic**
    * **Goal:** Execute the Coarse-to-Fine clock locking lifecycle on boot.
    * **Tasks:** Implement the local cold-start state machine. Program the receiver firmware to lock private keys, track high-flux slow pulsars (e.g., `PSR B0329+54`) for coarse millisecond alignment, transition to fast Millisecond Pulsars (MSPs) for fine alignment, and release the consensus private signing keys only when local oscillator tracking variance drops securely below 10 microseconds (`hardware_sigma <= 10.0`).

---

### Phase 4: Peer-to-Peer Networking & Consensus Window
**Objective:** Connect the independent tracking nodes into a globally synchronized peer-to-peer consensus matrix.

* **Milestone 4.1: Libp2p GossipSub Communication Mesh**
    * **Goal:** Establish secure, low-latency node-to-node telemetry broadcasting.
    * **Tasks:** Build a custom P2P communication layer using Libp2p primitives. Nodes must automatically discover network peers and broadcast signed observation payloads containing NodeID, calculated `t_actual`, and active epoch bounds.
* **Milestone 4.2: Asynchronous Consensus Window & Pipeline Execution**
    * **Goal:** Handle real-world network propagation latency and safely execute the PACT dynamic trimmed mean.
    * **Tasks:** Implement a rigid, 60-second temporal Consensus Window post-observation epoch to pool incoming asynchronous packets. Code the active aggregation pipeline execution: payload cryptographic verification, Step 3's 3.5-sigma outlier filtering, chronological array sorting, calculation of the active Byzantine margin (`f_trim`), and final clock statistical blending.

---

### Phase 5: Production Deployment & Tier 3 Edge Client Ecosystem
**Objective:** Transition the infrastructure to an un-foulable public timing commons by launching the hardened backbone network and deploying the open thin-client verification layer.

* **Milestone 5.1: Geodiversity Optimization & Backbone Deployment**
    * **Goal:** Fund and deploy 150 to 300 hardened, institutional-grade Tier 2 nodes (~$100k per node with Rubidium atomic standards) across optimized global latitudes/longitudes.
    * **Tasks:** Write the global geodiversity optimization script to calculate maximum fault-tolerance coverage coordinates. Establish the legal Perpetual Purpose Trust (PPT) frameworks to gift hardware arrays directly to independent community land trusts, rural mesh-net cooperatives, and local university labs.
* **Milestone 5.2: Open Tier 3 Edge Specification & Client**
    * **Goal:** Enable millions of lightweight, ultra-cheap consumer edge receivers to verify the global clock autonomously.
    * **Tasks:** Release open hardware schematics (Raspberry Pi/ESP32 + low-cost crystal clock components). Write the lightweight edge software allowing consumer-grade nodes to capture and cryptographically verify the signed consensus timestamps broadcast by nearby Tier 2 backbone nodes.

---

## How to Contribute

We are actively seeking maintainers and core contributors for the following technical roles:

### 1. DSP & Software Defined Radio Engineers (C++ / CUDA / FPGA)
* **Scope:** Coherent de-dispersion matrices, real-time epoch folding engines, and hardware acceleration pipelines on embedded platforms (Zynq FPGAs, Ettus USRPs, Jetson computing modules).
* **Keywords:** SDR, FFT, Polyphase Filter Banks, GNU Radio, RF Frontends, Open-Source Astronomy.

### 2. Applied Cryptographers & Protocol Engineers (Go / Rust)
* **Scope:** Implementing Shamir-based Threshold Signature Schemes (TSS) and validating asymmetric key distribution systems on resource-constrained embedded nodes.
* **Keywords:** Threshold Cryptography, Shamir Secret Sharing, Multi-Party Computation, Ed25519.

### 3. P2P Distributed Systems Engineers (Go / Rust)
* **Scope:** Libp2p network mesh architecture, GossipSub protocol tuning, network latency reduction, and asynchronous consensus handling across partitioned topologies.
* **Keywords:** Libp2p, DHT, GossipSub, NAT Traversal, Network Simulation, Byzantine Fault Tolerance.

### 4. Technical Writers & Radio Amateurs
* **Scope:** Compiling step-by-step physical hardware assembly blueprints, tuning local parabolic mesh tracking arrays, and documenting local cold-start lifecycle routines.
* **Keywords:** Hardware Specs, Ham Radio, Parabolic Reflector Arrays, Geodetic Surveying, Clock Disciplining.

---

## Getting Started

1. **Review the Whitepaper:** Read `/docs/Q-Thumbprint_Whitepaper.pdf` for deep mathematical and algorithmic foundations.
2. **Run the Simulation:** Execute `pact_simulation.py` to see the Step 3 multi-sigma filter and Step 4 Dynamic Byzantine Slicing functions execute in real-time.
3. **Pick an Issue:** Check our GitHub Issues tab for tags marked `Good First Issue` or jump straight into the ongoing architectural design discussions on our development mailing list.

---

*Time is a fundamental dimension of human coordination. Let's ensure it belongs to no single authority, and serves all of humanity.*

**Join the Q-Thumbprint Initiative. Let's claim our temporal birthright.**
