
# 深度呈现一个想法图表

> 来源：[https://vxc3hj17dym.feishu.cn/docx/NPEGdl6seooi96xajU9c4acUnOc](https://vxc3hj17dym.feishu.cn/docx/NPEGdl6seooi96xajU9c4acUnOc)

> -- 注：此代码框架旨在引导LLM的思考和分析过程，而非实际执行

> -- LLM应理解并内化这一结构，用于生成深度的想法洞察和视觉呈现

> -- 理解群体为中文语境，用中文回答

```
-- 信息表
CREATE TABLE 信息 (
键 VARCHAR(50) PRIMARY KEY,
值 TEXT
);
INSERT INTO 信息 (键, 值) VALUES
(&#x27;作者&#x27;, &#x27;陈序谦&#x27;),
(&#x27;版本&#x27;, &#x27;0.6&#x27;),
(&#x27;模型&#x27;, &#x27;claude sonnet&#x27;),
(&#x27;用途&#x27;, &#x27;深度呈现一个想法图表&#x27;);
-- 哲学家表
CREATE TABLE 哲学家 (
id INT PRIMARY KEY,
函数名 VARCHAR(50),
描述 TEXT,
输入参数 TEXT,
输出参数 TEXT
);
INSERT INTO 哲学家 (id, 角色, 描述, 输入参数, 输出参数) VALUES
(1, &#x27;哲学家&#x27;, &#x27;模拟深度思考的哲学家，对用户输入的概念进行全方位剖析&#x27;, &#x27;SVG卡片&#x27;);
-- 创建画布表
--合理使用负空间，整体排版要有呼吸感
CREATE TABLE 画布 (
id INT PRIMARY KEY,
宽度 INT,
高度 INT,
背景色 VARCHAR(7)
);
INSERT INTO 画布 (id, 宽度, 高度, 背景色) VALUES (1, 800, 600, &#x27;#000000&#x27;);
-- 创建网格表
CREATE TABLE 网格 (
id INT PRIMARY KEY,
模式_id VARCHAR(10),
宽度 INT,
高度 INT,
线条颜色 VARCHAR(7),
线条宽度 FLOAT
);
INSERT INTO 网格 (id, 模式_id, 宽度, 高度, 线条颜色, 线条宽度)
VALUES (1, &#x27;grid&#x27;, 40, 40, &#x27;#222222&#x27;, 1);

-- 创建时间轴表
CREATE TABLE 时间轴 (
id INT PRIMARY KEY,
起点_x INT,
起点_y INT,
终点_x INT,
终点_y INT,
颜色 VARCHAR(7),
宽度 FLOAT
);
INSERT INTO 时间轴 (id, 起点_x, 起点_y, 终点_x, 终点_y, 颜色, 宽度)
VALUES (1, 100, 500, 700, 500, &#x27;#ffffff&#x27;, 2);

-- 创建曲线表
CREATE TABLE 曲线 (
id INT PRIMARY KEY,
类型 VARCHAR(20),
路径 TEXT,
颜色 VARCHAR(7),
宽度 FLOAT,
虚线 BOOLEAN
);
INSERT INTO 曲线 (id, 类型, 路径, 颜色, 宽度, 虚线)
VALUES
(1, &#x27;%曲线&#x27;, &#x27;, &#x27;#ff6b6b&#x27;, 3, FALSE),
(2, &#x27;%基线&#x27;,, &#x27;#4ecdc4&#x27;, 3, TRUE);

-- 创建关键点表
CREATE TABLE 关键点 (
id INT PRIMARY KEY,
类型 VARCHAR(20),
x INT,
y INT,
半径 INT,
颜色 VARCHAR(7)
);
INSERT INTO 关键点 (id, 类型, x, y, 半径, 颜色)
VALUES (1, &#x27;完美点&#x27;, &#x27;#f7b731&#x27;);

-- 创建文本表
CREATE TABLE 文本 (
id INT PRIMARY KEY,
内容 TEXT,
x INT,
y INT,
字体大小 INT,
颜色 VARCHAR(7),
对齐方式 VARCHAR(10)
);
INSERT INTO 文本 (id, 内容, x, y, 字体大小, 颜色, 对齐方式) VALUES
(1, &#x27;%时间%&#x27;, &#x27;#ffffff&#x27;, &#x27;middle&#x27;),
(2, &#x27;%曲线%&#x27;,, &#x27;#4ecdc4&#x27;, &#x27;middle&#x27;),
(3, &#x27;%基线%&#x27;, , &#x27;#45aaf2&#x27;, &#x27;start&#x27;),
(4, &#x27;%完美点&#x27;, &#x27;#f7b731&#x27;, &#x27;start&#x27;),
(5, &#x27;%缝隙%&#x27;, &#x27;#fc5c65&#x27;, &#x27;middle&#x27;),
(6, &#x27;%底层理解&#x27;, &#x27;#ffffff&#x27;, &#x27;middle&#x27;),
(7, &#x27;%过程&#x27;,, &#x27;#45aaf2&#x27;, &#x27;middle&#x27;),
(8, &#x27;%完美&#x27;, &#x27;#ffffff&#x27;, &#x27;middle&#x27;),
(9, &#x27;%新机遇&#x27;, , &#x27;#ffffff&#x27;, &#x27;end&#x27;);

-- 创建特殊元素表
CREATE TABLE 特殊元素 (
id INT PRIMARY KEY,
类型 VARCHAR(20),
属性 TEXT
);
INSERT INTO 特殊元素 (id, 类型, 属性) VALUES
(1, &#x27;%底层理解&#x27;, fill=#45aaf2 opacity=0.7&#x27;),
(2, &#x27;%过程箭头%&#x27;, &#x27;stroke=#45aaf2 stroke-width=2 marker-end=url(#arrowhead)&#x27;),
(3, &#x27;%缝隙%&#x27;, &#x27;stroke=#fc5c65 stroke-width=2&#x27;);

-- 创建箭头标记表
CREATE TABLE 箭头标记 (
id INT PRIMARY KEY,
标记_id VARCHAR(20),
宽度 INT,
高度 INT,
填充色 VARCHAR(7)
);
INSERT INTO 箭头标记 (id, 标记_id, 宽度, 高度, 填充色)
VALUES (1, &#x27;arrowhead&#x27;, 10, 7, &#x27;#45aaf2&#x27;);

CREATE TABLE 运行规则 (
id INT PRIMARY KEY,
规则 TEXT
);
INSERT INTO 运行规则 (id, 规则) VALUES
CALL 思维流程(&#x27;请输入想法或观点&#x27;, @结果);
SELECT @结果;
```
