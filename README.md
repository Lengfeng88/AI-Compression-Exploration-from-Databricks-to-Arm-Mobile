# On-Device AI Compression Exploration on Databricks Free Edition

##  Project Overview
This project explores **four model compression techniques**—Knowledge Distillation, Quantization-Aware Training (QAT), Neural Architecture Search (NAS), and Structured Pruning—to enable real-time computer vision on **ultra-low-end Arm phones**, such as the **vivo Y81s (Cortex-A53, 3GB RAM)**.

Built entirely on **Databricks Free Edition**, we demonstrate a full MLOps pipeline:
- Train a ResNet-18 teacher model
- Apply 4 compression strategies
- Evaluate CPU latency, accuracy, and model size
- Export the best candidate (Pruning) to ONNX for mobile deployment

> **Note**: We attempted on-device inference on the vivo Y81s, but **hardware limitations** (lack of NEON FP16/INT8 support in ONNX Runtime) prevented successful execution. However, the model is valid and ready for deployment on more capable Arm devices.

##  Results Summary (Evaluated on Databricks CPU)

| Method       | Accuracy | Latency (CPU) | Model Size | ONNX Exported |
|--------------|----------|---------------|------------|----------------|
| Distillation | 64.90%   | 1408 ms       | 8.76 MB    | ✅ |
| QAT          | 68.72%   | 80.28 ms      | 9.49 MB    | ✅ |
| NAS          | 66.87%   | 1338 ms       | 5.96 MB    | ✅ |
| **Pruning**  | **69.57%** | **26.71 ms**  | **8.76 MB** | ✅ (**Best**) |

✅ **Key Insight**: Structured pruning achieves the best trade-off—**69.6% accuracy at just 26.7 ms latency (37 FPS)**—making it the ideal candidate for on-device AI.

## 🛠️ Lakehouse & Limitations
We attempted to use **Delta Lake** for structured result storage and versioning. However, **Databricks Free Edition** lacks Unity Catalog and cannot create the model_benchmarks, preventing automated Dashboard generation.

As a workaround, we **manually compiled results** and created a static visualization—demonstrating real-world adaptability in constrained environments.

##  Target Device: vivo Y81s
- **SoC**: MediaTek MT6762 (8x Cortex-A53 @ 2.0 GHz)
- **RAM**: 3 GB
- **Why it matters**: This represents **billions of real-world low-end devices**. While ONNX Runtime failed to execute due to missing CPU acceleration, our work proves that **model optimization must match device capability**.

##  Screenshots 
*All four compression experiments tracked in MLflow.*
*Latency vs Accuracy trade-off (Delta Lake Dashboard).*
*Converted compressed model imported into the Android mobile phone.*

##  Demo Video
[[![Watch the demo]([https://www.youtube.com/shorts/FJAhRthFuZs])](https://youtube.com/shorts/FJAhRthFuZs)
