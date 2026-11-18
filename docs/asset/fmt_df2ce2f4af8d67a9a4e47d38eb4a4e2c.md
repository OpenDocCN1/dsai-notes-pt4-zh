
```python
import json
import os
from typing import List, Dict, Any
from tqdm import tqdm
from vllm import LLM, SamplingParams
from datasets import Dataset

class MathAccuracy:
    """数学准确率评估器，使用math_verify包进行LaTeX解析和验证,参考swift accurary reward"""

    def __init__(self):
        import importlib.util
        assert importlib.util.find_spec('math_verify') is not None, (
            "The math_verify package is required but not installed. "
            "Please install it using 'pip install math_verify'.")

    def __call__(self, completions: List[str], solution: List[str], **kwargs) -> List[float]:
        from latex2sympy2_extended import NormalizationConfig
        from math_verify import LatexExtractionConfig, parse, verify

        rewards = []
        for content, sol in zip(completions, solution):
            gold_parsed = parse(sol, extraction_mode='first_match',
                              extraction_config=[LatexExtractionConfig()])

            if len(gold_parsed) != 0:
                # 解析模型生成的答案
                answer_parsed = parse(
                    content,
                    extraction_config=[
                        LatexExtractionConfig(
                            normalization_config=NormalizationConfig(
                                nits=False,
                                malformed_operators=False,
                                basic_latex=True,
                                boxed=True,
                                units=True,
                            ),
                            # 确保优先尝试匹配boxed内容
                            boxed_match_priority=0,
                            try_extract_without_anchor=False,
                        )
                    ],
                    extraction_mode='first_match',
                )
                # 如果内容与标准答案匹配，奖励为1，否则为0
                reward = float(verify(answer_parsed, gold_parsed))
            else:
                # 如果标准答案无法解析，跳过该样本并奖励1
                reward = 1.0

            rewards.append(reward)

        return rewards

def load_dataset(data_path: str) -> Dataset:
    if not os.path.exists(data_path):
        raise FileNotFoundError(f"数据集文件不存在: {data_path}")

    # 读取JSONL文件
    data = []
    with open(data_path, 'r', encoding='utf-8') as f:
        for line in f:
            if line.strip():
                data.append(json.loads(line.strip()))

    # 转换为Dataset对象
    dataset = Dataset.from_list(data)
    print(f"加载了 {len(dataset)} 个样本")

    return dataset

def format_prompt(messages: List[Dict[str, str]], tokenizer) -> str:
    # 检查是否有 chat template
    if hasattr(tokenizer, 'chat_template') and tokenizer.chat_template is not None:
        try:
            # 使用模型的chat template格式化消息
            prompt = tokenizer.apply_chat_template(
                messages,
                tokenize=False,
                add_generation_prompt=True,
            )
        except Exception as e:
            # 如果模板应用失败，回退到原始格式
            print(f"警告: 应用chat template失败 ({e})，使用备用格式")
            prompt = _format_fallback(messages)
    else:
        # 没有chat template，使用原始消息格式
        prompt = _format_fallback(messages)

    return prompt

def _format_fallback(messages: List[Dict[str, str]]) -> str:
    """
    备用格式化函数，当没有chat template时使用
    使用标准的 <s><|im_start|>{role}\n{content}<|im_end|> 格式
    """
    prompt = "<s>"
    for message in messages:
        role = message.get("role", "user")
        content = message.get("content", "")
        prompt += f"<|im_start|>{role}\n{content}<|im_end|>\n"

    # 添加assistant前缀，准备生成
    prompt += "<|im_start|>assistant\n"

    return prompt

def run_evaluation(
    model_path: str,
    data_path: str,
    output_path: str = None,
    tensor_parallel_size: int = 1,
    temperature: float = 0.0,
    max_tokens: int = 2048,
    batch_size: int = 32,
    seed: int = 42,
):
    dataset = load_dataset(data_path)

    prompts = []
    solutions = []

    for item in dataset:
        messages = item.get("messages", [])
        solution = item.get("solution", "")
        prompts.append(messages)  # 先保存原始messages
        solutions.append(solution)

    # 初始化vLLM模型
    llm = LLM(
        model=model_path,
        tensor_parallel_size=tensor_parallel_size,
        seed=seed,
        dtype="half",
        trust_remote_code=True,
    )

    # 获取模型的tokenizer用于应用chat template
    tokenizer = llm.get_tokenizer()

    # 格式化提示词
    formatted_prompts = []
    for messages in prompts:
        prompt = format_prompt(messages, tokenizer)
        formatted_prompts.append(prompt)

    # 配置采样参数
    sampling_params = SamplingParams(
        temperature=temperature,
        max_tokens=max_tokens,
        stop=["<|endoftext|>", "<|im_end|>"],
    )

    # 分批生成答案
    print("inferring...")
    all_completions = []

    for i in tqdm(range(0, len(formatted_prompts), batch_size), desc="生成进度"):
        batch_prompts = formatted_prompts[i:i + batch_size]

        outputs = llm.generate(
            batch_prompts,
            sampling_params=sampling_params,
            use_tqdm=False,
        )

        for output in outputs:
            # 获取生成的文本
            generated_text = output.outputs[0].text
            all_completions.append(generated_text)

    # 评估答案准确率
    print("evaluating...")
    evaluator = MathAccuracy()
    rewards = evaluator(all_completions, solutions)

    # 计算统计信息
    correct_count = sum(rewards)
    total_count = len(rewards)
    accuracy = correct_count / total_count * 100

    print(f"\n========== 评估结果 ==========")
    print(f"总样本数: {total_count}")
    print(f"正确数: {correct_count}")
    print(f"准确率: {accuracy:.2f}%")
    print(f"============================\n")

    # 保存详细结果
    if output_path:
        os.makedirs(os.path.dirname(output_path), exist_ok=True)

        results = {
            "model_path": model_path,
            "data_path": data_path,
            "total_samples": total_count,
            "correct_count": correct_count,
            "accuracy": accuracy,
            "individual_results": []
        }

        for i, (prompt, completion, solution, reward) in enumerate(zip(
            formatted_prompts, all_completions, solutions, rewards
        )):
            results["individual_results"].append({
                "index": i,
                "prompt": prompt,
                "completion": completion,
                "solution": solution,
                "reward": reward,
            })

        with open(output_path, 'w', encoding='utf-8') as f:
            json.dump(results, f, ensure_ascii=False, indent=2)

        print(f"详细结果已保存到: {output_path}")

    # 保存简洁结果（便于快速查看）
    summary_path = output_path.replace('.json', '_summary.json') if output_path else None
    if summary_path:
        summary = {
            "model_path": model_path,
            "total_samples": total_count,
            "correct_count": correct_count,
            "accuracy": accuracy,
        }

        with open(summary_path, 'w', encoding='utf-8') as f:
            json.dump(summary, f, ensure_ascii=False, indent=2)

        print(f"简洁结果已保存到: {summary_path}")

    return accuracy

def main():
    """主函数"""
    import argparse

    parser = argparse.ArgumentParser(description="GSM8K数学问题评估")

    parser.add_argument("--model_path", type=str,
                       default="/root/gsm8k_rl/output/Qwen2.5-Math-1.5B/checkpoint-2000",
                       help="模型路径或名称")
    parser.add_argument("--data_path", type=str,
                       default="/root/gsm8k_rl/data/test.jsonl",
                       help="数据集路径")
    parser.add_argument("--output_path", type=str,
                       default="/root/gsm8k_rl/eval_results.json",
                       help="输出结果路径")
    parser.add_argument("--tensor_parallel_size", type=int, default=1,
                       help="张量并行大小")
    parser.add_argument("--temperature", type=float, default=0.0,
                       help="采样温度（0表示贪婪解码）")
    parser.add_argument("--max_tokens", type=int, default=2048,
                       help="最大生成长度")
    parser.add_argument("--batch_size", type=int, default=32,
                       help="批处理大小")
    parser.add_argument("--seed", type=int, default=42,
                       help="随机种子")

    args = parser.parse_args()

    # 运行评估
    accuracy = run_evaluation(
        model_path=args.model_path,
        data_path=args.data_path,
        output_path=args.output_path,
        tensor_parallel_size=args.tensor_parallel_size,
        temperature=args.temperature,
        max_tokens=args.max_tokens,
        batch_size=args.batch_size,
        seed=args.seed,
    )

    return accuracy

if __name__ == "__main__":
    main()
```
