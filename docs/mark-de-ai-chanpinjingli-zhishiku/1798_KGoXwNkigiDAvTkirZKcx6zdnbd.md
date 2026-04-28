# 基于人气与协同过滤的推荐系统

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/LMNvdaf8XoSF8lxQ7vJcODCNnfd](https://qqs7y1hozd1.feishu.cn/docx/LMNvdaf8XoSF8lxQ7vJcODCNnfd)

## 1.数据集介绍

本数据集来源于Kaggle，原始数据集中有3个文件，这些文件是从一些图书销售网站中提取的。

Books——首先是关于书籍的，它包含了所有与书籍相关的信息，比如作者、书名、出版年份等。

Users——第二个文件包含注册用户的信息，如用户id、位置。

ratings——评级包含诸如哪个用户对哪本书给出了多少评级之类的信息。

## 2.技术工具

**Python版本: **3.9

**代码编辑器：**jupyter notebook

## 3.实验过程

### 3.1导入数据

```
import pandas as pd 
import numpy as np 
import matplotlib.pyplot as plt 
import seaborn as sns 
import warnings as w
w.filterwarnings(&#x27;ignore&#x27;)
from sklearn.metrics.pairwise import cosine_similarity
 
books = pd.read_csv(&#x27;Books.csv&#x27;)
users = pd.read_csv(&#x27;Users.csv&#x27;)
ratings = pd.read_csv(&#x27;Ratings.csv&#x27;)
```

### 3.2数据探索

书籍数据集

```
# books 
print(&#x27;shape of books &#x27;,books.shape)
print()
books.info()
```

![](img/4cb0fd03df7a3304eab5ca1a6c3de697.png)

用户数据集

```
# user 
print(&#x27;shape of users&#x27;,users.shape)
print()
users.info()
```

![](img/72f4441781612b172aef0e514543539e.png)

评分数据集

```
# ratings
print(&#x27;shape of rating&#x27;,ratings.shape)
print()
ratings.info()
```

![](img/b131b51640a8a973affb443ac765699c.png)

该数据集是可靠的，可以考虑作为一个大数据集。我们有271360本书的数据，网站上的注册用户总数约为27.8万，他们给出了近110万次评级。因此，我们可以说我们拥有的数据集是好的和可靠的。

### 3.3数据预处理

```
ratings[&#x27;Book-Rating&#x27;].value_counts().plot(kind=&#x27;bar&#x27;)
```

![](img/1b772a94897e6aba15b2eafd31e813f4.png)

在推荐系统中，通常会有隐式反馈数据，用户不会明确地提供评级，但他们的行为可以作为偏好的指示。在这种情况下，0的评级可能表示缺失或未知的评级，这通常被视为“隐含的负面反馈”。这意味着没有给一本书打分的用户并不一定表示他们不喜欢这本书;他们可能只是没有对此表达任何意见。

```
# 查找图书数据集中的空值
books.isnull().sum()
```

![](img/821218af4da88d8216fa5a1aff777736.png)

![](img/bb7593a358efa9f1cfcf23cb01c4d2b9.png)

```
books[&#x27;Year-Of-Publication&#x27;].value_counts().index.values
```

![](img/37c78f68dccaada365236e655d07aa2d.png)

存在无效值或非年份值，如'0'和'1376'。

```
# Step 1: 过滤掉无效或非年份值
valid_years = books[&#x27;Year-Of-Publication&#x27;].astype(str).str.isnumeric()
books = books[valid_years]
 
# Step 2: 将列数据转换为整数
books[&#x27;Year-Of-Publication&#x27;] = books[&#x27;Year-Of-Publication&#x27;].astype(int)
 
# Step 3: 从表示年份的整数中创建新的datetime列
books[&#x27;Publication_Date&#x27;] = pd.to_datetime(books[&#x27;Year-Of-Publication&#x27;], format=&#x27;%Y&#x27;, errors=&#x27;coerce&#x27;)
 
# 如果你不再需要旧的“出版年份”栏，请删除它
books.drop(columns=[&#x27;Year-Of-Publication&#x27;], inplace=True)
 
# 显示DataFrame以检查新的日期时间列
books = pd.DataFrame(books)
 
# Step 4: 创建一个只有年份部分为整数的新列
books[&#x27;Year-Of-Publication&#x27;] = books[&#x27;Publication_Date&#x27;].dt.year
```

```
books[&#x27;Year-Of-Publication&#x27;].value_counts().index.values
```

![](img/3b6d99861cbfb1094b0c162417509c7f.png)

```
# 这是目前所有无效的数据
books = books[~(books[&#x27;Year-Of-Publication&#x27;] == 2037)]
books = books[~(books[&#x27;Year-Of-Publication&#x27;] == 2026)] 
books = books[~(books[&#x27;Year-Of-Publication&#x27;] == 2030)]
books = books[~(books[&#x27;Year-Of-Publication&#x27;] == 2050)]
books = books[~(books[&#x27;Year-Of-Publication&#x27;] == 2038)]
```

```
plt.figure(figsize=(20,10))
sns.countplot(x=books[&#x27;Year-Of-Publication&#x27;]);
plt.xticks(rotation=90)
plt.show()
```

```
# 十大出版商
books[&#x27;Publisher&#x27;].value_counts().head(10).plot(kind=&#x27;bar&#x27;)
```

```
# 查找空值
round(100*(users.isnull().sum()/len(users.index)),2)
```

![](img/09dc4ed732f92ce89c96706237b485e3.png)

```
# 删除age列
users.drop(&#x27;Age&#x27;,axis=1,inplace=True)
users[&#x27;Location&#x27;] = users[&#x27;Location&#x27;].apply(lambda x:x.split(&#x27;,&#x27;)[-1])
```

```
# 十大用户位置
users[&#x27;Location&#x27;].value_counts().head(10).plot(kind=&#x27;bar&#x27;)
```

### 3.4基于人气的推荐系统

将显示平均评分最高的前50本书，但我们只考虑那些获得至少250票的书。

```
#合并数据评级和图书基于“ISBN”列
rating_books_name = ratings.merge(books,on=&#x27;ISBN&#x27;)
rating_books_name.head()
```

```
# 首先按“Book-Title”分组，找出投票总数(计数)
numer_rating = rating_books_name.groupby(&#x27;Book-Title&#x27;).count()[&#x27;Book-Rating&#x27;].reset_index()
numer_rating.rename(columns={&#x27;Book-Rating&#x27;:&#x27;Totle_number_rating&#x27;},inplace=True)
numer_rating.head()
```

```
# 总的平均评级
avg_rating = rating_books_name.groupby(&#x27;Book-Title&#x27;)[&#x27;Book-Rating&#x27;].mean().reset_index()
avg_rating.rename(columns={&#x27;Book-Rating&#x27;:&#x27;Totle_avg_rating&#x27;},inplace=True)
avg_rating.head()
```

```
# 合并&#x27;avg_rating&#x27;和&#x27;number_rating&#x27;基于&#x27;Book-Title&#x27;
popular_df = numer_rating.merge(avg_rating,on=&#x27;Book-Title&#x27;)
popular_df.head()
```

```
popular_df = popular_df[popular_df[&#x27;Totle_number_rating&#x27;] &gt;= 250].sort_values(&#x27;Totle_avg_rating&#x27;,ascending=False).head(50)
popular_df.head()
```

```
#  合并&#x27; popul_df &#x27;和&#x27;books&#x27;数据集
popular_df = popular_df.merge(books,on=&#x27;Book-Title&#x27;).drop_duplicates(&#x27;Book-Title&#x27;)[[&#x27;Book-Title&#x27;,&#x27;Book-Author&#x27;, &#x27;Image-URL-M&#x27;, &#x27;Totle_number_rating&#x27;,&#x27;Totle_avg_rating&#x27;]]
popular_df.head()
```

```
# 这都是前50本书
popular_df.head()
```

### 3.5基于协同过滤的推荐系统

不想在用户或书籍之间找到相似之处。我们想这样做，如果用户A读过并点赞x和y本书，用户B也点赞这两本书，现在用户A读过并点赞了B没有读过的z本书，所以我们必须向用户B推荐z本书，这就是协同过滤。

因此，这是使用矩阵分解实现的，我们将创建一个矩阵，其中列是用户，索引是图书，值是评级。比如我们需要创建一个数据透视表。

如果我们把所有的书和用户都拿来建模，你不觉得会产生问题吗?所以我们要做的就是减少用户和图书的数量，因为我们不能考虑只在网站上注册过或只读过一两本书的用户。对于这样的用户，我们不能依赖于向其他人推荐书籍，因为我们必须从数据中提取知识。所以我们会限制这个数字，我们会选择一个至少评价了200本书的用户，我们也会限制书籍，我们只会选择那些从用户那里得到至少50个评价的书。

因此，让我们开始分析并准备数据集，正如我们讨论的建模。让我们看看有多少用户给出了评分，并提取那些评分超过200的用户。

步骤1)提取超过200个的用户和评分

现在我们将提取给出超过200个评分的用户id，当我们有用户id时，我们将从评分数据框中提取仅该用户id的评分。

```
x = ratings[&#x27;User-ID&#x27;].value_counts() &gt; 200
y = x[x].index
print(y.shape)
```

![](img/3a51afc452a595c47ae9e0760def70a1.png)

只有899个用户的评分超过200

```
ratings = ratings[ratings[&#x27;User-ID&#x27;].isin(y)]
```

**步骤2)将评级与书籍合并**

900名用户给出了52万的评分，这是我们想要的。现在，我们将根据ISBN合并评分与图书，这样我们将得到每个用户对每个图书id的评分，而没有对该图书id评分的用户的值将为零。

```
rating_with_books = ratings.merge(books, on=&#x27;ISBN&#x27;)
rating_with_books.head(2)
```

**步骤3)提取评分超过50的书籍**

现在数据帧的大小减小了，我们有48万，因为当我们合并数据帧时，我们没有所有的book id-data。现在我们将计算每本书的评级，因此我们将根据标题对数据进行分组，并根据评级进行汇总。

```
number_rating = rating_with_books.groupby(&#x27;Book-Title&#x27;)[&#x27;Book-Rating&#x27;].count().reset_index()
number_rating.rename(columns= {&#x27;Book-Rating&#x27;:&#x27;number_of_ratings&#x27;}, inplace=True)
number_rating.head()
```

```
final_rating = rating_with_books.merge(number_rating, on=&#x27;Book-Title&#x27;)
final_rating = final_rating[final_rating[&#x27;number_of_ratings&#x27;] &gt;= 50]
final_rating.drop_duplicates([&#x27;User-ID&#x27;,&#x27;Book-Title&#x27;], inplace=True)
```

必须删除重复的值，因为如果同一用户多次评价同一本书，这将产生一个问题。最后，我们有一个用户的数据集，该用户对200多本图书进行了评分，并对50多本图书进行了评分。最终数据框的形状是59850行和8列。

**步骤4)创建数据透视表**

我们将创建一个数据透视表，其中列是用户id，索引是书名，值是评级。没有给任何一本书打分的用户id的value为NAN，因此将其归为0。

```
book_pivot = final_rating.pivot_table(columns=&#x27;User-ID&#x27;, index=&#x27;Book-Title&#x27;, values=&quot;Book-Rating&quot;)
book_pivot.fillna(0, inplace=True)
book_pivot
```

总计42本图书和888名用户

可以看到超过11个用户被移出，因为他们的评分是在那些评分不超过50的书上，所以他们被移出了。

**建模**

现在每本书都变成了888维空间中的向量，我们必须找到类似的向量。现在我们要计算每个向量和其他向量之间的欧氏距离根据这个距离我们就能知道哪本书是相似的。

使用余弦相似距离查找所有推荐的书籍

```
from sklearn.metrics.pairwise import cosine_similarity
similarity_scores = cosine_similarity(book_pivot)
similarity_scores
```

![](img/2e4965cd21350bd2cc51e7799ea4be80.png)

![](img/113e9f06a8cb92ef3d180ee8dff83e38.png)

(742,742)意味着每本书距离电影都很远

```
# 这是第一本书的距离，每本书的相似性
similarity_scores[0]
```

![](img/fb5f2693111401e4a6fb8876c551acd5.png)

```
# 定义一个根据书名进行推荐的函数
def recommend(book_name):
    index = np.where(book_pivot.index==book_name)[0][0]
    similar_items = sorted(list(enumerate(similarity_scores[index])),key=lambda x:x[1],reverse=True)[1:5]
    
    data = []
    for i in similar_items:
        item = []
        temp_df = books[books[&#x27;Book-Title&#x27;] == book_pivot.index[i[0]]]
        item.extend(list(temp_df.drop_duplicates(&#x27;Book-Title&#x27;)[&#x27;Book-Title&#x27;].values))
        item.extend(list(temp_df.drop_duplicates(&#x27;Book-Title&#x27;)[&#x27;Book-Author&#x27;].values))
        item.extend(list(temp_df.drop_duplicates(&#x27;Book-Title&#x27;)[&#x27;Image-URL-M&#x27;].values))
        data.append(item)
    
    return data
```

```
# 调用推荐函数，以“Harry Potter and the Chamber of Secrets (Book 2)”进行推荐
recommend(&#x27;Harry Potter and the Chamber of Secrets (Book 2)&#x27;)
```

## 4.总结

基于人气与协同过滤的图书推荐系统，通过对用户行为数据和图书人气信息的深入研究，以及协同过滤算法的应用，得出了以下几点总结：

**个性化推荐的重要性： **传统的图书推荐系统主要依赖于内容属性，但个性化推荐系统的兴起使得系统能够更好地理解用户的兴趣和需求。结合协同过滤算法，系统能够根据用户的历史行为数据，为每个用户提供更为个性化的图书推荐，从而提高推荐的精准度。

**人气信息的价值： **图书的人气信息是用户选择图书的重要参考因素之一。在推荐系统中，将人气信息纳入考虑范围，可以使系统更好地把握热门图书，为用户提供具有广泛吸引力的推荐。这对于提高用户体验、吸引更多读者具有积极作用。

**协同过滤的优越性： **协同过滤算法通过挖掘用户行为数据中的相似性，为用户提供与其兴趣相近的图书推荐。实验结果表明，协同过滤算法在提高推荐系统准确性方面具有显著优势。通过引入该算法，系统能够更好地适应用户的个性化需求，增强了推荐系统的智能化水平。

**综合优化： **在实验中，我们探索了如何综合利用人气信息和协同过滤算法，以进一步提高推荐系统的性能。通过精细调节算法参数和权重，我们成功实现了在保持系统简单性的同时提高了推荐的准确性。这为未来推荐系统的研究和应用提供了有益的经验。

+   综合，基于人气与协同过滤的图书推荐系统在本实验中取得了良好的效果，为提高图书推荐系统的智能化水平和用户体验提供了有益的参考。未来的研究方向可以在更多算法创新和用户反馈挖掘的基础上进一步完善推荐系统，以满足不断变化的用户需求。