
> 如果你想查看更多超参数对结果带来的变动，请参考 《LoRA 和 QLoRA 微调语言大模型：数百次实验后的见解》https://zhuanlan.zhihu.com/p/664912829

# 效果验证

## 命令行

1. 先把微调后的模型转换格式为hf，再与原模型合并，这里提供了统一脚本：

```
HF_OUTPUT_DIR="./hf" # lora转为hf格式后的输出地址
MERGE_OUTPUT_DIR="./merge" # 与原模型合并后的输出地址
SCRIPT_PATH="./internlm2_chat_7b_qlora_oasst1_e3_copy.py" # 训练配置文件
SRC_MODEL_PATH="/home/model_temp/Shanghai_AI_Laboratory/internlm2-chat-7b" # 原模型地址
WEIGHTS_PATH="/home/finetune/work_dirs/internlm2_chat_7b_qlora_oasst1_e3_copy/iter_150.pth" # lora权重地址
SYSTEM_PROMPT="你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福" # 默认system prompt

rm -rf $HF_OUTPUT_DIR
rm -rf $MERGE_OUTPUT_DIR
mkdir -p $HF_OUTPUT_DIR
mkdir -p $MERGE_OUTPUT_DIR

xtuner convert pth_to_hf "${SCRIPT_PATH}" "${WEIGHTS_PATH}" "${HF_OUTPUT_DIR}"
xtuner convert merge \
    "${SRC_MODEL_PATH}" \
    "${HF_OUTPUT_DIR}" \
    "${MERGE_OUTPUT_DIR}" \
    --max-shard-size "2GB"
```

如果这步报错，请检查 `WEIGHTS_PATH` 是否正确。

1. 启动对话：

```
# 如果想要多样性更强加上 --temperature 1
xtuner chat ./merge --prompt-template internlm2_chat --system "你现在是一个送祝福大师，帮我针对不同人和事情、节日送对应的祝福" --temperature 0.7
```

此时你会看到如下显示，只需要输入之前的 `prompt` 和触发风格即可验证。为了去除上下文影响，记得每次输入新提示前需要输入 `RESET` 进行重置。

```
double enter to end input (EXIT: exit chat, RESET: reset history) >>> 祝弟弟工作升职 ,小红书风格

弟弟呀，你的才华如同春日里的花朵，绽放出绚烂的光彩。工作如鱼得水，升职之路顺顺利利，愿你继续闪耀，成为职场的明星！加油哦！🌟🌈<|im_end|>

double enter to end input (EXIT: exit chat, RESET: reset history) >>> RESET

Log: History responses have been removed!

double enter to end input (EXIT: exit chat, RESET: reset history) >>> 我想送哥哥中秋节祝福,小红书风格

🌕🎉 哥，中秋月圆人团圆，愿你事业如月圆，家庭幸福似团圆，快乐满怀，幸福满满！🌟<|im_end|>

double enter to end input (EXIT: exit chat, RESET: reset history) >>> RESET

Log: History responses have been removed!

double enter to end input (EXIT: exit chat, RESET: reset history) >>> 我想送赵老师生日祝福,严肃风格

尊敬的赵老师，值此佳辰，恭祝您福寿安康，事业蒸蒸日上。愿您在未来的岁月里，如同春日之花，绽放出更加绚烂的光彩，为莘莘学子树立起崇高的榜样。愿您的生活如同诗篇般美好，每一天都充满着智慧与喜悦。愿您的工作如同乐章般动听，每一个音符都承载着对教育事业的热爱与执着。在此，衷心祝愿您生日快乐，万事胜意！<|im_end|>
```

## 网页 Demo

为了方便测试，我们还可以使用 `streamlit` 制作一个网页版的demo，方法如下：

1. 先安装下载必要组件和依赖：

```
pip install streamlit==1.24.0

# 找地方clone仓库
git clone https://github.com/InternLM/InternLM.git
```

1. 将 `InternLM/web_demo.py` 中 29 行和 33 行的模型路径更换为上文存放 Merge 后参数的路径 `/home/finetune/merge`

1. 按照如下命令运行 `InternLM` 目录下的 `web_demo.py` 文件即可：

```
streamlit run /root/personal_assistant/code/InternLM/web_demo.py --server.address 127.0.0.1 --server.port 6006
```

通过以上步骤，你就可以成功地微调出最初版本的天机送祝福模块并成功运行推理。

制造数据的质量仍有许多改善空间，如回答随机性注入、输入 `input` 的随机性加强等等。期待大家加入，一起来继续完善数据的制造 pipeline。送祝福完全可以变得更好、更智能、更通用，从而真正更接近中文下的使用场景，让LLM 对话更富有“人情味”！

最后，感谢你的阅读，祝福你有美好的一天。
