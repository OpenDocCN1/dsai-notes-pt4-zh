
```python
class IntelligentQuestionGenerator:
    def __init__(self):
        self.model = "claude-3-5-sonnet-20241022"
        
        self.question_types = {
            "选择题": "考察知识记忆和理解",
            "判断题": "考察概念辨析能力",
            "填空题": "考察知识点掌握",
            "简答题": "考察理解和表达能力",
            "计算题": "考察应用和计算能力",
            "编程题": "考察实践和问题解决能力"
        }
    
    def generate_questions(self, topic, difficulty, question_type, num_questions=5):
        """生成习题"""
        prompt = f"""作为专业的教育测评专家,为以下知识点出题:

知识点: {topic}
难度等级: {difficulty}(简单/中等/困难)
题型: {question_type}
题目数量: {num_questions}

出题要求:
1. 题目准确、清晰、无歧义
2. 难度符合要求
3. 覆盖知识点的不同方面
4. 包含详细的答案和解析
5. 如果是选择题,提供4个选项,包含干扰项

输出格式:
题目1:
【题干】...
【选项】(如适用)
A. ...
B. ...
C. ...
D. ...
【答案】...
【解析】...
【知识点】...
【难度】...

题目2:
...

请输出{num_questions}道题目:"""

        response = client.messages.create(
            model=self.model,
            max_tokens=2500,
            temperature=0.6,
            messages=[{"role": "user", "content": prompt}]
        )
        
        return response.content[0].text
    
    def generate_adaptive_questions(self, student_profile, topic):
        """根据学生水平生成个性化题目"""
        prompt = f"""根据学生的学习情况,生成个性化练习题:

学生画像:
- 当前水平: {student_profile['level']}
- 薄弱知识点: {', '.join(student_profile['weak_points'])}
- 学习进度: {student_profile['progress']}
- 错题类型: {', '.join(student_profile['error_types'])}

目标知识点: {topic}

要求:
1. 针对薄弱环节出题
2. 难度略高于当前水平(最近发展区)
3. 包含3道题目:
   - 1道巩固题(复习已学内容)
   - 1道提高题(挑战新知识)
   - 1道综合题(知识点融合)
4. 每道题包含详细解析和学习建议

输出题目和解析:"""

        response = client.messages.create(
            model=self.model,
            max_tokens=2000,
            temperature=0.5,
            messages=[{"role": "user", "content": prompt}]
        )
        
        return response.content[0].text
    
    def generate_exam_paper(self, exam_config):
        """生成完整试卷"""
        prompt = f"""生成一份完整的考试试卷:

考试信息:
- 科目: {exam_config['subject']}
- 考试时长: {exam_config['duration']}分钟
- 总分: {exam_config['total_score']}分
- 难度分布: 简单{exam_config['easy_ratio']}% / 中等{exam_config['medium_ratio']}% / 困难{exam_config['hard_ratio']}%
- 知识点: {', '.join(exam_config['topics'])}

题型分布:
{exam_config['question_distribution']}

要求:
1. 题目覆盖所有知识点
2. 难度分布合理
3. 时间分配恰当
4. 包含评分标准
5. 提供参考答案和评分细则

输出完整试卷:"""

        response = client.messages.create(
            model=self.model,
            max_tokens=4000,
            temperature=0.4,
            messages=[{"role": "user", "content": prompt}]
        )
        
        return response.content[0].text

# 使用示例
question_gen = IntelligentQuestionGenerator()

# 生成习题
questions = question_gen.generate_questions(
    topic="Python 列表操作",
    difficulty="中等",
    question_type="选择题",
    num_questions=5
)
print("生成的习题:")
print(questions)

# 个性化出题
student = {
    "level": "中级",
    "weak_points": ["列表切片", "列表推导式"],
    "progress": "已完成60%课程",
    "error_types": ["索引错误", "语法错误"]
}

adaptive_questions = question_gen.generate_adaptive_questions(student, "Python 列表高级操作")
print("\n个性化题目:")
print(adaptive_questions)

# 生成试卷
exam_config = {
    "subject": "Python 编程基础",
    "duration": 90,
    "total_score": 100,
    "easy_ratio": 30,
    "medium_ratio": 50,
    "hard_ratio": 20,
    "topics": ["变量与数据类型", "控制流", "函数", "列表与字典"],
    "question_distribution": "选择题20分,填空题20分,编程题60分"
}

exam_paper = question_gen.generate_exam_paper(exam_config)
print("\n试卷:")
print(exam_paper[:500] + "...")
```

### 7.3.3 作业批改辅助

**业务痛点**

+ 作业批改工作量大，耗时长
+ 难以给出个性化的反馈和建议
+ 主观题评分标准难以统一
+ 无法及时发现学生的共性问题

**解决方案架构**

````python
class HomeworkGradingAssistant:
    def __init__(self):
        self.model = "claude-3-5-sonnet-20241022"
    
    def grade_objective_questions(self, questions_answers):
        """批改客观题"""
        prompt = f"""批改以下客观题作业:

题目和学生答案:
{questions_answers}

要求:
1. 判断对错
2. 统计得分
3. 分析错题原因
4. 给出改进建议

输出格式:
题目1: ✓/✗ (得分/总分)
错误原因: ...
建议: ...

总分: X/Y
正确率: X%
薄弱知识点: ...
学习建议: ..."""

        response = client.messages.create(
            model=self.model,
            max_tokens=1500,
            temperature=0.0,  # 确保评分准确
            messages=[{"role": "user", "content": prompt}]
        )
        
        return response.content[0].text
    
    def grade_subjective_questions(self, question, answer, rubric):
        """批改主观题"""
        prompt = f"""根据评分标准批改以下主观题:

题目:
{question}

学生答案:
{answer}

评分标准:
{rubric}

要求:
1. 根据评分标准逐项打分
2. 指出答案的优点和不足
3. 给出具体的改进建议
4. 提供参考答案要点

输出格式:
## 评分详情
- 要点1: X/Y分 (评语)
- 要点2: X/Y分 (评语)
...

## 总分: X/Y分

## 优点:
- ...

## 不足:
- ...

## 改进建议:
- ...

## 参考答案要点:
- ..."""

        response = client.messages.create(
            model=self.model,
            max_tokens=1200,
            temperature=0.3,
            messages=[{"role": "user", "content": prompt}]
        )
        
        return response.content[0].text
    
    def grade_programming_assignment(self, code, requirements, test_cases):
        """批改编程作业"""
        prompt = f"""评估以下编程作业:

作业要求:
{requirements}

学生代码:
```python

{code}

```

测试用例：

{test_cases}

评估维度：

1.  功能完整性（40 分）: 是否满足所有要求

1.  代码质量（30 分）: 可读性、规范性、效率

1.  测试通过率（20 分）: 通过的测试用例比例

1.  创新性（10 分）: 是否有创新的解决方案

输出格式：

## 功能评估

+   要求 1: ✓/✗

+   要求 2: ✓/✗
...

## 代码质量

+   可读性： X/10 分 （评语）

+   规范性： X/10 分 （评语）

+   效率： X/10 分 （评语）

## 测试结果

+   通过： X/Y 个用例

+   失败： ……

## 总分： X/100 分

## 优点：

+   ...

## 问题：

+   ...

## 改进建议：

+   ...

## 参考代码：

```

# 关键改进点

...
"""

        response = client.messages.create(
            model=self.model,
            max_tokens=4000,
            temperature=0.4,
            messages=[{"role": "user", "content": prompt}]
        )
        
        return response.content[0].text
````
