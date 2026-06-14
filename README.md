# Q-Thumbprint: Pulsar Agreement for Continuous Time (PACT)

Global Navigation Satellite Systems (GNSS) like GPS represent a critical single point of failure for modern digital civilization. To address this crisis of temporal dependency, the Q-Thumbprint Initiative introduces a trustless, permissionless, and politically neutral distributed timing architecture anchored to the natural, incorruptible radio emissions of galactic millisecond pulsars (MSPs).

## Repository Structure

```
├── README.md                                  <- This root project architecture blueprint
├── LICENSE                                    <- The official GNU GPLv3 copyleft legal terms
├── docs/
│   ├── Q-Thumbprint_Whitepaper.pdf            <- Formal mathematical proofs and network architecture
│   └── Q-Thumbprint Manifesto & Principles.md <- Core philosophy and governance commitments
└── pact_simulation.py                         <- The standalone Python consensus simulation engine
```

## The PACT Consensus Pipeline (Software Architecture)

To achieve distributed consensus on a continuous physical variable (the absolute cosmic arrival time of a pulse), we developed the Pulsar Agreement for Continuous Time (PACT) algorithm. Because epoch folding requires extended observation windows to build signal-to-noise ratio (SNR), PACT functions as a **global clock disciplining protocol**. It translates continuous, noisy RF measurements into highly precise, discrete consensus timestamps (anchor points) which are used to continually discipline and correct local hardware oscillators through a four-step pipeline.

### Step 1: Signal Extraction (De-dispersion & Epoch Folding)

The consumer node records a continuous stream of broadband radio frequency data. Because the interstellar medium slows down lower frequencies, the node first applies **coherent de-dispersion** using the pulsar's Dispersion Measure (DM) provided by the Tier 1 Distributed Almanac Network (DAN) ephemeris.

Next, the software folds the corrected data. Because pulsars spin down over time, the folding relies on the Taylor series expansion of the rotational phase $\phi(t)$ rather than a static modulo:

$$
\phi(t) = \phi_0 + f(t - t_0) + \frac{1}{2}\dot{f}(t - t_0)^2
$$

Random background noise cancels out, revealing a clear pulse profile. The software identifies the peak of this pulse. Using a standard astronomical timing package (e.g., TEMPO2), the node accounts for relativistic barycentric-to-topocentric delays to calculate the highly specific, continuous local Time of Arrival, denoted as `t_actual`.

### Step 2: Cryptographic Authentication

Every node utilizes asymmetric cryptography. The node uses its Private Key to mathematically sign its calculated payload:

$$
\text{Payload}_i = \text{Sign}_{\text{PrivKey}_i}\left( \text{NodeID}_i, \, t_{\text{actual}}^{(i)}, \, \text{Epoch} \right)
$$

The node broadcasts its signed payload to the network. Any alteration to the timestamp in transit will invalidate the signature, allowing honest nodes to automatically discard tampered data.

### Step 3: Pre-Consensus Filtering (Residual Threshold)

To prevent network flooding and instantly eliminate grossly inaccurate data (whether from hardware failure or active spoofing attempts), the network applies a predictive threshold based on a multi-sigma envelope of the hardware precision limit. The network compares the reported time (`t_actual`) to the time predicted by the timing model (`t_expected`).

$$
R_i = \left| t_{\text{actual}}^{(i)} - t_{\text{expected}} \right| \le \delta
$$

Assuming theoretical consumer hardware with a baseline noise of $\sigma = 10\mu\text{s}$, the threshold boundary is set to a $3.5\sigma$ envelope ($\delta = 35\mu\text{s}$). This guarantees that 99.9% of valid honest node measurements pass through, while any payload yielding a residual difference `R_i > 35μs` is instantly discarded.

### Step 4: Byzantine Consensus & Aggregation (Dynamic Trimmed Mean)

Due to the latency of globally asynchronous networks, the protocol utilizes a strict **Consensus Window** (e.g., 60 seconds post-observation). Once this temporal window closes, the network pool locks. It collects all cryptographically verified and threshold-filtered timestamps that arrived within the window and sorts them chronologically:

$$
S = \left[ t_{\text{actual}}^{(1)}, \, t_{\text{actual}}^{(2)}, \, \dots, \, t_{\text{actual}}^{(n_{\text{pool}})} \right] \quad \text{where} \quad t^{(j)} \le t^{(j+1)}
$$

To prevent network starvation while satisfying Byzantine Fault Tolerance, the protocol dynamically calculates the maximum possible fraction of remaining compromised nodes in the active filtered pool (`f_trim`):

$$
f_{\text{trim}} = \left\lfloor \frac{n_{\text{pool}} - 1}{3} \right\rfloor
$$

The protocol automatically discards the highest `f_trim` and lowest `f_trim` values. The final unified timestamp (`T_consensus`) is calculated as the arithmetic mean of the remaining guaranteed-honest cluster:

$$
T_{\text{consensus}} = \frac{1}{n_{\text{pool}} - 2f_{\text{trim}}} \sum_{k=f_{\text{trim}}+1}^{n_{\text{pool}}-f_{\text{trim}}} S[k]
$$

---

## Reference Simulation Engine

This repository includes a standalone Python simulation (`pact_simulation.py`) that executes the consensus algorithm. It proves that even when dynamically discarding massive fractions of incoming network observations to maintain absolute zero-trust security, the network successfully refines consumer-grade, noisy observations ($\sigma = 10\mu\text{s}$) into a highly secure, sub-microsecond global timestamp.

### Expected Output Log:

```
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

## Theoretical Hardware Assembly Blueprint

The theoretical target for a baseline Tier 2 observation node utilizes off-the-shelf components to balance low costs with functional signal acquisition (Sub-$1,000 target):

* **SDR Receiver ($250):** A Software-Defined Radio receiver (e.g., HackRF One or RTL-SDR Blog V4) capable of capturing frequencies in the 400 MHz to 1.4 GHz range.
* **Low-Noise Amplifier ($50):** An LNA tuned specifically to cosmic radio astronomy bands to boost faint pulsar emissions above local thermal noise.
* **Reflector Antenna ($400):** A home-built 3-meter parabolic mesh satellite dish or a high-gain dual-17-element Yagi antenna array aimed directly at targeted bright pulsars.
* **Processing Unit ($200):** A standard single-board computer (such as a Raspberry Pi 5) to run the programmatic folding, signing, and transmission pipeline.

*Note: Due to localized atmospheric and ionospheric delay variances, global geodiversity of these nodes is mathematically mandatory to ensure localized noise characteristics are independent and average out to zero.*

### Deployment Configuration Options

The Q-Thumbprint framework supports two native deployment configurations depending on network maturity:

1. **The Democratic Mesh (Default):** A highly scalable, crowd-sourced mesh of thousands of sub-$1,000 consumer installations balancing thermal drift via dense statistical averaging across the Central Limit Theorem.
2. **The Hardened Three-Tier Backbone (Enterprise):** A high-reliability institutional configuration where philanthropic patrons fund 150–300 enterprise-grade backbone nodes (~$100k per node with Rubidium standards) that are gifted to independent community stewards. This heavy spine serves as a clock-disciplining broadcast layer that supports millions of lightweight, ultra-cheap sub-$100 Tier 3 consumer edge devices for local cryptographic auditing.

---

## Limitations & Future Hardware R&D

While the software consensus architecture (PACT) is fully functional and mathematically sound, deploying an independent, sub-$1,000 ground node currently faces physical engineering bottlenecks that represent the next frontier of the Q-Thumbprint Initiative's R&D:

1. **The Local Oscillator (Clock Drift) Trap:** To accurately perform epoch folding over long periods, the SDR's local clock must remain perfectly stable. Standard TCXOs in consumer SDRs drift by ~1 ppm, which smears signal data by milliseconds over an hour. Achieving $10\mu\text{s}$ bounds currently requires non-GPS frequency stabilization (e.g., OCXOs or atomic standards), which challenges the sub-$1,000 hardware budget.
2. **Signal-to-Noise Ratio (SNR) Limits:** While 3-meter dishes can detect bright, slow pulsars (like PSR B0329+54), achieving precision timing requires Millisecond Pulsars (MSPs). MSPs are incredibly faint, and detecting them with a 3-meter dish without being overwhelmed by terrestrial thermal noise will likely require advancements in consumer-grade phased array combining.
3. **The Geographic Bootstrap Paradox:** To calculate localized `t_expected`, a node must know its exact geographic coordinates down to a few meters. Until an alternative global geodetic standard exists, new Tier 2 nodes will initially require standard surveying or a one-time GPS coordinate ping during installation to establish their baseline location before operating independently.

---

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

For academic inquiries, peer-review hardware engineering proposals, or to coordinate Tier 1 reference node testing, connect with the project coordination pipeline:
* **Official Domain:** qthumbprint.org
* **Correspondence Inbox:** contact@qthumbprint.org
