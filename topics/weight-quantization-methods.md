# Weight Quantization Methods

**Keywords**: weight quantization methods,quantization schemes neural networks,symmetric asymmetric quantization,per channel quantization,quantization calibration

---

**Weight Quantization Methods** are **the precision reduction techniques that map high-precision floating-point weights to low-bitwidth integer or fixed-point representations — using symmetric or asymmetric scaling, per-tensor or per-channel granularity, and various calibration strategies to minimize quantization error while achieving 2-8× memory reduction and enabling efficient integer arithmetic on specialized hardware**.

**Quantization Schemes:**
- **Uniform Affine Quantization**: maps float x to integer q via q = round(x/scale + zero_point); dequantization: x ≈ scale · (q - zero_point); scale and zero_point are calibration parameters determined from weight statistics; most common scheme due to hardware support
- **Symmetric Quantization**: constrains zero_point = 0, so q = round(x/scale); simpler hardware implementation (no zero-point subtraction); scale = max(|x|) / (2^(bits-1) - 1); suitable for symmetric distributions (weights after BatchNorm)
- **Asymmetric Quantization**: allows non-zero zero_point; scale = (max(x) - min(x)) / (2^bits - 1), zero_point = round(-min(x)/scale); better for skewed distributions (ReLU activations are always non-negative); requires additional zero-point arithmetic
- **Power-of-Two Scaling**: restricts scale to powers of 2; enables bit-shift operations instead of multiplication; scale = 2^(-n) for integer n; slightly less accurate than arbitrary scale but much faster on hardware without multipliers

**Granularity Levels:**
- **Per-Tensor Quantization**: single scale and zero_point for entire weight tensor; simplest approach with minimal overhead; sufficient for activations but often too coarse for weights (different channels have different ranges)
- **Per-Channel Quantization**: separate scale and zero_point for each output channel; captures variation in weight magnitudes across channels; critical for maintaining accuracy in convolutional and linear layers; standard in TensorRT, ONNX Runtime
- **Per-Group Quantization**: divides channels into groups, quantizes each group independently; interpolates between per-tensor (1 group) and per-channel (C groups); used in LLM quantization (GPTQ, AWQ) with groups of 32-128 weights
- **Per-Token/Per-Row Quantization**: for activations in Transformers, quantize each token independently; handles outlier tokens that would dominate per-tensor statistics; SmoothQuant uses per-token quantization for activations

**Calibration Methods:**
- **MinMax Calibration**: scale = (max - min) / (2^bits - 1); simple but sensitive to outliers; a single extreme value can waste quantization range; suitable for well-behaved distributions without outliers
- **Percentile Calibration**: uses 99.9th or 99.99th percentile instead of absolute max; clips outliers to improve quantization range utilization; percentile threshold is hyperparameter (higher = more outliers preserved, lower = better range utilization)
- **MSE Minimization (TensorRT)**: searches for scale that minimizes mean squared error between original and quantized values; iterates over candidate scales, computes MSE, selects best; more accurate than MinMax but computationally expensive
- **Cross-Entropy Calibration**: minimizes KL divergence between original and quantized activation distributions; preserves statistical properties of activations; used in TensorRT for activation quantization
- **GPTQ (Hessian-Based)**: uses second-order information (Hessian) to quantize weights; quantizes weights column-by-column while compensating for quantization error in remaining columns; enables INT4 weight quantization of LLMs with <1% perplexity increase

**Advanced Quantization Techniques:**
- **Mixed-Precision Quantization**: different layers use different bitwidths based on sensitivity; first/last layers often kept at INT8 or FP16; middle layers use INT4 or INT2; automated search (HAQ, HAWQ) finds optimal per-layer bitwidth allocation
- **Outlier-Aware Quantization**: identifies and handles outlier weights/activations separately; LLM.int8() keeps outliers in FP16 while quantizing rest to INT8; <0.1% of weights are outliers but they dominate quantization error
- **SmoothQuant**: migrates quantization difficulty from activations to weights by scaling; multiplies weights by s and activations by 1/s where s is chosen to balance their quantization difficulty; enables INT8 inference for LLMs with minimal accuracy loss
- **AWQ (Activation-Aware Weight Quantization)**: scales salient weight channels (identified by activation magnitudes) before quantization; protects important weights from quantization error; achieves better INT4 quantization than uniform rounding

**Quantization-Aware Training (QAT) Techniques:**
- **Fake Quantization**: inserts quantize-dequantize operations during training; forward pass uses quantized values, backward pass uses straight-through estimator (STE) for gradient; model learns to be robust to quantization error
- **Learned Step Size Quantization (LSQ)**: learns quantization scale via gradient descent; scale becomes a trainable parameter; gradient: ∂L/∂scale = ∂L/∂q · ∂q/∂scale where ∂q/∂scale is approximated by STE
- **Differentiable Quantization (DQ)**: replaces hard rounding with soft differentiable approximation; uses sigmoid or tanh to approximate round function; gradually sharpens approximation during training
- **Quantization Noise Injection**: adds noise during training to simulate quantization error; noise magnitude matches expected quantization error; simpler than fake quantization but less accurate

**Hardware-Specific Quantization:**
- **INT8 Tensor Cores (NVIDIA)**: requires specific data layout and alignment; TensorRT automatically handles layout transformation; achieves 2× throughput over FP16 on A100/H100
- **INT4 Quantization (Qualcomm, Apple)**: specialized hardware for INT4 compute; weights stored as INT4, activations often INT8 or INT16; enables 4× memory reduction and 2-4× speedup
- **Binary/Ternary Quantization**: extreme quantization to {-1, +1} or {-1, 0, +1}; enables XNOR operations instead of multiplication; 32× memory reduction but significant accuracy loss (5-10%); practical only for specific applications
- **NormalFloat (NF4)**: information-theoretically optimal 4-bit format for normally distributed weights; used in QLoRA; quantization bins are non-uniform, denser near zero; better than uniform INT4 for LLM weights

**Practical Considerations:**
- **Calibration Data**: 100-1000 samples typically sufficient for PTQ calibration; should be representative of deployment distribution; more data doesn't always help (diminishing returns beyond 1000 samples)
- **Accuracy Recovery**: INT8 quantization typically <1% accuracy loss; INT4 requires careful calibration or QAT, 1-3% loss; INT2 often requires QAT and accepts 3-5% loss
- **Inference Frameworks**: TensorRT, ONNX Runtime, OpenVINO provide optimized INT8 kernels; llama.cpp, GPTQ, AWQ provide INT4 LLM inference; framework support is critical for realizing speedups

Weight quantization methods are **the bridge between high-precision training and efficient deployment — enabling models trained in FP32 or BF16 to run in INT8 or INT4 with minimal accuracy loss, making the difference between a model that requires a datacenter and one that runs on a smartphone**.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/weight-quantization-methods) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
