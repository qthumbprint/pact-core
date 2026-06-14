# Q-Thumbprint: Pulsar Agreement for Continuous Time (PACT)

Global Navigation Satellite Systems (GNSS) like GPS represent a critical single point of failure for modern digital civilization. To address this crisis of temporal dependency, the Q-Thumbprint Initiative introduces a trustless, permissionless, and politically neutral distributed timing architecture anchored to the natural, incorruptible radio emissions of galactic millisecond pulsars (MSPs).

## Repository Structure

```text
├── README.md                                  <- This root project architecture blueprint
├── LICENSE                                    <- The official GNU GPLv3 copyleft legal terms
├── docs/
│   ├── Q-Thumbprint_Whitepaper.pdf            <- Formal mathematical proofs and network architecture
│   └── Q-Thumbprint Manifesto...              <- Core philosophy and governance commitments
└── pact_simulation.py                         <- The standalone Python consensus simulation engine
```

## The PACT Consensus Pipeline

To achieve distributed consensus on a continuous physical variable (the absolute cosmic arrival time of a pulse), we developed the Pulsar Agreement for Continuous Time (PACT) algorithm. It translates continuous, noisy measurements into an immutable, unified global clock tick through a four-step pipeline.

### Step 1: Signal Extraction (Epoch Folding)

The consumer node records a continuous stream of noisy radio frequency data. Using the known rotation rate of the target pulsar provided by the Tier 1 Distributed Almanac Network (DAN), the software overlaps and stacks millions of periodic intervals:

$$
t_{\text{folded}} = t_{\text{raw}} \pmod P
$$

Random background noise cancels out, revealing a clear pulse profile. The software identifies the peak of this pulse to calculate the specific, continuous local Time of Arrival ($t_{\text{actual}}^{(i)}$).

### Step 2: Cryptographic Authentication

Every node utilizes asymmetric cryptography. The node uses its Private Key to mathematically sign its calculated payload:

$$
\text{Payload}_i = \text{Sign}_{\text{PrivKey}_i}\left( \text{NodeID}_i, \, t_{\text{actual}}^{(i)}, \, \text{Epoch} \right)
$$

The node broadcasts its signed payload to the network. Any alteration to the timestamp in transit will invalidate the signature, allowing honest nodes to automatically discard tampered data.

### Step 3: Pre-Consensus Filtering (Residual Threshold)

To prevent network flooding and instantly eliminate grossly inaccurate data (whether from hardware failure or active spoofing attempts), the network applies a predictive threshold based on a multi-sigma envelope of the hardware precision limit. The network compares the reported time ($t_{\text{actual}}^{(i)}$) to the time predicted by the timing model ($t_{\text{expected}}$).

$$
R_i = \left| t_{\text{actual}}^{(i)} - t_{\text{expected}} \right| \le \delta
$$

Assuming consumer hardware with a baseline noise of $\sigma = 10\mu\text{s}$, the threshold boundary is set to a $3.5\sigma$ envelope ($\delta = 35\mu\text{s}$). This guarantees that 99.9% of valid honest node measurements pass through, while any payload yielding a residual difference $R_i > 35\mu\text{s}$ is instantly discarded.

### Step 4: Byzantine Consensus & Aggregation (Dynamic Trimmed Mean)

The network collects all cryptographically verified and threshold-filtered timestamps and sorts them chronologically:

$$
S = \left[ t_{\text{actual}}^{(1)}, \, t_{\text{actual}}^{(2)}, \, \dots, \, t_{\text{actual}}^{(n_{\text{pool}})} \right] \quad \text{where} \quad t^{(j)} \le t^{(j+1)}
$$

To prevent network starvation while satisfying Byzantine Fault Tolerance, the protocol dynamically calculates the maximum possible fraction of remaining compromised nodes in the active filtered pool:

$$
f_{\text{trim}} = \left\lfloor \frac{n_{\text{pool}} - 1}{3} \right\rfloor
$$

The protocol automatically discards the highest $f_{\text{trim}}$ and lowest $f_{\text{trim}}$ values. The final unified timestamp ($T_{\text{consensus}}$) is calculated as the arithmetic mean of the remaining guaranteed-honest cluster:

$$
T_{\text{consensus}} = \frac{1}{n_{\text{pool}} - 2f_{\text{trim}}} \sum_{k=f_{\text{trim}}+1}^{n_{\text{pool}}-f_{\text{trim}}} S[k]
$$

---

## Reference Simulation Engine

This repository includes a standalone Python simulation (`pact_simulation.py`) that executes the consensus algorithm. It proves that even when dynamically discarding massive fractions of incoming network observations to maintain absolute zero-trust security, the network successfully refines consumer-grade, noisy observations ($\sigma = 10\mu\text{s}$) into a highly secure, sub-microsecond global timestamp.

### Expected Output Log:

```text
--- Initiating PACT Consensus Simulation ---
Target True Cosmic Time: 1500000.0 microseconds
Total Network Nodes (n): 1000
Active Byzantine Attackers (f): 333

[Step 3] Residual Threshold Filter applied.
        Dropped 333 payloads exceeding the 35.0 microsecond threshold limit.
        Payloads remaining in consensus pool: 667

[Step 4] Trimmed Mean applied. Dynamically discarded upper 222 and lower 222 bounds.
        Guaranteed honest nodes remaining for statistical blending: 223

--- CONSENSUS RESULT ---
Calculated Global Timestamp: 1500000.2825 microseconds
Absolute Network Clock Error: 0.2825 microseconds
Success: Network achieved sub-microsecond precision using consumer-grade baselines!
```

---

## Hardware Assembly Blueprint (Under $1,000)

To construct a baseline Tier 2 observation node, operators utilize off-the-shelf components to balance low costs with functional signal acquisition:
* **SDR Receiver ($250):** A Software-Defined Radio receiver (e.g., HackRF One or RTL-SDR Blog V4) capable of capturing frequencies in the 400 MHz to 1.4 GHz range.
* **Low-Noise Amplifier ($50):** An LNA tuned specifically to cosmic radio astronomy bands to boost faint pulsar emissions above local thermal noise.
* **Reflector Antenna ($400):** A home-built 3-meter parabolic mesh satellite dish or a high-gain dual-17-element Yagi antenna array aimed directly at targeted bright pulsars (e.g., PSR B0329+54).
* **Processing Unit ($200):** A standard single-board computer (such as a Raspberry Pi 5) to run the programmatic folding, signing, and transmission pipeline.

*Note: Due to localized atmospheric and ionospheric delay variances, global geodiversity of these nodes is mathematically mandatory to ensure localized noise characteristics are independent and average out to zero.*

## Project Governance & Legal Shield

To permanently protect this technology from private enclosure, corporate monopolization, or state-level classification, the Q-Thumbprint codebase is licensed strictly under the **GNU General Public License v3 (GPLv3)**.

The copyleft provisions of the GPLv3 act as a defensive legal shield: while any commercial entity, individual, or government agency is free to run, copy, and modify the software, they are legally compelled to publish any modified versions under the exact same open, copyleft license. This ensures that any technical improvements built on top of Q-Thumbprint are automatically returned to the global public domain. The stars belong to no one; the math belongs to everyone.

### Copyright & Trademark Notice

The original author and lead theorist of this work is **Gabe Arnold**. The core operational mechanics, technical specifications, and systemic architectures detailed herein are protected under international copyright frameworks (© 2026 Gabe Arnold. All rights reserved). 

**Q-Thumbprint™** and **PACT™ (Pulsar Agreement for Continuous Time)** are proprietary common-law trademarks held by Gabe Arnold, to be assigned in full to the Q-Thumbprint Foundation immediately upon formal non-profit incorporation and receipt of development funding.

### Governance Roadmap

1. **The Incubation Phase:** Gabe Arnold retains master repository maintainer status and sole veto power over core protocol changes to protect early-stage network configurations from malicious vector insertions or early-stage fragmentation. This phase persists until Tier 1 reaches its target deployment of 15 reference observatories.
2. **The Federated Phase:** Core protocol updates pass to a supermajority agreement (11/15) of the verified academic institutions hosting the DAN consensus keys.
3. **The Democratic Phase:** Upon surpassing 10,000 active, globally distributed Tier 2 validation nodes, governance fully transitions to a decentralized community architecture, cementing Q-Thumbprint as a self-sustaining public utility.

---

### Contact & Collaboration

For academic inquiries, peer-review feedback, or to coordinate Tier 1 reference node testing, connect with the project coordination pipeline:
* **Official Domain:** qthumbprint.org
* **Correspondence Inbox:** contact@qthumbprint.org
