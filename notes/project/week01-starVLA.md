<!-- # 🚀 starVLA Learning

## 📖 Github Link
https://github.com/starVLA/starVLA

## 🎯 Learning Goals
- [ ] 部署项目
- [ ] 跑通libero快速测评
...

### ✅ Completed
- [√] Environment setup and StarVLA deployment
- [√] 跑通libero_spatial测评

## 🛠️ 困难与解决方案
1. 报错：ValueError: FlashAttention2 has been toggled on, but it cannot be used due to the following error: Flash Attention 2 is not available on CPU. Please make sure torch can access a CUDA device.
可能是accelerate与torch环境不配套，下面是一套可以跑通代码的环境。

pip install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 --index-url https://download.pytorch.org/whl/cu121
检验：
python -c "import torch; print('CUDA可用:' , torch.cuda.is_available()); print('当前版本:', torch.version.cuda); print('设备名称:', torch.cuda.get_device_name(0) if torch.cuda.is_available() else '无')"

pip install https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl
检验：python -c "import torch; from flash_attn import flash_attn_func; print('FlashAttention 验证成功！')"

grep -vE "torch|torchvision" requirements.txt > requirements_new.txt    # 1. 过滤到新文件
pip install -r requirements_new.txt    # 2. 安装过滤后的依赖
检验：
python -c "from accelerate import Accelerator; print(Accelerator().device)"
accelerate test

pip install -e .

2. 报错：RuntimeError: Cannot access accelerator device when none is available.
忘记修改run_policy_server.sh中的gpu_id了。根据自己服务器上希望用的gpu编号去修改即可。

3. run_policy_server.sh环境修改
export gpu_id=0    # 使用第0号GPU
export star_vla_python=/mnt/16T/wangyunda/miniconda3/envs/starVLA/bin/python    # 设置starVLA环境
your_ckpt=/mnt/16T/wangyunda/starVLA/Qwen2.5-VL-GR00T-LIBERO-4in1/checkpoints/steps_30000_pytorch_model.pt    #模型权重路径

## 🔬 Experiment Results
视频文件存储在 ~/starVLA/results/libero_spatial中，训练日志存储在 ~/starVLA/logs/20260207_233945/output.log中。
<video src="/mnt/16T/wangyunda/starVLA/results/libero_spatial/Qwen2.5-VL-GR00T-LIBERO-4in1_checkpoints_steps_30000_pytorch_model.pt/rollout_pick_up_the_black_bowl_between_the_plate_and_the_ramekin_and_place_it_on_the_plate_episode0_success.mp4" controls width="100%"></video> -->

# 🚀 starVLA Learning Report

> **项目仓库**: [🔗 starVLA/starVLA](https://github.com/starVLA/starVLA)  
> **记录时间**: 2026-02-08

---

## 🎯 学习目标 (Learning Goals)

- [x] **环境部署**：完成基础环境搭建与 StarVLA 部署
- [x] **快速测评**：跑通 `libero_spatial` 测评流程
- [ ] **深度研究**：分析不同 Checkpoints 在不同任务下的表现
- [ ] **自定义训练**：尝试微调模型参数

---

## 🛠️ 困难与解决方案 (Troubleshooting)

### 1. FlashAttention 2 与设备不匹配
> **报错信息**：`ValueError: FlashAttention2 has been toggled on, but it cannot be used due to the following error: Flash Attention 2 is not available on CPU.`

**💡 根本原因**：`accelerate` 库无法识别 GPU，或 `torch` 与 `flash-attention` 版本不兼容。

**✅ 解决方案（经验证的稳定环境）**：

1. **重新安装核心驱动与 Torch**：
   ```bash
   pip install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 --index-url [https://download.pytorch.org/whl/cu121](https://download.pytorch.org/whl/cu121)

```

2. **安装适配的 FlashAttention 二进制包**：
```bash
pip install [https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl](https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl)

```


3. **过滤并安装其余依赖**：
```bash
# 过滤掉已手动安装的 torch 依赖，避免冲突
grep -vE "torch|torchvision" requirements.txt > requirements_new.txt
pip install -r requirements_new.txt
pip install -e .

```



**🔍 环境验证表**：

| 检查项 | 验证命令 | 预期输出 |
| --- | --- | --- |
| **CUDA 可用性** | `python -c "import torch; print(torch.cuda.is_available())"` | `True` |
| **FlashAttn** | `python -c "from flash_attn import flash_attn_func"` | `(无报错)` |
| **Accelerate** | `accelerate test` | `Passed` |

---

### 2. 显卡设备访问失败 (RuntimeError)

> **报错信息**：`RuntimeError: Cannot access accelerator device when none is available.`

**📍 修正方法**：检查并修改 `run_policy_server.sh` 中的环境变量：

* **GPU ID**: 确保 `export gpu_id=0` 与当前服务器空闲显卡编号一致。
* **路径检查**: 确保 `your_ckpt` 指向了正确的 `.pt` 文件绝对路径。

```bash
# 修改示例
export gpu_id=0 
export star_vla_python=/mnt/16T/wangyunda/miniconda3/envs/starVLA/bin/python 
your_ckpt=/mnt/16T/wangyunda/starVLA/Qwen2.5-VL-GR00T-LIBERO-4in1/checkpoints/steps_30000_pytorch_model.pt

```

---

## 🔬 实验结果展示 (Experiment Results)

### 📊 文件路径说明

* **视频存储**: `~/starVLA/results/libero_spatial`
* **训练日志**: `~/starVLA/logs/20260207_233945/output.log`

### 📹 Rollout 演示

**任务描述**：*Pick up the black bowl between the plate and the ramekin and place it on the plate*

<div align="center">
<table style="border: none; background-color: transparent;">
<tr>
<td align="center" style="border: none;">
<video src="VLA-Learning/notes/project/rollout_pick_up_the_black_bowl_between_the_plate_and_the_ramekin_and_place_it_on_the_plate_episode0_success.mp4"
controls
width="100%"
style="border-radius: 10px; box-shadow: 0 4px 12px rgba(0,0,0,0.3);">
您的浏览器不支持 HTML5 视频播放。
</video>
<p style="margin-top: 10px; color: #666;">
<b>🎥 场景演示：抓取黑碗并精准放置 (Episode 0 - Success)</b>
</p>
</td>
</tr>
</table>
</div>

---

```



