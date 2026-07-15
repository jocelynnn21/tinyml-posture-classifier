# Exported Models

TFLite models exported from the compression notebook (`../notebook/`). These are the compression-study models, distinct from the Edge Impulse int8 model that was actually flashed to the Arduino (that one lives inside the Edge Impulse deployment package, `../edge-impulse/deployment/`).

| File | Method | Size | Test Accuracy |
|---|---|---|---|
| `model_float32.tflite` | No compression (reference) | 6.8 KB | 100% |
| `model_ptq_int8.tflite` | Post-training quantization | 4.6 KB | 100% |
| `model_qat_int8.tflite` | Quantization-aware training | 4.1 KB | 100% |
| `model_pruned_ptq.tflite` | 50% pruning + PTQ | 4.7 KB | 100% (48.3% sparsity) |

QAT produced the best size/accuracy tradeoff of the four. Pruning's sparsity didn't translate to the smallest file — sparse storage overhead outweighs the benefit at this model size.
