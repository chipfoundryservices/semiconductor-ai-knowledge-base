# Mixed Precision Training

**Keywords**: fp8 training, half precision training, bfloat16 fp16 comparison, loss scaling amp, automatic mixed precision fp8

---

**Mixed Precision Training** is **a deep learning training technique that uses lower-precision floating-point formats (FP16, BF16, or FP8) for computation while maintaining FP32 master weights for numerical stability** — delivering up to 3× throughput improvement and 2× memory reduction on modern AI accelerators with minimal impact on model accuracy, and now considered the default training mode for essentially all large-scale deep learning work.

**Why Numerical Precision Matters**

Neural network training involves billions of floating-point multiply-accumulate operations per step. Higher-precision formats (FP32, FP64) represent real numbers with more bits, reducing rounding errors that accumulate across deep networks. However, higher precision comes at a direct throughput cost: NVIDIA H100 delivers 989 TFLOPS FP32 but 3,958 TFLOPS FP8 — a 4× gap that translates directly to training speed.

The fundamental insight of mixed precision training is that different operations have different precision requirements:
- **Weight accumulation** during optimizer updates requires FP32 precision to avoid gradient underflow and weight drift over millions of steps
- **Forward and backward pass computations** tolerate FP16/BF16 with proper loss scaling
- **Very aggressive quantization** (FP8, INT8) works for inference and increasingly for training with modern hardware support

**FP32 vs FP16 vs BF16 vs FP8**

| Format | Total Bits | Exponent Bits | Mantissa Bits | Dynamic Range | Notes |
|--------|-----------|---------------|---------------|---------------|-------|
| FP32 | 32 | 8 | 23 | ±3.4×10^38 | Standard training default (legacy) |
| FP16 | 16 | 5 | 10 | ±6.5×10^4 | Needs loss scaling; overflow risk |
| BF16 | 16 | 8 | 7 | ±3.4×10^38 | Same range as FP32; preferred for training |
| FP8 E4M3 | 8 | 4 | 3 | ±448 | Forward pass optimized |
| FP8 E5M2 | 8 | 5 | 2 | ±57344 | Backward pass optimized |

**BF16 vs FP16 — The Critical Difference**

FP16 has only 5 exponent bits, giving it a much smaller dynamic range than FP32. Gradient values during backpropagation span many orders of magnitude — gradients for early layers in deep networks can be many thousands of times smaller than gradients for final layers. FP16 loses these small gradients entirely (they underflow to zero), which is why FP16 training requires loss scaling.

BF16 trades mantissa precision for exponent range — it has the same 8 exponent bits as FP32, so it never overflows or underflows where FP32 would. On NVIDIA A100/H100 and Google TPUs (which natively support BF16), BF16 is strictly preferable: same dynamic range as FP32, no loss scaling required, 2× memory saving. On older hardware (V100, which supports FP16 but not BF16 natively), FP16 with loss scaling is the only option.

**The Standard Mixed Precision Recipe (AMP)**

The NVIDIA-recommended procedure, implemented by PyTorch's `torch.cuda.amp`:

1. **Maintain FP32 master weights**: The optimizer always stores and updates the authoritative copy of weights in FP32
2. **Cast to FP16/BF16 for compute**: Before each forward pass, weights are cast from FP32 to FP16/BF16. Activations and gradients are computed in half precision on Tensor Cores
3. **Loss scaling** (FP16 only): Multiply the loss by a large constant (e.g., 2^16) before backward pass to shift gradient values into the representable FP16 range. Unscale before the optimizer step
4. **FP32 gradient accumulation**: Gradients from FP16 backward pass are converted back to FP32 and accumulated into FP32 master copies
5. **FP32 optimizer step**: Adam/AdamW updates the FP32 master weights using FP32 gradients

**PyTorch AMP Implementation**

```python
from torch.cuda.amp import autocast, GradScaler

scaler = GradScaler()  # Only needed for FP16; BF16 does not need it

for batch in dataloader:
    with autocast(dtype=torch.bfloat16):  # or torch.float16
        output = model(batch)
        loss = criterion(output, target)
    
    scaler.scale(loss).backward()
    scaler.unscale_(optimizer)
    torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
    scaler.step(optimizer)
    scaler.update()
    optimizer.zero_grad()
```

For BF16, the GradScaler is redundant but kept for API compatibility. Modern code omits it for BF16 training.

**FP8 Training — The Frontier (H100 and Beyond)**

NVIDIA H100 introduced hardware-native FP8 support via the Transformer Engine library. FP8 training follows a more complex protocol:

- **Two FP8 formats**: E4M3 (4 exponent, 3 mantissa — higher precision, used for forward pass activations) and E5M2 (5 exponent, 2 mantissa — higher dynamic range, used for backward pass gradients)
- **Per-tensor scaling**: Since FP8 range is very limited, each tensor needs a scaling factor updated every step (delayed scaling or just-in-time scaling)
- **Current support**: NVIDIA Transformer Engine (used in NeMo, Megatron-LM), DeepSpeed FP8, PyTorch Inductor FP8

FP8 training achieves ~2× throughput over BF16 on H100 for transformer-dominated workloads, with accuracy recovery requiring careful tuning of the scaling factor update frequency.

**Memory and Throughput Gains**

For a 7B parameter model trained on H100:

| Configuration | Model Memory | Activation Memory | Throughput |
|--------------|-------------|-------------------|-----------|
| FP32 full | 28 GB | ~40 GB | 1× baseline |
| AMP BF16 | 14 GB weights + 28 GB master | ~20 GB | ~2.5× |
| FP8 training | 7 GB weights + 28 GB master | ~10 GB | ~4× |

The FP32 master weights persist throughout training regardless of compute precision — this is a fixed 4 bytes/parameter cost that cannot be eliminated without sacrificing training stability.

**Integration with Distributed Training**

Mixed precision interacts with all major distributed training frameworks:
- **DeepSpeed ZeRO**: ZeRO-3 shards FP32 master weights across GPUs, so the per-GPU FP32 memory cost scales down with GPU count. ZeRO-3 + BF16 is the standard recipe for 70B+ models
- **PyTorch FSDP**: Full Sharded Data Parallel shards both FP32 and BF16 copies across devices
- **Tensor parallelism**: Megatron-LM and NeMo handle mixed precision correctly across tensor-parallel ranks

Mixed precision is not optional at scale — training GPT-4 class models purely in FP32 would require 4× more GPU-hours and 2× more GPU memory, adding tens of millions of dollars to the pre-training budget.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/fp8-training) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
