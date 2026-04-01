# 01_processdata.ipynb

> 来源：[https://v11enp9ok1h.feishu.cn/docx/PHITdSS26oLSXHxZHXKcq67gnvi](https://v11enp9ok1h.feishu.cn/docx/PHITdSS26oLSXHxZHXKcq67gnvi)

![](img/c249bd6a567d6aa51bc9ee06889dae29.png)

[code_01_processdata.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WiFRbBFFGoNULMxg0L5cjqionzc)

```
{
 &quot;cells&quot;: [
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 1,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;import numpy as np\n&quot;,
    &quot;import pandas as pd\n&quot;,
    &quot;import re\n&quot;,
    &quot;from pathlib import Path\n&quot;,
    &quot;import os\n&quot;,
    &quot;import shutil\n&quot;,
    &quot;\n&quot;,
    &quot;\n&quot;,
    &quot;def cleanquestion(x: str) -&gt; str:\n&quot;,
    &quot;    \&quot;去除文本中的符号 仅保留中文、英文、数字等\&quot;\n&quot;,
    &quot;    str_text = re.sub(\n&quot;,
    &quot;        u\&quot;([^\\u4e00-\\u9fa5\\u0030-\\u0039\\u0041-\\u005a\\u0061-\\u007a])\&quot;, \&quot;\&quot;, x)\n&quot;,
    &quot;    return str_text\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 2,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/html&quot;: [
       &quot;&lt;div&gt;\n&quot;,
       &quot;&lt;style scoped&gt;\n&quot;,
       &quot;    .dataframe tbody tr th:only-of-type {\n&quot;,
       &quot;        vertical-align: middle;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe tbody tr th {\n&quot;,
       &quot;        vertical-align: top;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe thead th {\n&quot;,
       &quot;        text-align: right;\n&quot;,
       &quot;    }\n&quot;,
       &quot;&lt;/style&gt;\n&quot;,
       &quot;&lt;table border=\&quot;1\&quot; class=\&quot;dataframe\&quot;&gt;\n&quot;,
       &quot;  &lt;thead&gt;\n&quot;,
       &quot;    &lt;tr style=\&quot;text-align: right;\&quot;&gt;\n&quot;,
       &quot;      &lt;th&gt;&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;label&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;review&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;0&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;距离川沙公路较近,但是公交指示不对,如果是\&quot;蔡陆线\&quot;的话,会非常麻烦.建议用别的路线.房间较...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;商务大床房，房间很大，床有2M宽，整体感觉经济实惠不错!&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;早餐太差，无论去多少人，那边也不加食品的。酒店应该重视一下这个问题了。房间本身很好。&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;3&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;宾馆在小街道上，不大好找，但还好北京热心同胞很多~宾馆设施跟介绍的差不多，房间很小，确实挺小...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;4&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;CBD中心,周围没什么店铺,说5星有点勉强.不知道为什么卫生间没有电吹风&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;...&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;7761&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;尼斯酒店的几大特点：噪音大、环境差、配置低、服务效率低。如：1、隔壁歌厅的声音闹至午夜3点许...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;7762&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;盐城来了很多次，第一次住盐阜宾馆，我的确很失望整个墙壁黑咕隆咚的，好像被烟熏过一样家具非常的...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;7763&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;看照片觉得还挺不错的，又是4星级的，但入住以后除了后悔没有别的，房间挺大但空空的，早餐是有但...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;7764&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;我们去盐城的时候那里的最低气温只有4度，晚上冷得要死，居然还不开空调，投诉到酒店客房部，得到...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;7765&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;说实在的我很失望，之前看了其他人的点评后觉得还可以才去的，结果让我们大跌眼镜。我想这家酒店以...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;p&gt;7765 rows × 2 columns&lt;/p&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;      label                                             review\n&quot;,
       &quot;0         1  距离川沙公路较近,但是公交指示不对,如果是\&quot;蔡陆线\&quot;的话,会非常麻烦.建议用别的路线.房间较...\n&quot;,
       &quot;1         1                       商务大床房，房间很大，床有2M宽，整体感觉经济实惠不错!\n&quot;,
       &quot;2         1         早餐太差，无论去多少人，那边也不加食品的。酒店应该重视一下这个问题了。房间本身很好。\n&quot;,
       &quot;3         1  宾馆在小街道上，不大好找，但还好北京热心同胞很多~宾馆设施跟介绍的差不多，房间很小，确实挺小...\n&quot;,
       &quot;4         1               CBD中心,周围没什么店铺,说5星有点勉强.不知道为什么卫生间没有电吹风\n&quot;,
       &quot;...     ...                                                ...\n&quot;,
       &quot;7761      0  尼斯酒店的几大特点：噪音大、环境差、配置低、服务效率低。如：1、隔壁歌厅的声音闹至午夜3点许...\n&quot;,
       &quot;7762      0  盐城来了很多次，第一次住盐阜宾馆，我的确很失望整个墙壁黑咕隆咚的，好像被烟熏过一样家具非常的...\n&quot;,
       &quot;7763      0  看照片觉得还挺不错的，又是4星级的，但入住以后除了后悔没有别的，房间挺大但空空的，早餐是有但...\n&quot;,
       &quot;7764      0  我们去盐城的时候那里的最低气温只有4度，晚上冷得要死，居然还不开空调，投诉到酒店客房部，得到...\n&quot;,
       &quot;7765      0  说实在的我很失望，之前看了其他人的点评后觉得还可以才去的，结果让我们大跌眼镜。我想这家酒店以...\n&quot;,
       &quot;\n&quot;,
       &quot;[7765 rows x 2 columns]&quot;
      ]
     },
     &quot;execution_count&quot;: 2,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;rawdata = pd.read_csv(\&quot;data_all/ChnSentiCorp_htl_all.csv\&quot;).pipe(\n&quot;,
    &quot;    lambda x: x.dropna()\n&quot;,
    &quot;).pipe(\n&quot;,
    &quot;    lambda x: x\n&quot;,
    &quot;    # lambda x: x.assign(**{\n&quot;,
    &quot;    #     &#x27;review&#x27;:x[&#x27;review&#x27;].apply(lambda j: j[:512])\n&quot;,
    &quot;    # })\n&quot;,
    &quot;)\n&quot;,
    &quot;rawdata&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 3,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/html&quot;: [
       &quot;&lt;div&gt;\n&quot;,
       &quot;&lt;style scoped&gt;\n&quot;,
       &quot;    .dataframe tbody tr th:only-of-type {\n&quot;,
       &quot;        vertical-align: middle;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe tbody tr th {\n&quot;,
       &quot;        vertical-align: top;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe thead th {\n&quot;,
       &quot;        text-align: right;\n&quot;,
       &quot;    }\n&quot;,
       &quot;&lt;/style&gt;\n&quot;,
       &quot;&lt;table border=\&quot;1\&quot; class=\&quot;dataframe\&quot;&gt;\n&quot;,
       &quot;  &lt;thead&gt;\n&quot;,
       &quot;    &lt;tr style=\&quot;text-align: right;\&quot;&gt;\n&quot;,
       &quot;      &lt;th&gt;&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;freq&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;label&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;0&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;2443&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;5322&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;       freq\n&quot;,
       &quot;label      \n&quot;,
       &quot;0      2443\n&quot;,
       &quot;1      5322&quot;
      ]
     },
     &quot;execution_count&quot;: 3,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;rawdata.groupby([&#x27;label&#x27;]).agg(\n&quot;,
    &quot;    freq = (&#x27;review&#x27;, &#x27;count&#x27;)\n&quot;,
    &quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 4,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;target_data_dir = Path(\&quot;data_all/data\&quot;)\n&quot;,
    &quot;\n&quot;,
    &quot;if target_data_dir.exists():\n&quot;,
    &quot;    shutil.rmtree(target_data_dir)\n&quot;,
    &quot;\n&quot;,
    &quot;os.makedirs(name=target_data_dir, exist_ok=True)\n&quot;,
    &quot;\n&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 5,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [
    {
     &quot;data&quot;: {
      &quot;text/html&quot;: [
       &quot;&lt;div&gt;\n&quot;,
       &quot;&lt;style scoped&gt;\n&quot;,
       &quot;    .dataframe tbody tr th:only-of-type {\n&quot;,
       &quot;        vertical-align: middle;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe tbody tr th {\n&quot;,
       &quot;        vertical-align: top;\n&quot;,
       &quot;    }\n&quot;,
       &quot;\n&quot;,
       &quot;    .dataframe thead th {\n&quot;,
       &quot;        text-align: right;\n&quot;,
       &quot;    }\n&quot;,
       &quot;&lt;/style&gt;\n&quot;,
       &quot;&lt;table border=\&quot;1\&quot; class=\&quot;dataframe\&quot;&gt;\n&quot;,
       &quot;  &lt;thead&gt;\n&quot;,
       &quot;    &lt;tr style=\&quot;text-align: right;\&quot;&gt;\n&quot;,
       &quot;      &lt;th&gt;&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;label&lt;/th&gt;\n&quot;,
       &quot;      &lt;th&gt;text&lt;/th&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/thead&gt;\n&quot;,
       &quot;  &lt;tbody&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;6620&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;这家酒店我一共住了3次,在这里不得不说说他的不好,因为位置离我要去的地方近,所以选择了这家酒...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2230&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;还不错,就是房间和我预先看的不太一样,隔音不是很好~其它还不错~根据地段和价位,性价比还不错...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2441&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;环境相当好,很适合到那里渡假,软硬件设施都不错,我住的是小套房,房假也合理,就是早餐跟其它四...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;5904&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;地点在十字路口边上，十分嘈杂，巴士车呼啦啦直到夜里11点。房间很小，洗漱用品不全：沐浴液、洗...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;3873&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;一个非常不错的太湖边酒店,我的运气不错，本来订的是一般房间，结果被升级到了行政套房.酒店拥有...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;...&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;...&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2502&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;10月17日入住，总体感觉可以，离景区较远，正好可以从西塘政治中心一路走到老镇，感受岁月变化...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;6451&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;0&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;订了商务大床间，可惜房间小的可怜！床和写字台之间太紧，基本上不能转身。更无法忍受的是房间卫生...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;1730&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;环境不错、床单毛巾洁净、服务还行、房间宽敞，但感觉茶几、桌椅、地毯、浴帘有点脏，床垫不太舒服...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;2446&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;国际五星级酒店情侣档专业酒店评价（TT）：先提一句，本来并没有订峨眉山大酒店，是从峨眉山天颐...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;    &lt;tr&gt;\n&quot;,
       &quot;      &lt;th&gt;4068&lt;/th&gt;\n&quot;,
       &quot;      &lt;td&gt;1&lt;/td&gt;\n&quot;,
       &quot;      &lt;td&gt;很好的酒店，就是不知道为什么一直在涨价，价格越来越离谱，在常州属于极贵的酒店了。是不是携程和...&lt;/td&gt;\n&quot;,
       &quot;    &lt;/tr&gt;\n&quot;,
       &quot;  &lt;/tbody&gt;\n&quot;,
       &quot;&lt;/table&gt;\n&quot;,
       &quot;&lt;p&gt;6988 rows × 2 columns&lt;/p&gt;\n&quot;,
       &quot;&lt;/div&gt;&quot;
      ],
      &quot;text/plain&quot;: [
       &quot;      label                                               text\n&quot;,
       &quot;6620      0  这家酒店我一共住了3次,在这里不得不说说他的不好,因为位置离我要去的地方近,所以选择了这家酒...\n&quot;,
       &quot;2230      1  还不错,就是房间和我预先看的不太一样,隔音不是很好~其它还不错~根据地段和价位,性价比还不错...\n&quot;,
       &quot;2441      1  环境相当好,很适合到那里渡假,软硬件设施都不错,我住的是小套房,房假也合理,就是早餐跟其它四...\n&quot;,
       &quot;5904      0  地点在十字路口边上，十分嘈杂，巴士车呼啦啦直到夜里11点。房间很小，洗漱用品不全：沐浴液、洗...\n&quot;,
       &quot;3873      1  一个非常不错的太湖边酒店,我的运气不错，本来订的是一般房间，结果被升级到了行政套房.酒店拥有...\n&quot;,
       &quot;...     ...                                                ...\n&quot;,
       &quot;2502      1  10月17日入住，总体感觉可以，离景区较远，正好可以从西塘政治中心一路走到老镇，感受岁月变化...\n&quot;,
       &quot;6451      0  订了商务大床间，可惜房间小的可怜！床和写字台之间太紧，基本上不能转身。更无法忍受的是房间卫生...\n&quot;,
       &quot;1730      1  环境不错、床单毛巾洁净、服务还行、房间宽敞，但感觉茶几、桌椅、地毯、浴帘有点脏，床垫不太舒服...\n&quot;,
       &quot;2446      1  国际五星级酒店情侣档专业酒店评价（TT）：先提一句，本来并没有订峨眉山大酒店，是从峨眉山天颐...\n&quot;,
       &quot;4068      1  很好的酒店，就是不知道为什么一直在涨价，价格越来越离谱，在常州属于极贵的酒店了。是不是携程和...\n&quot;,
       &quot;\n&quot;,
       &quot;[6988 rows x 2 columns]&quot;
      ]
     },
     &quot;execution_count&quot;: 5,
     &quot;metadata&quot;: {},
     &quot;output_type&quot;: &quot;execute_result&quot;
    }
   ],
   &quot;source&quot;: [
    &quot;rawdata.sample(frac=0.9).pipe(\n&quot;,
    &quot;    lambda x: x.rename(columns={\n&quot;,
    &quot;        &#x27;review&#x27;:&#x27;text&#x27;\n&quot;,
    &quot;    })\n&quot;,
    &quot;)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 6,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;rawdata.sample(frac=0.9).pipe(\n&quot;,
    &quot;    lambda x: x.rename(columns={\n&quot;,
    &quot;        &#x27;review&#x27;:&#x27;text&#x27;\n&quot;,
    &quot;    })\n&quot;,
    &quot;).to_csv(\&quot;data_all/data/train_data.csv\&quot;, index=False)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 7,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;rawdata.sample(frac=0.1).pipe(\n&quot;,
    &quot;    lambda x: x.rename(columns={\n&quot;,
    &quot;        &#x27;review&#x27;:&#x27;text&#x27;\n&quot;,
    &quot;    })\n&quot;,
    &quot;).to_csv(\&quot;data_all/data/test_data.csv\&quot;, index=False)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: 8,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: [
    &quot;rawdata.sample(frac=0.2).pipe(\n&quot;,
    &quot;    lambda x: x.rename(columns={\n&quot;,
    &quot;        &#x27;review&#x27;:&#x27;text&#x27;\n&quot;,
    &quot;    })\n&quot;,
    &quot;).to_csv(\&quot;data_all/data/valid_data.csv\&quot;, index=False)&quot;
   ]
  },
  {
   &quot;cell_type&quot;: &quot;code&quot;,
   &quot;execution_count&quot;: null,
   &quot;metadata&quot;: {},
   &quot;outputs&quot;: [],
   &quot;source&quot;: []
  }
 ],
 &quot;metadata&quot;: {
  &quot;kernelspec&quot;: {
   &quot;display_name&quot;: &quot;mynet2&quot;,
   &quot;language&quot;: &quot;python&quot;,
   &quot;name&quot;: &quot;python3&quot;
  },
  &quot;language_info&quot;: {
   &quot;codemirror_mode&quot;: {
    &quot;name&quot;: &quot;ipython&quot;,
    &quot;version&quot;: 3
   },
   &quot;file_extension&quot;: &quot;.py&quot;,
   &quot;mimetype&quot;: &quot;text/x-python&quot;,
   &quot;name&quot;: &quot;python&quot;,
   &quot;nbconvert_exporter&quot;: &quot;python&quot;,
   &quot;pygments_lexer&quot;: &quot;ipython3&quot;,
   &quot;version&quot;: &quot;3.10.8&quot;
  },
  &quot;orig_nbformat&quot;: 4,
  &quot;vscode&quot;: {
   &quot;interpreter&quot;: {
    &quot;hash&quot;: &quot;110bc624a448454d574a0cd6cc76359fd86f75739e493913b3d71c2e04f2ffdb&quot;
   }
  }
 },
 &quot;nbformat&quot;: 4,
 &quot;nbformat_minor&quot;: 2
}
```