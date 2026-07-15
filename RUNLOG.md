# RUNLOG

## Run 1 - Baseline

### Hypothesis
Establish a baseline before making any modifications.

### Changes
- Ran the provided starter code without architectural changes.
- Baseline optimizer and training configuration.

### Result
- Dev bpb: **2.5101**

### Conclusion
The baseline provided a reference point. There was significant room for improvement through architecture and training optimization.

---

## Run 2 - Larger Transformer

### Hypothesis
Increasing model capacity (more layers, larger context length, larger hidden size and dropout) may improve language modeling performance.

### Changes
- Increased block size.
- Increased model depth and width.
- Added dropout.

### Result
- Dev bpb: **2.6433**

### Conclusion
Performance degraded despite lower training loss. Under the fixed 2000-step training budget, the larger model did not converge sufficiently and generalized worse than the baseline.

---

## Run 3 - SwiGLU Architecture

### Hypothesis
Replacing the standard feed-forward network with SwiGLU while keeping model size similar should improve representation quality without increasing parameters significantly.

### Changes
- Replaced MLP with SwiGLU.
- AdamW optimizer.
- Cosine learning rate schedule.
- 200-step warmup.
- Gradient clipping (1.0).
- Weight decay = 0.05.
- Kept model under the 2M parameter limit.

### Result
- Dev bpb: **2.2612**

### Conclusion
This produced the best result. Architectural improvement of the feed-forward block was more effective than simply increasing model capacity.

---

## Run 4 - Xavier Initialization + Optimizer Parameter Groups

### Hypothesis
Improved initialization and excluding LayerNorm/bias parameters from weight decay may improve optimization.

### Changes
- Xavier initialization.
- AdamW parameter groups.
- Learning rate tuned.

### Result
- Dev bpb: **2.43**

### Conclusion
Performance regressed compared to the SwiGLU configuration. The previous initialization and optimizer setup were retained for the final submission.

---

# Final Configuration

- Architecture: Transformer with SwiGLU feed-forward blocks.
- Parameters: **1,340,864**
- Optimizer: AdamW
- Learning rate: 3e-4
- Weight decay: 0.05
- Warmup: 200 steps
- Scheduler: Cosine decay
- Gradient clipping: 1.0
- Final dev bpb: **2.2612**