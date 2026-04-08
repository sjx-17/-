# VisualPatchCompressor 交接说明


这份代码库用于交接一次未完成的实验性修改：尝试把 `VisualPatchCompressor` 接入 `starVLA` 的 `QwenGR00T` 框架中。


## 本次修改的位置

核心改动集中在：

- `starVLA/model/framework/QwenGR00T.py`

## 我做过的尝试

在 `QwenGR00T.py` 中做了以下改动：

1. 新增了 `VisualPatchCompressor` 模块。
2. 在 `Qwen_GR00T.__init__()` 中实例化了压缩器。
3. 在 `forward()` 中，先获取 `Qwen-VL` 最后一层隐藏状态，再送入 `VisualPatchCompressor` 压缩后交给 `action_model`。
4. 在 `predict_action()` 中同步加入了相同的压缩逻辑，保证训练和推理路径一致。
5. 保留了简要中文注释

## 修改意图

当时的想法是：

- `Qwen-VL` 输出的 token 序列较长；
- 在进入 `GR00T action head` 之前，尝试先用可学习的 memory tokens 做一次 cross-attention 压缩；
- 希望把长序列压缩成固定数量 token，再交给后续 action head 处理。

本次尝试中使用的是：

- `num_memory_tokens = 64`

这个值是实验时直接写在代码里的，没有继续做成配置项。

## 当前状态

这次修改已经写进了代码，但**没有完成完整实验验证**。

当时运行时遇到的实际情况是：

- 模型在跑的时候出现了**显存不足**；
- 因此没有继续完成后续训练或推理验证；
- 当前仓库中的这部分修改应视为“已接线但未完成验证”的尝试性代码。


