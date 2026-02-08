# Experiment XXX: [实验名称]

## 📋 基本信息

- **实验ID:** exp_XXX
- **日期:** YYYY-MM-DD
- **状态:** 🔄 进行中 / ✅ 完成 / ❌ 失败
- **负责人:** 你的名字

## 🎯 实验目标
简要描述这个实验想要验证什么假设或解决什么问题

## 💡 核心想法
详细解释你的方法/改进

## 🔧 实验设置

### 硬件环境
- GPU: 4x NVIDIA A100 40GB
- CPU: ...
- 内存: ...

### 软件环境
- PyTorch版本: 2.0.1
- CUDA版本: 11.8
- 其他依赖: ...

### 数据集
- 训练集: Bridge V2 (10k episodes)
- 验证集: LIBERO-Spatial (500 episodes)
- 测试集: ...

### 模型配置
```yaml
# 粘贴你的config.yaml
model:
  framework: QwenOFT
  base_vlm: Qwen2.5-VL-3B
  ...
```

### 训练参数
- Learning rate: 1e-4
- Batch size: 16
- Training steps: 5000
- Optimizer: AdamW

## 📊 实验结果

### 定量结果

| 指标 | Baseline | Ours | 提升 |
|-----|----------|------|------|
| Success Rate |  |  |  |
| Action L2 Error |  |  |  |
| Inference FPS |  |  |  |

### 定性分析

- **成功案例:** [描述或附图]
- **失败案例:** [分析原因]

### 可视化

![训练曲线](results/training_curve.png)
![注意力可视化](results/attention_map.png)

## 🔍 分析与讨论

### 为什么有效？
- 原因1: ...
- 原因2: ...

### 意外发现
- 发现1: ...

### 局限性
- 局限1: ...

## 📝 结论

1-2句话总结实验结论

## 🚀 下一步

- [ ] 尝试在更大数据集上验证
- [ ] 进行ablation study
- [ ] 优化推理速度

## 📎 附件

- 代码: `code/03-starvla-experiments/exp_XXX/`
- 完整日志: `experiments/exp_XXX/logs/`
- Checkpoint: [链接到云存储]
- WandB: [链接]