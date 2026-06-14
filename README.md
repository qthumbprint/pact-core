# Q-Thumbprint: Cosmic Timing for Human Freedom

The temporal foundation of modern digital civilization is monopolized by state-controlled, military-operated satellite constellations (GPS, GLONASS, Galileo, Beidou). This centralization introduces a catastrophic systemic point of failure, subjecting global networks to localized jamming, carrier-phase spoofing, and geopolitical manipulation. 

**Q-Thumbprint** is a trustless, permissionless, and politically neutral distributed timing architecture anchored to the natural, incorruptible radio emissions of galactic millisecond pulsars (MSPs). By utilizing the **Pulsar Agreement for Continuous Time (PACT)** consensus algorithm, Q-Thumbprint aggregates noisy, sub-$1,000 consumer-grade radio antennas into an un-jammable, globally synchronized atomic-precision clock standard.

---

## Technical Overview

The network addresses the trade-off between astronomical measurement precision and democratic hardware scale by utilizing a specialized two-tiered network topology:

1. **Tier 1: Distributed Almanac Network (DAN)** – A federated consortium of 15 world-class radio observatories tracking the pulsar mesh to maintain an updated, cryptographically signed predictive timing model (the ephemeris).
2. **Tier 2: Distributed Pulsar Network (DPN)** – A highly scalable, crowd-sourced mesh of 10,000+ geographically independent consumer validation nodes capturing raw cosmic signals.

Through the integration of the Central Limit Theorem and Approximate Byzantine Agreement via a sorted Trimmed Mean, the network successfully eliminates malicious actors trying to poison the clock. Even under a maximum-scale Byzantine attack where up to one-third ($1/3$) of all validation nodes are actively compromised, PACT refines noisy consumer inputs ($\sigma = 10\mu\text{s}$) into a secure, sub-microsecond ($\sim 0.547\mu\text{s}$) global timestamp.

---

## Repository Structure

* `/docs` — Contains the official academic whitepaper: *PACT: A Two-Tiered Byzantine Fault-Tolerant Architecture for Geographically Distributed Cosmic Timing Standards*.
* `/sim` — The Python-based network simulation engine proving Byzantine resilience boundaries.
* `/hardware` — Open-source schematics for constructing an under-$1,000 ground tracking array using Software-Defined Radio (SDR) and mesh satellite dishes.

---

## Running the Verification Simulator

To validate the algorithmic logic of the PACT consensus pipeline and observe its resilience against active clock-poisoning attacks, you can execute the reference simulator instantly. The engine models a 1,000-node validation network under an active 33.3% Byzantine attack vector.

### Quick Start
Ensure you have Python 3.x installed on your local machine. No external dependencies or heavy frameworks are required.

```bash
# Clone the core repository
git clone [https://github.com/qthumbprint/pact-core.git](https://github.com/qthumbprint/pact-core.git)

# Navigate to the simulation directory
cd pact-core/sim

# Run the consensus simulation
python pact_simulation.py
