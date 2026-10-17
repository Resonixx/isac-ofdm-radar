# isac-ofdm-radar

Simulation of OFDM-based integrated sensing and communication (ISAC): how splitting time-frequency resources between data and radar sensing trades off data rate against detection performance.

> **Status:** early development (bachelor's thesis project). No results yet; the roadmap below shows what is planned.

## Overview

In ISAC, one OFDM waveform both carries data and acts as a radar. The base station sends a signal, and the echo reflected from a moving target gives its range and velocity. But the resources are shared: every subcarrier used for sensing is one less for data.

This project studies that trade-off in simulation:

- How much data rate is lost for a given radar detection requirement?
- Which allocation pattern of sensing subcarriers and symbols works best: a contiguous block, a uniform comb, a random pattern, or an optimized one?
- Can a heuristic optimizer (greedy search, genetic algorithm) beat the classic patterns?

## Model

| Parameter | Value |
|---|---|
| Subcarriers (N) | 256 |
| Subcarrier spacing (Δf) | 120 kHz |
| OFDM symbols (M) | 64 |
| Carrier frequency | 28 GHz |
| Radar symbols | known, constant-modulus (PSK / Zadoff-Chu) |
| Target | one or more point targets (range, velocity) |
| Noise | AWGN |

Radar processing follows the standard OFDM-radar chain: divide the received grid by the known transmitted symbols, take an IDFT across subcarriers (range) and a DFT across symbols (Doppler), then detect peaks in the range-Doppler map with a CFAR detector.

Resolution limits used for validation:

- Range resolution: `ΔR = c / (2 · B)`, with `B` the occupied bandwidth
- Velocity resolution: `Δv = λ / (2 · M · T_sym)`

## Metrics

- Range and Doppler resolution (3 dB mainlobe width)
- Peak sidelobe level (PSL) within a region of interest
- Detection probability at a fixed false-alarm rate
- Data rate (Shannon bound per data subcarrier)

## Planned experiments

1. Baseline allocation patterns on the time-frequency grid: TDM, FDM, uniform, random
2. Rate-versus-detection trade-off curves for each pattern
3. Optimized patterns: greedy construction with single-subcarrier exchange, then a genetic algorithm minimizing PSL in a region of interest under a resolution constraint
4. Fair comparison against strong baselines, including best-of-N random search
5. Optional: CNN detector versus CFAR

## Roadmap

- [ ] Phase 0: OFDM and resource-grid basics
- [ ] Phase 1: OFDM transmitter and receiver, BER curve validated against theory
- [ ] Phase 2: target channel and range-Doppler map, resolution validated
- [ ] Phase 3: allocation patterns, CFAR, metrics
- [ ] Phase 4: optimization (greedy, genetic algorithm)
- [ ] Phase 5: report and presentation

## Planned structure

```
isac-ofdm-radar/
├── src/isac/        # ofdm, channel, radar, allocation, metrics
├── notebooks/       # exploration and figures
├── results/         # generated plots
├── tests/
└── README.md
```

## Getting started

Requirements: Python 3.10+, NumPy, SciPy, Matplotlib.

```bash
git clone https://github.com/<your-username>/isac-ofdm-radar.git
cd isac-ofdm-radar
pip install numpy scipy matplotlib
```

## References

- P. Li, M. Li, R. Liu, Q. Liu, and A. L. Swindlehurst, "Sensing-Oriented Adaptive Resource Allocation Designs for OFDM-ISAC Systems," arXiv:2504.06605, 2025.
- C. Sturm and W. Wiesbeck, "Waveform design and signal processing aspects for fusion of wireless communications and radar sensing," Proc. IEEE, vol. 99, no. 7, 2011.
- F. Liu et al., "Integrated sensing and communications: Towards dual-functional wireless networks for 6G and beyond," IEEE J. Sel. Areas Commun., vol. 40, no. 6, 2022.

## Author

Ali Saljoughi, Electrical Engineering (Communication Systems), IRIB University.

## License

MIT
