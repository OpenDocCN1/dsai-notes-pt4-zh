# （还在更新中）沐曦 Swift 微调 InternLM 赛事 Baseline

> 来源：[https://aicarrier.feishu.cn/docx/CUIMdSBK8ojQ4lxXTfhc79Sonxd](https://aicarrier.feishu.cn/docx/CUIMdSBK8ojQ4lxXTfhc79Sonxd)

### 数据处理

正如刚才所说，本次比赛是限定在 26 类中进行分类，以下提供一个简单脚本，可用于生成 ms-swift 的 sft 数据（该脚本仅用于验证，并不保证得分）。

```
import json
import argparse
import os

def extract_category_mapping():
    &quot;&quot;&quot;定义类别到选项的映射&quot;&quot;&quot;
    category_to_option = {
        &#x27;quant-ph&#x27;: &#x27;A&#x27;,
        &#x27;physics.chem-ph&#x27;: &#x27;B&#x27;, 
        &#x27;physics.atom-ph&#x27;: &#x27;C&#x27;,
        &#x27;cond-mat.soft&#x27;: &#x27;D&#x27;,
        &#x27;cs.RO&#x27;: &#x27;E&#x27;,
        &#x27;cs.CL&#x27;: &#x27;F&#x27;,
        &#x27;cs.SE&#x27;: &#x27;G&#x27;,
        &#x27;cs.IR&#x27;: &#x27;H&#x27;,
        &#x27;hep-th&#x27;: &#x27;I&#x27;,
        &#x27;hep-ph&#x27;: &#x27;J&#x27;,
        &#x27;physics.optics&#x27;: &#x27;K&#x27;,
        &#x27;cs.AI&#x27;: &#x27;L&#x27;,
        &#x27;cs.CV&#x27;: &#x27;M&#x27;,
        &#x27;nucl-th&#x27;: &#x27;N&#x27;,
        &#x27;astro-ph&#x27;: &#x27;O&#x27;,
        &#x27;math.PR&#x27;: &#x27;P&#x27;,
        &#x27;cs.OS&#x27;: &#x27;Q&#x27;,
        &#x27;eess.SP&#x27;: &#x27;R&#x27;,
        &#x27;math.OC&#x27;: &#x27;S&#x27;,
        &#x27;math.DS&#x27;: &#x27;T&#x27;,
        &#x27;math.DG&#x27;: &#x27;U&#x27;,
        &#x27;math.MP&#x27;: &#x27;V&#x27;,
        &#x27;cs.MM&#x27;: &#x27;W&#x27;,
        &#x27;stat.ME&#x27;: &#x27;X&#x27;,
        &#x27;math.CO&#x27;: &#x27;Y&#x27;,
        &#x27;cs.NE&#x27;: &#x27;Z&#x27;
    }
    return category_to_option

def get_category_options_text():
    &quot;&quot;&quot;生成选项文本&quot;&quot;&quot;
    options = [
        &quot;A. quant-ph&quot;, &quot;B. physics.chem-ph&quot;, &quot;C. physics.atom-ph&quot;, &quot;D. cond-mat.soft&quot;,
        &quot;E. cs.RO&quot;, &quot;F. cs.CL&quot;, &quot;G. cs.SE&quot;, &quot;H. cs.IR&quot;, &quot;I. hep-th&quot;, &quot;J. hep-ph&quot;,
        &quot;K. physics.optics&quot;, &quot;L. cs.AI&quot;, &quot;M. cs.CV&quot;, &quot;N. nucl-th&quot;, &quot;O. astro-ph&quot;,
        &quot;P. math.PR&quot;, &quot;Q. cs.OS&quot;, &quot;R. eess.SP&quot;, &quot;S. math.OC&quot;, &quot;T. math.DS&quot;,
        &quot;U. math.DG&quot;, &quot;V. math.MP&quot;, &quot;W. cs.MM&quot;, &quot;X. stat.ME&quot;, &quot;Y. math.CO&quot;, &quot;Z. cs.NE&quot;
    ]
    return &quot;\n&quot;.join(options)

def process_paper(paper_data, verbose=False):
    &quot;&quot;&quot;处理单篇论文数据&quot;&quot;&quot;
    category_mapping = extract_category_mapping()
    
    # 提取基本信息
    paper_id = paper_data.get(&#x27;id&#x27;, &#x27;&#x27;)
    title = paper_data.get(&#x27;title&#x27;, &#x27;&#x27;).replace(&#x27;\n&#x27;, &#x27; &#x27;).strip()
    authors = paper_data.get(&#x27;authors&#x27;, &#x27;&#x27;)
    abstract = paper_data.get(&#x27;abstract&#x27;, &#x27;&#x27;).replace(&#x27;\n&#x27;, &#x27; &#x27;).strip()
    categories = paper_data.get(&#x27;categories&#x27;, &#x27;&#x27;)
    
    # 检查是否包含多个类别（用空格分隔）
    category_list = categories.split()
    if len(category_list) &gt; 1:
        if verbose:
            print(f&quot;跳过多类别论文 {paper_id}: {categories}&quot;)
        return None
    
    # 检查类别是否在我们的目标列表中
    target_category = category_list[0] if category_list else &#x27;&#x27;
    if target_category not in category_mapping:
        if verbose:
            print(f&quot;跳过非目标类别论文 {paper_id}: {target_category}&quot;)
        return None
    
    # 获取对应的选项字母
    correct_option = category_mapping[target_category]
    
    # 构建human问题
    options_text = get_category_options_text()
    human_content = f&quot;Based on the title &#x27;{title}&#x27;, authors &#x27;{authors}&#x27;, and abstract &#x27;{abstract}&#x27;, please determine the scientific category of this paper.\n\n{options_text}&quot;
    
    # 构建JSONL条目
    jsonl_entry = {
        &quot;system&quot;: &quot;你是个优秀的论文分类师&quot;,
        &quot;conversation&quot;: [
            {
                &quot;human&quot;: human_content,
                &quot;assistant&quot;: correct_option
            }
        ]
    }
    
    if verbose:
        print(f&quot;处理论文 {paper_id}: {target_category} -&gt; {correct_option}&quot;)
    
    return jsonl_entry

def main():
    &quot;&quot;&quot;主函数&quot;&quot;&quot;
    parser = argparse.ArgumentParser(description=&#x27;将arXiv论文JSON数据转换为分类训练JSONL格式&#x27;)
    
    parser.add_argument(&#x27;input_file&#x27;, 
                        help=&#x27;输入的JSON文件路径&#x27;)
    parser.add_argument(&#x27;output_file&#x27;, 
                        help=&#x27;输出的JSONL文件路径&#x27;)
    parser.add_argument(&#x27;--verbose&#x27;, &#x27;-v&#x27;, 
                        action=&#x27;store_true&#x27;,
                        help=&#x27;显示详细处理信息&#x27;)
    parser.add_argument(&#x27;--force&#x27;, &#x27;-f&#x27;,
                        action=&#x27;store_true&#x27;, 
                        help=&#x27;强制覆盖输出文件&#x27;)
    parser.add_argument(&#x27;--limit&#x27;, &#x27;-l&#x27;,
                        type=int,
                        help=&#x27;限制处理的论文数量&#x27;)
    
    args = parser.parse_args()
    
    # 检查输入文件是否存在
    if not os.path.exists(args.input_file):
        print(f&quot;错误: 输入文件 {args.input_file} 不存在&quot;)
        return 1
    
    # 检查输出文件是否已存在
    if os.path.exists(args.output_file) and not args.force:
        response = input(f&quot;输出文件 {args.output_file} 已存在，是否覆盖？(y/N): &quot;)
        if response.lower() != &#x27;y&#x27;:
            print(&quot;操作取消&quot;)
            return 1
    
    try:
        # 读取JSON数据
        with open(args.input_file, &#x27;r&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            content = f.read().strip()
        
        # 按行分割处理每个JSON对象
        lines = content.split(&#x27;\n&#x27;)
        
        processed_count = 0
        skipped_count = 0
        total_lines = len([line for line in lines if line.strip()])
        
        print(f&quot;开始处理 {total_lines} 行数据...&quot;)
        
        with open(args.output_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
            for i, line in enumerate(lines):
                if line.strip():
                    # 检查是否达到限制数量
                    if args.limit and processed_count &gt;= args.limit:
                        print(f&quot;达到处理限制 {args.limit}，停止处理&quot;)
                        break
                    
                    try:
                        paper_data = json.loads(line)
                        result = process_paper(paper_data, args.verbose)
                        
                        if result:
                            f.write(json.dumps(result, ensure_ascii=False) + &#x27;\n&#x27;)
                            processed_count += 1
                        else:
                            skipped_count += 1
                        
                        # 显示进度
                        if not args.verbose and (i + 1) % 100 == 0:
                            print(f&quot;已处理 {i + 1}/{total_lines} 行，成功: {processed_count}，跳过: {skipped_count}&quot;)
                            
                    except json.JSONDecodeError as e:
                        if args.verbose:
                            print(f&quot;第 {i+1} 行JSON解析错误: {e}&quot;)
                        skipped_count += 1
        
        print(f&quot;\n处理完成!&quot;)
        print(f&quot;成功处理: {processed_count} 篇论文&quot;)
        print(f&quot;跳过: {skipped_count} 篇论文&quot;)
        print(f&quot;输出文件: {args.output_file}&quot;)
        
        return 0
        
    except FileNotFoundError:
        print(f&quot;错误: 找不到输入文件 {args.input_file}&quot;)
        return 1
    except Exception as e:
        print(f&quot;处理过程中出现错误: {e}&quot;)
        return 1

if __name__ == &quot;__main__&quot;:
    exit(main())
```

用法如下：

```
python process_papers.py input.json output.jsonl
```

```
# 显示详细处理信息
python process_papers.py input.json output.jsonl --verbose

# 强制覆盖输出文件
python process_papers.py input.json output.jsonl --force

# 限制处理前1000篇论文
python process_papers.py input.json output.jsonl --limit 1000

# 组合使用多个参数
python process_papers.py input.json output.jsonl -v -f -l 500
```

**参数说明：**

+   input_file: 必需参数，输入的JSON文件路径

+   output_file: 必需参数，输出的JSONL文件路径

+   --verbose, -v: 可选，显示详细的处理信息，包括每篇论文的处理状态

+   --force, -f: 可选，强制覆盖已存在的输出文件

+   --limit, -l: 可选，限制处理的论文数量，用于测试或部分处理

特别注意，请先初始化共享存储，并在we端上传一个txt文件，否则可能挂载不上

转换后的数据，将SFT微调数据上传至/root/data/datasets  （在/root/data目录下新建一个datasets目录）

![](img/3f7f372ff52fd0417b6255a78764877e.png)

[output.jsonl](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TwZPb3ebpoZvgcx4Zt4cgSGfnEl)

#### 数据样例

```
{&quot;system&quot;: &quot;你是个优秀的论文分类师&quot;, &quot;conversation&quot;: [{&quot;human&quot;: &quot;Based on the title &#x27;Calculation of prompt diphoton production cross sections at Tevatron and   LHC energies&#x27;, authors &#x27;C. Bal\\&#x27;azs, E. L. Berger, P. M. Nadolsky, C.-P. Yuan&#x27;, and abstract &#x27;A fully differential calculation in perturbative quantum chromodynamics is presented for the production of massive photon pairs at hadron colliders. All next-to-leading order perturbative contributions from quark-antiquark, gluon-(anti)quark, and gluon-gluon subprocesses are included, as well as all-orders resummation of initial-state gluon radiation valid at next-to-next-to-leading logarithmic accuracy. The region of phase space is specified in which the calculation is most reliable. Good agreement is demonstrated with data from the Fermilab Tevatron, and predictions are made for more detailed tests with CDF and DO data. Predictions are shown for distributions of diphoton pairs produced at the energy of the Large Hadron Collider (LHC). Distributions of the diphoton pairs from the decay of a Higgs boson are contrasted with those produced from QCD processes at the LHC, showing that enhanced sensitivity to the signal can be obtained with judicious selection of events.&#x27;, please determine the scientific category of this paper.\n\nA. quant-ph\nB. physics.chem-ph\nC. physics.atom-ph\nD. cond-mat.soft\nE. cs.RO\nF. cs.CL\nG. cs.SE\nH. cs.IR\nI. hep-th\nJ. hep-ph\nK. physics.optics\nL. cs.AI\nM. cs.CV\nN. nucl-th\nO. astro-ph\nP. math.PR\nQ. cs.OS\nR. eess.SP\nS. math.OC\nT. math.DS\nU. math.DG\nV. math.MP\nW. cs.MM\nX. stat.ME\nY. math.CO\nZ. cs.NE&quot;, &quot;assistant&quot;: &quot;J&quot;}]}
{&quot;system&quot;: &quot;你是个优秀的论文分类师&quot;, &quot;conversation&quot;: [{&quot;human&quot;: &quot;Based on the title &#x27;A determinant of Stirling cycle numbers counts unlabeled acyclic   single-source automata&#x27;, authors &#x27;David Callan&#x27;, and abstract &#x27;We show that a determinant of Stirling cycle numbers counts unlabeled acyclic single-source automata. The proof involves a bijection from these automata to certain marked lattice paths and a sign-reversing involution to evaluate the determinant.&#x27;, please determine the scientific category of this paper.\n\nA. quant-ph\nB. physics.chem-ph\nC. physics.atom-ph\nD. cond-mat.soft\nE. cs.RO\nF. cs.CL\nG. cs.SE\nH. cs.IR\nI. hep-th\nJ. hep-ph\nK. physics.optics\nL. cs.AI\nM. cs.CV\nN. nucl-th\nO. astro-ph\nP. math.PR\nQ. cs.OS\nR. eess.SP\nS. math.OC\nT. math.DS\nU. math.DG\nV. math.MP\nW. cs.MM\nX. stat.ME\nY. math.CO\nZ. cs.NE&quot;, &quot;assistant&quot;: &quot;Y&quot;}]}
{&quot;system&quot;: &quot;你是个优秀的论文分类师&quot;, &quot;conversation&quot;: [{&quot;human&quot;: &quot;Based on the title &#x27;The Spitzer c2d Survey of Large, Nearby, Insterstellar Clouds. IX. The   Serpens YSO Population As Observed With IRAC and MIPS&#x27;, authors &#x27;Paul Harvey, Bruno Merin, Tracy L. Huard, Luisa M. Rebull, Nicholas\n  Chapman, Neal J. Evans II, Philip C. Myers&#x27;, and abstract &#x27;We discuss the results from the combined IRAC and MIPS c2d Spitzer Legacy observations of the Serpens star-forming region. In particular we present a set of criteria for isolating bona fide young stellar objects, YSO&#x27;s, from the extensive background contamination by extra-galactic objects. We then discuss the properties of the resulting high confidence set of YSO&#x27;s. We find 235 such objects in the 0.85 deg^2 field that was covered with both IRAC and MIPS. An additional set of 51 lower confidence YSO&#x27;s outside this area is identified from the MIPS data combined with 2MASS photometry. We describe two sets of results, color-color diagrams to compare our observed source properties with those of theoretical models for star/disk/envelope systems and our own modeling of the subset of our objects that appear to be star+disks. These objects exhibit a very wide range of disk properties, from many that can be fit with actively accreting disks to some with both passive disks and even possibly debris disks. We find that the luminosity function of YSO&#x27;s in Serpens extends down to at least a few x .001 Lsun or lower for an assumed distance of 260 pc. The lower limit may be set by our inability to distinguish YSO&#x27;s from extra-galactic sources more than by the lack of YSO&#x27;s at very low luminosities. A spatial clustering analysis shows that the nominally less-evolved YSO&#x27;s are more highly clustered than the later stages and that the background extra-galactic population can be fit by the same two-point correlation function as seen in other extra-galactic studies. We also present a table of matches between several previous infrared and X-ray studies of the Serpens YSO population and our Spitzer data set.&#x27;, please determine the scientific category of this paper.\n\nA. quant-ph\nB. physics.chem-ph\nC. physics.atom-ph\nD. cond-mat.soft\nE. cs.RO\nF. cs.CL\nG. cs.SE\nH. cs.IR\nI. hep-th\nJ. hep-ph\nK. physics.optics\nL. cs.AI\nM. cs.CV\nN. nucl-th\nO. astro-ph\nP. math.PR\nQ. cs.OS\nR. eess.SP\nS. math.OC\nT. math.DS\nU. math.DG\nV. math.MP\nW. cs.MM\nX. stat.ME\nY. math.CO\nZ. cs.NE&quot;, &quot;assistant&quot;: &quot;O&quot;}]}
```

### 训练

#### 环境安装

安装 ms-swift 由于实例重启会重置/root/data 目录之外的所有数据，我们所有的工作目录都在/root/data下

```
#1.conda新建ms-swift环境
conda create --prefix=/root/data/envs/ms-swift python=3.10 -y

#2.切换conda环境
conda activate /root/data/envs/ms-swift

#3.安装ms-swift训练框架
pip install &#x27;ms-swift&#x27;
```

我们需要看见适配沐曦的特定包 ，新建requirements.txt 填入以下内容

```
apex==0.1+metax2.32.0.3
torch==2.6.0+metax2.32.0.3
torchaudio==2.4.1+metax2.32.0.3
torchvision==0.15.1+metax2.32.0.3
triton==3.0.0+metax2.32.0.3 
flash_attn
```

```
##安装沐曦专用的包
pip install -r requirements.txt -i https://repos.metax-tech.com/r/maca-pypi/simple --trusted-host repos.metax-tech.com --no-build-isolation
```

## 链接模型位置

```
cd /root/data

ln -s /root/public-model/models/Shanghai_AI_Laboratory/internlm2_5-1_8b-chat ./
```

#### 训练

新建paper_sft_ms-swift.sh  脚本内容如下：

```
#!/bin/bash

# 创建日志目录
LOG_DIR=&quot;logs&quot;
mkdir -p $LOG_DIR

# 获取当前时间戳
TIMESTAMP=$(date +&quot;%Y%m%d_%H%M%S&quot;)
LOG_FILE=&quot;$LOG_DIR/internlm2.5-1.8b_lora_sft_${TIMESTAMP}.log&quot;

# 设置CUDA设备
#export NPROC_PER_NODE=1
export OMP_NUM_THREADS=1
export CUDA_VISIBLE_DEVICES=0

nohup swift sft \
    --model {path-of-InternLM2.5-1.8B} \
    --train_type lora \
    --dataset {path-of-your-data} \
    --torch_dtype bfloat16 \
    --num_train_epochs 4 \
    --per_device_train_batch_size 4 \
    --learning_rate 5e-5 \
    --warmup_ratio 0.1 \
    --split_dataset_ratio 0 \
    --lora_rank 8 \
    --lora_alpha 32 \
    --target_modules all-linear \
    --gradient_accumulation_steps 2 \
    --save_steps 2000 \
    --save_total_limit 5 \
    --gradient_checkpointing_kwargs &#x27;{&quot;use_reentrant&quot;: false}&#x27; \
    --logging_steps 5 \
    --max_length 2048 \
    --output_dir ./swift_output/InternLM2.5-1.8B-Lora \
    --dataloader_num_workers 256 \
    --model_author JimmyMa99 \
    --model_name InternLM2.5-1.8B-Lora \
    &gt; &quot;$LOG_FILE&quot; 2&gt;&amp;1 &amp;

# 打印进程ID和日志文件位置
echo &quot;Training started with PID $!&quot;
echo &quot;Log file: $LOG_FILE&quot;

# 显示查看日志的命令
echo &quot;To view logs in real-time, use:&quot;
echo &quot;tail -f $LOG_FILE&quot;
```

要注意在 --model 和 --dataset 中分别填入模型的路径和数据集的路径

在终端中运行：

```
bash paper_sft_ms-swift.sh
```

接着会在所在工作路径内生成 logs、swift_output 等文件夹，同时也可以根据终端显示指示来开启实时 log 显示。

#### 合并权重

使用 lora 训练需要合并权重以获得完整的模型权重，使用以下命令

```
swift export --adapters xx/checkpoint-xxx --merge_lora true --output_dir {save-dir}
```

随后就会在对应的{save-dir}中得到合并权重

#### 上传权重

为了保证比赛公正，本次使用的魔乐社区托管权重，请使用组织功能，并邀请JimmyMa-99 用户加入团队，同时请将上传的权重设置为“私有”。

![](img/8eec89f78a2d6a46f3a881340d16749d.png)

![](img/c1cd642a70c4f5beb5c537e4667ed7a7.png)

注意：邀请团队需要用户同意，如在 24 小时内没有同意邀请，请联系🛰️`xxxxx`

注意注意：需要将训练的 log 和权重放在同一个文件夹一同上传

上传权重的脚本如下：

```
from openmind_hub import upload_folder,create_repo

create_repo(
    token=&quot;&quot;,
    repo_id=&quot;&quot;,
    repo_type=&quot;model&quot;
)

upload_folder(
    token=&quot;&quot;,
    folder_path=&quot;/root/data/swift output/internlm2 5-7b-Lora/v8-20258623-114320/checkpoint-2800-merged&quot;,
    repo_id=&quot;&quot;,
)

```

#### 提交测评

提交测评前务必保证组织内模型为私有，且邀请JimmyMa-99 用户加入团队，否则结果会为-1.

提交测评的[入口](https://aicarrier.feishu.cn/share/base/form/shrcnCR6hPPtl3v3y2ZVXomhME9)：https://aicarrier.feishu.cn/share/base/form/shrcnCR6hPPtl3v3y2ZVXomhME9，输入 uid 和对应的微调权重的模型 ID。

![](img/31750b109b028287fef2889b53468f9e.png)

注意看以上的红字！！！！

**第一个空是 UID，对应的是**第五期学员 ID，不要提交开发机 ID！！！！

第二个空是魔乐社区模型 ID，**不要提交链接！！！！**

#### 查询分数

查询测评的分数[入口](https://aicarrier.feishu.cn/share/base/query/shrcn3UPdvR9e0D1REcjQ9LLNub)：https://aicarrier.feishu.cn/share/base/query/shrcn3UPdvR9e0D1REcjQ9LLNub 输入 uid 即可查询：