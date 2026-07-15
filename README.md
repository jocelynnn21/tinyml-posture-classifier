# TinyML Posture Classifier

A real-time binary posture classifier (**good posture** vs. **slouch**) built for the Arduino Nano 33 BLE Sense as part of EE 446: TinyML for Ultra Low-Power Edge Computing, Spring 2026.

The system reads onboard IMU data (accelerometer + gyroscope) at 50 Hz from a board mounted on the user's upper back, runs on-device inference through Edge Impulse, and prints live posture predictions through the Edge Impulse CLI.

**Team:** Jocelyn He, Pandha Panthawangkun

---

## Results Summary

| Metric | Value |
|---|---|
| Test accuracy (deployed model) | 100% |
| Flash usage | 20.4K |
| RAM usage | 2.1K |
| Estimated latency | 19 ms |
| Measured end-to-end latency (DSP + inference) | ~31 ms |
| Classes | good, slouch |
| Sensor | 6-axis IMU (accX, accY, accZ, gyrX, gyrY, gyrZ) @ 50 Hz |

### Compression comparison (Colab notebook)

| Model | Size | Accuracy | Notes |
|---|---|---|---|
| Float32 baseline | 6.8 KB | 100% | Reference model |
| PTQ int8 | 4.6 KB | 100% | 32% smaller |
| **QAT int8** | **4.1 KB** | 100% | Smallest, 40% smaller — best tradeoff |
| Pruning 50% + PTQ | 4.7 KB | 100% | 48.3% sparsity, but not the smallest file |

QAT gave the best size/accuracy tradeoff. Pruning did not translate into the smallest file size — sparse storage overhead doesn't pay off at this model scale.

**Note:** BLE wireless output was attempted but not completed due to Bluetooth library issues. The final system uses Edge Impulse CLI terminal output instead. This is documented here for transparency, not included as a working feature.

---

## Repository Structure

```
tinyml-posture-classifier/
├── README.md                     ← you are here
├── notebook/                     ← Python compression study (Colab)
│   ├── posture_compression.ipynb
│   └── README.md
├── edge-impulse/                 ← Edge Impulse project artifacts + deployment package
│   ├── deployment/               ← exported Arduino library/firmware from Edge Impulse
│   └── README.md
├── arduino/                      ← Arduino sketch(es) for running inference on-device
│   └── README.md
├── models/                       ← exported TFLite models from the compression study
│   ├── model_float32.tflite
│   ├── model_ptq_int8.tflite
│   ├── model_qat_int8.tflite
│   ├── model_pruned_ptq.tflite
│   └── README.md
└── docs/
    ├── screenshots/              ← CLI output screenshots (live good/slouch predictions)
    ├── figures/                  ← board mounting photo, feature explorer plots, etc.
    └── report.pdf                ← final project report (optional, if you want it in-repo)
```

---

## Reproducing This Project

1. Collect IMU data using the Edge Impulse Data Forwarder CLI (`edge-impulse-data-forwarder`), mounting the board on the upper back between the shoulder blades.
2. Label data as `good` or `slouch` in Edge Impulse Studio.
3. Create the impulse: time-series input window (~1 s / 50 samples, ~100 ms window increase) + spectral analysis + classifier block.
4. Train the baseline dense neural network (2×20 neurons, softmax output) in Edge Impulse.
5. Quantize to int8 for Arduino deployment.
6. Run the Colab compression notebook (`notebook/posture_compression.ipynb`) to reproduce the float32 / PTQ / QAT / pruning comparison.
7. Deploy the Edge Impulse int8 model to the Arduino Nano 33 BLE Sense.
8. Verify live predictions with `edge-impulse-run-impulse`.

### Notebook dependencies
- TensorFlow
- TensorFlow Model Optimization Toolkit (`tfmot`)
- NumPy
- Pandas
- scikit-learn
- cbor2

---

## Links

- **Edge Impulse public project:** https://studio.edgeimpulse.com/public/1014920/live

---

## Hardware

- Arduino Nano 33 BLE Sense (onboard LSM9DS1 IMU)
- Edge Impulse firmware + CLI

## Limitations

- Dataset was self-collected and relatively small (40 samples, ~6 min 41 s total); generalization to other users/chairs/placements is untested.
- Only two posture states are classified (good, slouch) — no support for leaning, fidgeting, standing, or empty-chair detection.
- Colab latency figures were measured on Colab CPU, not on-device; Edge Impulse/CLI timings are the ones relevant to actual deployment.
- BLE output and OLED display were not completed in the final system.
- Button implementation is in future work.
