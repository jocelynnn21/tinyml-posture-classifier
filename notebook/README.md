# Compression Notebook

This folder holds the Python/Colab notebook used for the advanced component: cascading model compression.

**Suggested file:** `posture_compression.ipynb`

## What it does
1. Loads exported Edge Impulse CBOR files
2. Parses labels and IMU values
3. Applies sliding-window feature extraction (~1 s windows @ 50 Hz)
4. Standardizes features
5. Splits into train/test
6. Trains the baseline dense network (TensorFlow/Keras, categorical crossentropy, Adam, lr=0.0005, batch size 32, 50 epochs)
7. Converts to TFLite float32
8. Applies PTQ int8
9. Applies QAT int8
10. Applies pruning (50%) + PTQ
11. Compares accuracy, latency, and model size across all four
12. Exports the four `.tflite` files to `../models/`

## Results produced by this notebook

| Model | Size | Test Accuracy | Colab CPU Latency |
|---|---|---|---|
| Float32 baseline | 6.8 KB | 100% | 0.003 ms |
| PTQ int8 | 4.6 KB | 100% | 0.002 ms |
| QAT int8 | 4.1 KB | 100% | 0.002 ms |
| Pruning 50% + PTQ | 4.7 KB | 100% | 0.002 ms |

Note: this notebook was a **supporting compression study**, not the main deployment pipeline. Edge Impulse remained the primary workflow for training, testing, and Arduino deployment.
