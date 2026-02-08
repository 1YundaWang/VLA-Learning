# 🚀 starVLA Learning

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
<video src="/mnt/16T/wangyunda/starVLA/results/libero_spatial/Qwen2.5-VL-GR00T-LIBERO-4in1_checkpoints_steps_30000_pytorch_model.pt/rollout_pick_up_the_black_bowl_between_the_plate_and_the_ramekin_and_place_it_on_the_plate_episode0_success.mp4" controls width="100%"></video>

