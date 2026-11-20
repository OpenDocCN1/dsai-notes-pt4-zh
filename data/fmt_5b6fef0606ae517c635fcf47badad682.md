
```python
import numpy as np
from collections import defaultdict
import pickle
import jieba

class GloVe:
    def __init__(self, embedding_size=100, context_size=5, x_max=100, alpha=0.75):
        """
        初始化GloVe模型参数
        embedding_size: 词向量维度
        context_size: 上下文窗口大小
        x_max: 权重函数参数
        alpha: 权重函数指数
        """
        self.embedding_size = embedding_size
        self.context_size = context_size
        self.x_max = x_max
        self.alpha = alpha
        self.word2index = {}
        self.index2word = {}
        self.vocab_size = 0
        self.word_vectors = None
        self.context_vectors = None
        self.biases = None
        self.context_biases = None
        self.cooccur = None

    def build_vocab(self, corpus):
        """
        构建词汇表
        corpus: 分词后的文本列表
        """
        vocab = set()
        for sentence in corpus:
            vocab.update(sentence)
        
        self.vocab_size = len(vocab)
        self.word2index = {word: idx for idx, word in enumerate(vocab)}
        self.index2word = {idx: word for word, idx in self.word2index.items()}
        
        # 初始化参数矩阵
        self.word_vectors = np.random.uniform(
            -0.5/self.embedding_size, 
            0.5/self.embedding_size, 
            (self.vocab_size, self.embedding_size)
        )
        self.context_vectors = np.random.uniform(
            -0.5/self.embedding_size, 
            0.5/self.embedding_size, 
            (self.vocab_size, self.embedding_size)
        )
        self.biases = np.zeros(self.vocab_size)
        self.context_biases = np.zeros(self.vocab_size)

    def build_cooccurence_matrix(self, corpus):
        """
        构建共现矩阵
        """
        cooccur = defaultdict(float)
        
        for sentence in corpus:
            for i, center_word in enumerate(sentence):
                center_idx = self.word2index[center_word]
                # 定义上下文窗口
                start = max(0, i - self.context_size)
                end = min(len(sentence), i + self.context_size + 1)
                
                for j in range(start, end):
                    if i != j:  # 不计算单词与自身的共现
                        context_word = sentence[j]
                        context_idx = self.word2index[context_word]
                        # 使用距离加权
                        distance = abs(i - j)
                        weight = 1.0 / distance
                        cooccur[(center_idx, context_idx)] += weight
        
        self.cooccur = cooccur

    def weight_function(self, x):
        """
        计算权重函数 f(x)
        """
        if x < self.x_max:
            return (x / self.x_max) ** self.alpha
        return 1.0

    def train(self, num_epochs=100, learning_rate=0.05):
        """
        训练GloVe模型
        """
        for epoch in range(num_epochs):
            total_loss = 0
            # 随机打乱词对
            word_pairs = list(self.cooccur.keys())
            np.random.shuffle(word_pairs)
            
            for word_idx, context_idx in word_pairs:
                cooccur_count = self.cooccur[(word_idx, context_idx)]
                weight = self.weight_function(cooccur_count)
                
                # 计算loss
                word_vec = self.word_vectors[word_idx]
                context_vec = self.context_vectors[context_idx]
                word_bias = self.biases[word_idx]
                context_bias = self.context_biases[context_idx]
                
                inner_product = np.dot(word_vec, context_vec)
                log_cooccur = np.log(cooccur_count + 1)
                
                loss = weight * (inner_product + word_bias + context_bias - log_cooccur) ** 2
                total_loss += loss
                
                # 计算梯度
                grad_common = 2 * weight * (inner_product + word_bias + context_bias - log_cooccur)
                grad_word = grad_common * context_vec
                grad_context = grad_common * word_vec
                grad_bias = grad_common
                grad_context_bias = grad_common
                
                # 更新参数
                self.word_vectors[word_idx] -= learning_rate * grad_word
                self.context_vectors[context_idx] -= learning_rate * grad_context
                self.biases[word_idx] -= learning_rate * grad_bias
                self.context_biases[context_idx] -= learning_rate * grad_context_bias
            
            if epoch % 10 == 0:
                print(f"Epoch {epoch}, Loss: {total_loss}")

    def get_word_vector(self, word):
        """
        获取单词的向量表示
        """
        idx = self.word2index.get(word)
        if idx is not None:
            # 返回中心词向量和上下文向量的和
            return self.word_vectors[idx] + self.context_vectors[idx]
        return None

    def save_model(self, filepath):
        """
        保存模型
        """
        model_data = {
            'embedding_size': self.embedding_size,
            'context_size': self.context_size,
            'x_max': self.x_max,
            'alpha': self.alpha,
            'word2index': self.word2index,
            'index2word': self.index2word,
            'word_vectors': self.word_vectors,
            'context_vectors': self.context_vectors,
            'biases': self.biases,
            'context_biases': self.context_biases
        }
        with open(filepath, 'wb') as f:
            pickle.dump(model_data, f)

    def load_model(self, filepath):
        """
        加载模型
        """
        with open(filepath, 'rb') as f:
            model_data = pickle.load(f)
        self.embedding_size = model_data['embedding_size']
        self.context_size = model_data['context_size']
        self.x_max = model_data['x_max']
        self.alpha = model_data['alpha']
        self.word2index = model_data['word2index']
        self.index2word = model_data['index2word']
        self.word_vectors = model_data['word_vectors']
        self.context_vectors = model_data['context_vectors']
        self.biases = model_data['biases']
        self.context_biases = model_data['context_biases']
        self.vocab_size = len(self.word2index)

    def similarity(self, word1, word2):
        """
        计算两个单词的余弦相似度
        """
        vec1 = self.get_word_vector(word1)
        vec2 = self.get_word_vector(word2)
        if vec1 is None:
            print(f"错误：单词 '{word1}' 不在词汇表中")
            return None
        if vec2 is None:
            print(f"错误：单词 '{word2}' 不在词汇表中")
            return None
        norm1 = np.linalg.norm(vec1)
        norm2 = np.linalg.norm(vec2)
        if norm1 == 0 or norm2 == 0:
            print(f"错误：单词 '{word1}' 或 '{word2}' 的向量范数为零")
            return None
        return np.dot(vec1, vec2) / (norm1 * norm2)

    def most_similar(self, positive, negative=None, topn=10):
        """
        找到与目标最相似的单词，支持类比任务
        positive: 正向单词列表
        negative: 负向单词列表
        topn: 返回前n个最相似的单词
        """
        if isinstance(positive, str):
            positive = [positive]
        if negative is None:
            negative = []

        # 计算目标向量
        target_vector = np.zeros(self.embedding_size)
        valid_words = 0
        for word in positive:
            vec = self.get_word_vector(word)
            if vec is not None:
                target_vector += vec
                valid_words += 1
            else:
                print(f"警告：单词 '{word}' 不在词汇表中，已忽略")
        for word in negative:
            vec = self.get_word_vector(word)
            if vec is not None:
                target_vector -= vec
                valid_words += 1
            else:
                print(f"警告：单词 '{word}' 不在词汇表中，已忽略")

        if valid_words == 0:
            print("错误：所有输入单词都不在词汇表中")
            return []

        # 计算所有单词与目标向量的相似度
        similarities = {}
        target_norm = np.linalg.norm(target_vector)
        if target_norm == 0:
            print("错误：目标向量的范数为零")
            return []

        for word in self.word2index:
            if word in positive or word in negative:
                continue
            vec = self.get_word_vector(word)
            if vec is not None:
                vec_norm = np.linalg.norm(vec)
                if vec_norm > 0:  # 确保向量范数不为零
                    sim = np.dot(target_vector, vec) / (target_norm * vec_norm)
                    similarities[word] = sim

        # 按相似度排序并返回前topn个结果
        sorted_similarities = sorted(similarities.items(), key=lambda x: x[1], reverse=True)
        return sorted_similarities[:topn]

# 使用示例
if __name__ == "__main__":
    # 提供的语料库
    raw_corpus = [
        "猫喜欢吃鱼",
        "狗喜欢吃肉",
        "猫和狗是宠物",
        "国王和王后统治国家",
        "男人和女人是朋友"
    ]
    
    # 对语料库进行分词
    corpus = [list(jieba.cut(sentence)) for sentence in raw_corpus]
    
    # 初始化模型
    glove = GloVe(
        embedding_size=50,  # 词向量维度
        context_size=2,     # 上下文窗口大小
        x_max=100,          # 权重函数参数
        alpha=0.75         # 权重函数指数
    )
    
    # 构建词汇表
    glove.build_vocab(corpus)
    
    # 打印分词结果和词汇表，便于调试
    print("分词后的语料库：")
    for i, sentence in enumerate(corpus):
        print(f"句子 {i+1}: {sentence}")
    print("\n词汇表：", list(glove.word2index.keys()))
    
    # 构建共现矩阵
    glove.build_cooccurence_matrix(corpus)
    
    # 训练模型
    glove.train(num_epochs=100, learning_rate=0.05)
    
    # 保存模型
    glove.save_model("glove_model.pkl")
    
    # 加载模型
    glove = GloVe()  # 创建新实例
    glove.load_model("glove_model.pkl")
    
    # 警告：语料库较小
    print("\n警告：当前语料库较小（仅包含5句话），训练结果可能不够准确。建议使用更大的语料库以获得更好的词向量质量。\n")
    
    # 任务1：获取单词的向量表示
    print("任务1：获取单词的向量表示")
    cat_vector = glove.get_word_vector("猫")
    if cat_vector is not None:
        print("‘猫’的词向量（前5维）：", cat_vector[:5])  # 只显示前5维以节省空间
    else:
        print("错误：单词 '猫' 不在词汇表中")

    # 任务2：计算单词之间的相似度
    print("\n任务2：计算单词之间的相似度")
    similarity = glove.similarity("猫", "狗")  # 计算“猫”和“狗”的相似度
    if similarity is not None:
        print("‘猫’和‘狗’的相似度：", similarity)

    # 任务3：解决类比问题（国王:王后 :: 男人: ?）
    print("\n任务3：解决类比问题")
    result = glove.most_similar(positive=["男人", "王后"], negative=["国王"], topn=1)
    if result:
        print("‘国王:王后 :: 男人: ?’的答案是：", result[0][0])
    else:
        print("错误：无法完成类比任务，请检查输入单词是否在词汇表中")

    # 任务4：找到与某个单词最相似的单词
    print("\n任务4：找到与‘猫’最相似的单词")
    similar_words = glove.most_similar("猫", topn=3)
    if similar_words:
        print("与‘猫’最相似的3个单词：", similar_words)
    else:
        print("错误：无法找到与‘猫’相似的单词，请检查词汇表")
```
