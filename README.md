# Q-Thumbprint: Cosmic Timing for Human Freedom
The temporal foundation of modern digital civilization is monopolized by state-controlled, military-operated satellite constellations (GPS, GLONASS, Galileo, Beidou). This extreme centralization introduces a catastrophic systemic point of failure, subjecting global telecommunications, financial markets, and electrical grids to localized jamming, carrier-phase spoofing, orbital hardware anomalies, and arbitrary geopolitical manipulation.
**Q-Thumbprint** is a trustless, permissionless, and politically neutral distributed timing architecture anchored to the natural, incorruptible radio emissions of galactic millisecond pulsars (MSPs). By utilizing the **Pulsar Agreement for Continuous Time (PACT)** consensus algorithm, Q-Thumbprint aggregates noisy, sub-$1,000 consumer-grade radio antennas into an un-jammable, globally synchronized atomic-precision clock standard.
# Technical Overview
The network resolves the traditional trade-off between astronomical measurement precision and democratic hardware scale by utilizing a specialized, two-tiered network topology:
1. **Tier 1: Distributed Almanac Network (DAN)** – A federated consortium of 15 world-class radio observatories (such as MeerKAT, Jodrell Bank, and the Parkes Observatory) tracking the pulsar mesh to maintain an updated, cryptographically signed predictive timing model (the ephemeris).
2. **Tier 2: Distributed Pulsar Network (DPN)** – A highly scalable, crowd-sourced mesh of 10,000+ geographically independent consumer validation nodes capturing raw cosmic signals using basic Software-Defined Radios (SDRs).

⠀Through the integration of the Central Limit Theorem and Approximate Byzantine Agreement via a sorted Trimmed Mean, the network successfully eliminates malicious actors trying to poison the clock. Even under a maximum-scale Byzantine attack where up to one-third ($1/3$) of all validation nodes are actively compromised, PACT refines noisy consumer inputs ($\sigma = 10\mu\text{s}$) into a secure, sub-microsecond ($\sim 0.547\mu\text{s}$) global timestamp.
# Repository Structure
├── README.md            <- This root project architecture blueprint and manifesto
├── LICENSE              <- The official GNU GPLv3 copyleft legal terms
└── pact_simulation.py   <- The standalone Python consensus simulation engine

# The PACT Consensus Pipeline
To achieve distributed consensus on a continuous physical variable (cosmic time of arrival), the Q-Thumbprint framework utilizes the PACT Algorithm—a sequential, four-step pipeline. This ensures data integrity, cryptographic security, and mathematical alignment with established Byzantine fault tolerance parameters.
### Step 1: Signal Extraction (Epoch Folding)
The physical layer challenge involves extracting a sub-microsecond pulse from the cosmic noise floor.
* **The Process:** The consumer node records a continuous stream of noisy radio frequency data. Using the known rotation rate of the target pulsar (the ephemeris) provided by the DAN, the software overlaps and stacks millions of periodic intervals:   $$t_{\text{folded}} = t_{\text{raw}} \pmod P$$
* **The Output:** Random background noise cancels out, revealing a clear pulse profile. The software identifies the peak of this pulse to calculate the specific, continuous local Time of Arrival ($t_{\text{actual}}^{(i)}$).

⠀Step 2: Cryptographic Authentication
Before the data enters the network, it must be secured against spoofing and in-transit tampering.
* **The Process:** Every node utilizes asymmetric cryptography (Public/Private Keys). The node uses its Private Key to mathematically sign its calculated payload:   $$\text{Payload}_i = \text{Sign}_{\text{PrivKey}_i}\left( \text{NodeID}_i, \, t_{\text{actual}}^{(i)}, \, \text{Epoch} \right)$$
* **The Output:** The node broadcasts its signed payload to the network. Any alteration to the timestamp in transit will invalidate the signature, allowing honest nodes to automatically discard tampered data.

⠀Step 3: Pre-Consensus Filtering (Residual Threshold)
To prevent network flooding and instantly eliminate grossly inaccurate data (whether from hardware failure or active spoofing attempts), the network applies a strict predictive threshold based on the known hardware precision limit of 10 microseconds.
* **The Process:** The network compares the reported time ($t_{\text{actual}}^{(i)}$) to the time predicted by the timing model ($t_{\text{expected}}$).
* **The Filter Equation:**   $$R_i = \left| t_{\text{actual}}^{(i)} - t_{\text{expected}} \right| \le \delta$$ Where the threshold boundary $\delta = 10\mu\text{s}$. Any payload yielding a residual difference $R_i > 10\mu\text{s}$ is instantly discarded before entering the consensus aggregation stage.

⠀Step 4: Byzantine Consensus & Aggregation (Trimmed Mean)
Standard discrete Byzantine Fault Tolerance (e.g., PBFT) cannot process the natural continuous variance of honest analog measurements. PACT utilizes Approximate Byzantine Agreement via a sorted Trimmed Mean.
* **The Process:** The network collects all cryptographically verified and threshold-filtered timestamps and sorts them chronologically:   $$S = \left[ t_{\text{actual}}^{(1)}, \, t_{\text{actual}}^{(2)}, \, \dots, \, t_{\text{actual}}^{(n_{\text{pool}})} \right] \quad \text{where} \quad t^{(j)} \le t^{(j+1)}$$
* **The Aggregation Equation:** To satisfy the Byzantine tolerance of $f$ maximum faulty nodes (where $n = 1000$ and $f = 333$), the protocol automatically discards the highest $f$ and lowest $f$ values. The final unified timestamp ($T_{\text{consensus}}$) is calculated as the arithmetic mean of the remaining honest cluster:   $$T_{\text{consensus}} = \frac{1}{n_{\text{pool}} - 2f} \sum_{k=f+1}^{n_{\text{pool}}-f} S[k]$$

⠀Running the Verification Simulator
To validate the algorithmic logic of the PACT consensus pipeline and observe its resilience against active clock-poisoning attacks, you can execute the reference simulator instantly. The engine models a 1,000-node validation network under an active 33.3% Byzantine attack vector.
### Quick Start
Ensure you have Python 3.x installed on your local machine. No external dependencies or heavy frameworks are required.
# Clone the core repository
git clone [https://github.com/qthumbprint/pact-core.git](https://github.com/qthumbprint/pact-core.git)

# Navigate to the directory
cd pact-core

# Run the consensus simulation
python pact_simulation.py

### Expected Output Pipeline
When executed, the simulator initializes a raw target cosmic timestamp, injects 333 malicious actors trying to introduce a violent clock drift, processes the data through the PACT residual threshold filters, and executes the statistical trimmed mean to generate a sub-microsecond output:
--- Initiating PACT Consensus Simulation ---
Target True Cosmic Time: 1500000.0 microseconds
Total Network Nodes (n): 1000
Active Byzantine Attackers (f): 333

[Step 3] Residual Threshold Filter applied.
        Dropped 333 payloads exceeding the 10 microsecond threshold limit.
        Payloads remaining in consensus pool: 667

[Step 4] Trimmed Mean applied. Discarded upper 333 and lower 333 bounds.
        Guaranteed honest nodes remaining for statistical blending: 334

--- CONSENSUS RESULT ---
Calculated Global Timestamp: 1500000.1241 microseconds
Absolute Network Clock Error: 0.1241 microseconds
Success: Network achieved sub-microsecond precision using consumer-grade baselines!

# Hardware Assembly Blueprint (Under $1,000)
To construct a baseline Tier 2 observation node, operators utilize off-the-shelf components to balance low costs with functional signal acquisition:
* **SDR Receiver ($250):** A Software-Defined Radio receiver (e.g., HackRF One or RTL-SDR Blog V4) capable of capturing frequencies in the 400 MHz to 1.4 GHz range.
* **Low-Noise Amplifier ($50):** An LNA tuned specifically to cosmic radio astronomy bands to boost faint pulsar emissions above local thermal noise.
* **Reflector Antenna ($400):** A home-built 3-meter parabolic mesh satellite dish or a high-gain dual-17-element Yagi antenna array aimed directly at targeted bright pulsars (e.g., PSR B0329+54).
* **Processing Unit ($200):** A standard single-board computer (such as a Raspberry Pi 5) to run the programmatic folding, signing, and transmission pipeline.

⠀*Note: Due to localized atmospheric and ionospheric delay variances, global geodiversity of these nodes is mathematically mandatory to ensure localized noise characteristics are independent and average out to zero.*

# Project Governance & Legal Shield
To permanently protect this technology from private enclosure, corporate monopolization, or state-level classification, the Q-Thumbprint codebase is licensed strictly under the **GNU General Public License v3 (GPLv3)**.
The copyleft provisions of the GPLv3 act as a defensive legal shield: while any commercial entity, individual, or government agency is free to run, copy, and modify the software, they are legally compelled to publish any modified versions under the exact same open, copyleft license. This ensures that any technical improvements built on top of Q-Thumbprint are automatically returned to the global public domain. The stars belong to no one; the math belongs to everyone.

### Copyright & Trademark Notice
The original author and lead theorist of this work is **Gabe Arnold**. The core operational mechanics, technical specifications, and systemic architectures detailed herein are protected under international copyright frameworks (© 2026 Gabe Arnold. All rights reserved). 

**Q-Thumbprint™** and **PACT™ (Pulsar Agreement for Continuous Time)** are proprietary common-law trademarks held by Gabe Arnold, to be assigned in full to the Q-Thumbprint Foundation immediately upon formal non-profit incorporation and receipt of development funding.

### Governance Roadmap
1. **The Incubation Phase:** Gabe Arnold retains master repository maintainer status and sole veto power over core protocol changes to protect early-stage network configurations from malicious vector insertions or early-stage fragmentation. This phase persists until Tier 1 reaches its target deployment of 15 reference observatories.
2. **The Federated Phase:** Core protocol updates pass to a supermajority agreement ($11/15$) of the verified academic institutions hosting the DAN consensus keys.
3. **The Democratic Phase:** Upon surpassing 10,000 active, globally distributed Tier 2 validation nodes, governance fully transitions to a decentralized community architecture, cementing Q-Thumbprint as a self-sustaining public utility.

Contact & Collaboration
For academic inquiries, peer-review feedback, or to coordinate Tier 1 reference node testing, connect with the project coordination pipeline:
* **Official Domain:** ~[qthumbprint.org](https://qthumbprint.org/)~
* **Correspondence Inbox:** contact@qthumbprint.org
