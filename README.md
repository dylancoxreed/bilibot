
```
python -m mlx_lm.lora --model models/Qwen1.5-32B-Chat --data data/ --train --iters 1000 --batch-size 16 --lora-layers 12
```

将微调后的 `adapters` 文件与基础模型合并：

```
python -m mlx_lm.fuse --model models/Qwen1.5-32B-Chat --save-path models/Qwen1.5-32B-Chat-FT --adapter-path models/Qwen1.5-32B-Chat-Adapters
```

对合并后的模型进行量化加速：
python tools/compress_model.py

对微调训练后的模型进行对话测试：
python chat.py

### 语音生成
本项目借助开源项目 [GPT-SoVITS](https://github.com/RVC-Boss/GPT-SoVITS) 进行语音生成。

首先参考 [GPT-SoVITS](https://github.com/RVC-Boss/GPT-SoVITS) 的官方指南配置环境并运行语音生成程序。

```
conda create -n GPTSOVITS python=3.9
conda activate GPTSOVITS
cd GPT-SoVITS
pip install -r requirements.txt
python webui.py
```

运行 api 程序，分别使用端口 9880 与 9881 提供派蒙与林亦的语音生成服务，以下请使用 GPT-SoVITS 代码库完成：
```
python api.py -s SoVITS_weights/paimeng2_e110_s159940.pth -g GPT_weights/paimeng2-e10.ckpt -dr samples/Paimon/疑问—哇，这个，还有这个…只是和史莱姆打了一场，就有这么多结论吗？.wav -dt "哇，这个，还有这个…只是和史莱姆打了一场，就有这么多结论吗？" -dl "zh" -a 127.0.0.1 -p 9880
python api.py -s SoVITS_weights/linyi_e25_s1150.pth -g GPT_weights/linyi-e50.ckpt -dr "samples/linyi/【愤怒】你这问题太弱智了，我都不知道该从哪开始骂你。.WAV" -dt "你这问题太弱智了，我都不知道该从哪开始骂你。" -dl "zh" -a 127.0.0.1 -p 9881
```

运行问答生成程序：
```
python start_qa_dialogue.py
```

## 参考

1. 机器学习框架 MLX，来自苹果机器学习研究组：https://github.com/ml-explore/mlx
2. 阿里通义千问 Qwen1.5：https://qwenlm.github.io/zh/blog/qwen1.5/
3. 开源文本转语音项目 GPT-SoVITS，作者[花儿不哭](https://space.bilibili.com/5760446)：https://github.com/RVC-Boss/GPT-SoVITS
4. 派蒙语音模型，作者[白菜工厂1145号员工](https://space.bilibili.com/518098961)：[【GPT-SoVITS】30小时超大数据集测试，堆时长真的有用吗？](https://www.bilibili.com/video/BV1Yu4m1N79m)
