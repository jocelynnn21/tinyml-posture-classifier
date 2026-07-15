# Edge Impulse Project

This folder documents the main deployment pipeline, which ran through Edge Impulse Studio rather than raw code in this repo.

- **Public project:** https://studio.edgeimpulse.com/public/1014920/live
- **Dataset:** 40 samples, ~6 min 41 s total, collected via the Edge Impulse Data Forwarder with the board mounted on the upper back between the shoulder blades
- **Impulse design:** time-series input (window size ~1 s / 50 samples @ 50 Hz, window increase ~100 ms) + spectral analysis + classifier (2 dense layers, 20 neurons each, softmax output)
- **Train/test split:** ~78% / 23% (project setup described as 80/20)

## deployment/
Put the exported Arduino library .zip (or its unzipped contents) from Edge Impulse's "Deployment" tab here — this is the package that gets flashed to the Arduino Nano 33 BLE Sense.

## Deployed model results

| Model | Accuracy | Flash | RAM | Latency |
|---|---|---|---|---|
| Edge Impulse float32 (unoptimized) | 100% | 35.9K | 2.6K | 20 ms |
| **Edge Impulse int8 (deployed)** | **100%** | **20.4K** | **2.1K** | **19 ms estimate** |

Measured live CLI timing: DSP ~20–24 ms, inference ~7 ms, total ~31 ms per classification (well under the 100 ms target).
