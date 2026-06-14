# Q-Thumbprint: Node Acquisition & Lock Lifecycle Specification

## 1. Overview
The Pulsar Agreement for Continuous Time (PACT) algorithm defines a zero-trust consensus framework for a steady-state distributed clock network. However, on initial boot, power cycling, or extended offline durations (a "Cold Start"), a local node's un-disciplined hardware oscillator exhibits an arbitrary phase offset and thermal drift that can easily exceed milliseconds.

Because the steady-state PACT protocol enforces a strict Step 3 Pre-Consensus residual threshold filter limit ($\delta = 35\,\mu\text{s}$ based on an expected consumer noise standard deviation of $\sigma = 10\,\mu\text{s}$), a cold-booted node will yield massive initial residual errors ($R_i \gg 35\,\mu\text{s}$). Consequently, its payloads will be rejected, locking the node out of the consensus pool and preventing it from receiving the precise timestamps needed to discipline its local clock.

To break this bootstrap paradox without introducing external satellite synchronization dependencies, all Tier 2 and Tier 3 node tracking engines must implement this local **Acquisition & Lock Lifecycle Specification**. This protocol acts as a local firmware wrapper that establishes temporal alignment prior to PACT network engagement.

---

## 2. The Node Lifecycle State Machine

A node must sequentially advance through four operational states before it is authorized to participate in the global cryptographic timing consensus layer.

+──────────────────────────────────────────────────────────+
│                  STATE 0: COLD START                     │
+──────────────────────────────────────────────────────────+
│
▼
+──────────────────────────────────────────────────────────+
│             STATE 1: COARSE DISCIPLINING                 │
│  - Target: High-Flux, Slow Pulsars (e.g., PSR B0329+54)  │
│  - Phase Envelope: Milliseconds                          │
+──────────────────────────────────────────────────────────+
│
▼
+──────────────────────────────────────────────────────────+
│               STATE 2: FINE TRACKING LOCK                │
│  - Target: Ultra-Stable Millisecond Pulsars (MSPs)       │
│  - Phase Envelope: < 10 Microseconds                     │
+──────────────────────────────────────────────────────────+
│
▼
+──────────────────────────────────────────────────────────+
│             STATE 3: PACT NETWORK CONSENSUS              │
│  - Cryptographic key pair unlocked                       │
│  - Broadcast signed t_actual payloads                    │
+──────────────────────────────────────────────────────────+

### State 0: Cold Start
* **Definition:** The node has zero trusted internal time data, no current ephemeris cache, or a local oscillator drift configuration that exceeds the acceptable $3.5\sigma$ network envelope.
* **Firmware Constraints:**
  * Master network private cryptographic signing keys **MUST** remain locked.
  * Payloads **MUST NOT** be broadcast to the network.
  * The local digital signal processing (DSP) stack initiates a bootstrap routine to ingest the signed Tier 1 Distributed Almanac Network (DAN) ephemeris file.

### State 1: Coarse Disciplining Phase
* **Objective:** Absorb macro-level timing errors and reduce local oscillator tracking offsets from the millisecond/second regime down to a sub-millisecond window.
* **Targeting Logic:** The firmware bypasses fast, faint Millisecond Pulsars (MSPs) and commands the radio frontend to lock onto bright, high-flux density, slow-period pulsars (e.g., `PSR B0329+54`).
* **Signal Processing Mechanics:**
  * Because slow pulsars possess wide pulse profiles and long periods, a node with severe local clock drift can still isolate the pulse's peak power.
  * The software executes a wide-window cross-correlation between the observed pulse profile and the reference template provided by the DAN ephemeris.
  * The firmware iteratively steps the local clock to align the mathematical peak of the slow profile. This coarse alignment is maintained until clock errors drop below $500\,\mu\text{s}$.

### State 2: Fine Tracking Lock Phase
* **Objective:** Achieve high-precision frequency stabilization and pull the local clock variance firmly within the steady-state threshold required by the consensus pool.
* **Targeting Logic:** The firmware switches the antenna tracking array or beamforming vector from slow pulsars to highly stable Millisecond Pulsars (MSPs).
* **Signal Processing Mechanics:**
  * The software initializes extended epoch-folding integration windows (ranging from 15 to 60 minutes) to extract the faint MSP signals from the thermal noise floor.
  * The folding algorithm relies on the Taylor series expansion of the rotational phase $\phi(t)$ to account for pulsar spin-down:
    $$\phi(t) = \phi_0 + f(t - t_0) + \frac{1}{2}\dot{f}(t - t_0)^2$$
  * A standard astronomical timing framework (e.g., TEMPO2 or PINT) is executed locally to calculate relativistic corrections—specifically accounting for Roemer, Shapiro, and Einstein delays—to map the Solar System Barycenter (SSB) predictions onto the node's topocentric coordinate frame.
  * The tracking loop continually adjusts the clock phase until the local measurement standard deviation satisfies the protocol ceiling: `hardware_sigma <= 10.0 microseconds`.

### State 3: PACT Network Consensus (Steady State)
* **Definition:** The node achieves "Consensus Lock" status.
* **Firmware Execution:**
  * The local node's private cryptographic key pair is unlocked.
  * The node begins generating signed `Payload_i` packages using its continuous local Time of Arrival measurements (`t_actual`).
  * Payloads are broadcast to the network mesh. Because the local oscillator has been pre-disciplined to a $\sigma \le 10\,\mu\text{s}$ baseline, these measurements cleanly pass the Step 3 Pre-Consensus Multi-Sigma envelope filter ($R_i \le 35\,\mu\text{s}$), contributing to the secure, global dynamic trimmed mean.

---

## 3. Exception Handling & Safe Recovery Loops

To preserve the network from data pollution while guaranteeing that malfunctioning hardware can self-heal, the node firmware must enforce two automated recovery loops:

1. **The Threshold Rejection Reset:** If a node in State 3 has its broadcast payloads rejected by the network's Step 3 filter for 3 consecutive consensus epochs, the network layer assumes the local oscillator has decoupled due to severe thermal shock or hardware anomaly. The node must automatically lock its private keys, terminate broadcasts, drop to **State 2**, and re-verify its fine tracking lock.
