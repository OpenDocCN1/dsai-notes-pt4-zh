# SELA

> 来源：[https://deepwisdom.feishu.cn/docx/GfdJd6ZPOoH2E2xsPFHcDVhun5e](https://deepwisdom.feishu.cn/docx/GfdJd6ZPOoH2E2xsPFHcDVhun5e)

论文：[《](https://arxiv.org/abs/2410.17238)SELA: Tree-Search Enhanced LLM Agents for Automated Machine Learning》

SELA 是一个创新系统，它通过将蒙特卡洛树搜索（MCTS）与基于大语言模型（LLM）的智能体相结合，增强了自动机器学习（AutoML）的能力。传统的 AutoML 方法往往生成多样性较低且不够优化的代码，这限制了它们在模型选择和集成方面的效果。SELA 通过将流水线配置表示为树结构来解决这些挑战，使智能体能够智能地探索解决方案空间，并基于实验反馈迭代优化其策略。

## 数据准备

数据集获取方式有两种选择：

1.  直接下载数据集：

+   访问数据集[链接](https://drive.google.com/drive/folders/151FIZoLygkRfeJgSI9fNMiLsixh1mK0r)下载

+   如果链接访问不成功，直接从选择下方对应数据集下载，然后解压到你自己的目录

+

![](img/914c613084481c66de4ccd36ea651096.png)

[smoker-status-20241112T061548Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CGsMb4hrqoKHHKxkkOrcKUGgnSd)

+

![](img/f72eecd2eac332382e1cb25cebbfd022.png)

[software-defects-20241112T061549Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JKMKbXuStobGdwxxbRkcijftnUg)

+

![](img/604bb92ba766b867acd5f5e1ae52be03.png)

[segment-20241112T061546Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CEjzbFBvKoYtDexDPygcDLNvnZd)

+

![](img/32003001055c0a49650c219ff80ac540.png)

[wine-quality-white-20241112T061551Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PRP8bxry6oP7OcxLSLkctssDnTg)

+

![](img/c75ebaa89344ad9524b8c0059bf66249.png)

[mfeat-factors-20241112T061542Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IWqSb0KZWoeqmqxhbu6c3Aednwh)

+

![](img/458a318fe8e44c107ec5ca5b52b16863.png)

[kick-20241112T061541Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WuFLbD5O0oz5dexLzqEcQ87LnUf)

+

![](img/abd7738b28196ca833bdabc865375610.png)

[SAT11-HAND-runtime-regression-20241112T061545Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QhUPbo412oxJonxB0hIcovZ1nSH)

+

![](img/ff6fa51bfd7806e70c552191012b625c.png)

[GesturePhaseSegmentationProcessed-20241112T061537Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Eqtcb2xtiojLLCx532xcEZZLnjw)

+

![](img/0ceebe1d4e11c33053310c65ab7f5302.png)

[Moneyball-20241112T061544Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/J1nIbXkkPoGj5OxdABKcwyGnneb)

+

![](img/bf2c0c1410f82c66f534356ef8b5ba07.png)

[kc1-20241112T061539Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GVHzbjyihoGaXFxFAtCc2Cj3nfh)

+

![](img/400c3fbffd486f3b09188fb7909334f8.png)

[jasmine-20241112T061538Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ORp4bVGPLomzeXxMvwFcp6uRnHd)

+

![](img/2bc1bdd048038730bcfc6f7bb94e0a63.png)

[concrete-strength-20241112T061532Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DC34bwtzkoaTRwxCiYAcrHvTnDg)

+

![](img/7479d9705872a33b697dd731c219917f.png)

[diamonds-20241112T061535Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PyvCbNNfgoU6xzxhgz9caZTnnuc)

+

![](img/14514eced3d03a15a0a0ddfe9b5d9902.png)

[credit-g-20241112T061534Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WT6fb4XLpof8wJxzZ93cFnWmnYd)

+

![](img/7cca7d6a36ff0f966bf6fef6074996f0.png)

[colleges-20241112T061531Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FkGfbEAOco6roUxVnvocQTEgnVg)

+

![](img/08f8fe908d5c29e8033649a0644996bc.png)

[Click_prediction_small-20241112T061529Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XfcIb84kGovS3LxM0sccplZWndg)

+

![](img/e8e480dd811ed38c2fa45703bce7d820.png)

[boston-20241112T061528Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G03MbCiyMonqkdxsT1ecLEB3nwd)

+

![](img/db27a49dd8e8e2ddcef910f95672dd2f.png)

[07_icr-identify-age-related-conditions-20241112T061526Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Bi3Ub2xdIooCshx6FcrcT3EPnO4)

+

![](img/eb12889ecaf3323958820173b1931beb.png)

[04_titanic-20241112T061507Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XF99bhjsToo6YtxwNk7cFig1ntb)

+

![](img/6e8825aad19266f2f4c14e21ef7372ef.png)

[05_house-prices-advanced-regression-techniques-20241112T061507Z-001.zip](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/L59cb7FOUon4u4xl3vrcHEVzntb)

1.  从零开始准备数据集：

+   自行收集和处理原始数据

```
cd data
python dataset.py --save_analysis_pool
python hf_data.py --save_analysis_pool
```

## 配置

### 数据配置

+   **datasets.yaml****：** 为相应的数据集提供基础提示语、评估指标和目标列。

+   **data.yaml****：** 将 datasets_dir 修改为所有已准备好的数据集的基础目录。

### 大模型配置

+   [大模型 API 配置](https://deepwisdom.feishu.cn/wiki/VKwOwffAUitxPOk5wvMcNJBLndf)

### 任务配置

（首次尝试建议先不尝试）

+   在 **datasets.yaml**** **中可以找到每个任务的 user_requirement ，可以进行修改；比如添加：数据可视化

+   **注意：你所添**加的执行 Action，会在每次模拟实验中执行，所以需要谨慎添加

## **SELA**

### 安装

```
pip install -e .

cd metagpt/ext/sela

pip install -r requirements.txt
```

### 运行实验

+   python run_experiment.py --exp_mode mcts --task titanic --rollouts 10

+   python run_experiment.py --exp_mode mcts --task house-prices --rollouts 10 --low_is_better

### 参数

+   **--rollouts****：** 展开（模拟）的次数。

+   **-**-use_fixed_insights： 包含保存在 expo/insights/fixed_insights.json中的固定见解。

+   **--low_is_better****：** 如果数据集使用回归指标，则使用此参数。

+   **--from_scratch****：** 在运行蒙特卡洛树搜索（MCTS）之前，基于数据集生成新的见解池。

+   **--role_timeout****：** 限制单次模拟的持续时间（例如：10 rollouts with timeout 1,000 = 最多 10,000 秒）。

+   **--max_depth****：** 设置蒙特卡洛树搜索的最大深度（默认为 4）。

+   **--load_tree****：** 如果之前的实验被中断，加载已存在的蒙特卡洛树搜索树。

+   例子：python run_experiment.py --exp_mode mcts --task titanic --rollouts 10

### 消融研究

**随机搜索**

+   使用单个见解：python run_experiment.py --exp_mode rs --task titanic --rs_mode single

+   使用一组见解：python run_experiment.py --exp_mode rs --task titanic --rs_mode set

## 引用

如果您在研究中使用了 SELA，或觉得它很酷或有用，请引用我们的论文！

```
@misc{chi2024selatreesearchenhancedllm,
      title={SELA: Tree-Search Enhanced LLM Agents for Automated Machine Learning}, 
      author={Yizhou Chi and Yizhang Lin and Sirui Hong and Duyi Pan and Yaying Fei and Guanghao Mei and Bangbang Liu and Tianqi Pang and Jacky Kwok and Ceyao Zhang and Bang Liu and Chenglin Wu},
      year={2024},
      eprint={2410.17238},
      archivePrefix={arXiv},
      primaryClass={cs.AI},
      url={https://arxiv.org/abs/2410.17238}, 
}
```

## **论文解读**

[MetaGPT 开源 SELA，让 AI 设计与调优 AI，效果超越 OpenAI 使用的 AIDE](https://deepwisdom.feishu.cn/wiki/GvFiwiWFxi3qyFkfnobcFtZTnl3)