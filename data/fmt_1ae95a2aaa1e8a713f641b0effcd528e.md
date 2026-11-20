
```python
import json
import os
import requests
from typing import Optional, Type

from lagent.actions.base_action import BaseAction, tool_api
from lagent.actions.parser import BaseParser, JsonParser
from lagent.schema import ActionReturn, ActionStatusCode

class WeatherQuery(BaseAction):
    """Weather plugin for querying weather information."""
    
    def __init__(self,
                 key: Optional[str] = None,
                 description: Optional[dict] = None,
                 parser: Type[BaseParser] = JsonParser,
                 enable: bool = True) -> None:
        super().__init__(description, parser, enable)
        key = os.environ.get('WEATHER_API_KEY', key)
        if key is None:
            raise ValueError(
                'Please set Weather API key either in the environment '
                'as WEATHER_API_KEY or pass it as `key`')
        self.key = key
        self.location_query_url = 'https://geoapi.qweather.com/v2/city/lookup'
        self.weather_query_url = 'https://devapi.qweather.com/v7/weather/now'

    @tool_api
    def run(self, query: str) -> ActionReturn:
        """一个天气查询API。可以根据城市名查询天气信息.
        
        Args:
            query (:class:`str`): The city name to query.
        """
        tool_return = ActionReturn(type=self.name)
        status_code, response = self._search(query)
        if status_code == -1:
            tool_return.errmsg = response
            tool_return.state = ActionStatusCode.HTTP_ERROR
        elif status_code == 200:
            parsed_res = self._parse_results(response)
            tool_return.result = [dict(type='text', content=str(parsed_res))]
            tool_return.state = ActionStatusCode.SUCCESS
        else:
            tool_return.errmsg = str(status_code)
            tool_return.state = ActionStatusCode.API_ERROR
        return tool_return
    
    def _parse_results(self, results: dict) -> str:
        """Parse the weather results from QWeather API.
        
        Args:
            results (dict): The weather content from QWeather API
                in json format.
        
        Returns:
            str: The parsed weather results.
        """
        now = results['now']
        data = [
            f'数据观测时间: {now["obsTime"]}',
            f'温度: {now["temp"]}°C',
            f'体感温度: {now["feelsLike"]}°C',
            f'天气: {now["text"]}',
            f'风向: {now["windDir"]}，角度为 {now["wind360"]}°',
            f'风力等级: {now["windScale"]}，风速为 {now["windSpeed"]} km/h',
            f'相对湿度: {now["humidity"]}',
            f'当前小时累计降水量: {now["precip"]} mm',
            f'大气压强: {now["pressure"]} 百帕',
            f'能见度: {now["vis"]} km',
        ]
        return '\n'.join(data)

    def _search(self, query: str):
        # get city_code
        try:
            city_code_response = requests.get(
                self.location_query_url,
                params={'key': self.key, 'location': query}
            )
        except Exception as e:
            return -1, str(e)
        if city_code_response.status_code != 200:
            return city_code_response.status_code, city_code_response.json()
        city_code_response = city_code_response.json()
        if len(city_code_response['location']) == 0:
            return -1, '未查询到城市'
        city_code = city_code_response['location'][0]['id']
        # get weather
        try:
            weather_response = requests.get(
                self.weather_query_url,
                params={'key': self.key, 'location': city_code}
            )
        except Exception as e:
            return -1, str(e)
        return weather_response.status_code, weather_response.json()
```

![](img/fdfc8c0b86cf65abfb6c891de9415b02.png)

## 获取 API KEY

为了获得稳定的天气查询服务，我们首先要获取 API KEY。首先打开 `https://dev.qweather.com/docs/api/` 后，点击右上角控制台。（如下图所示），这里需要我们先注册“和风天气”的账号和密码。当然有用户的直接登录点击控制台即可

![](img/92e3d7092fffb126e700859fc704b0bf.png)

## 体验自定义工具效果

与 前面构建`Lagent` web demo 部分类似，我们在两个 `terminal` 中分别启动 `LMDeploy` 服务和 Tutorial 已经写好的用于这部分的 Web Demo：

Important

注意，确保 1.1 节中的 `LMDeploy` 服务以及 1.2 节中的 Web Demo 服务已经停止（即 `terminal` 已关闭），否则会出现 CUDA Out of Memory 或是端口已占用的情况！

```
conda activate agent
lmdeploy serve api_server /root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-7b \
                            --server-name 127.0.0.1 \
                            --model-name internlm2-chat-7b \
                            --cache-max-entry-count 0.1
```

```
export WEATHER_API_KEY=在2.2节获取的API KEY
# 比如 export WEATHER_API_KEY=1234567890abcdef
conda activate agent
cd /root/agent/Tutorial/agent
streamlit run internlm2_weather_web_demo.py --server.address 127.0.0.1 --server.port 7860
```

![](img/bc749899d177a49746e92a70324b15dc.png)

并在本地执行如下操作以进行端口映射：

```
ssh -CNg -L 7860:127.0.0.1:7860 -L 23333:127.0.0.1:23333 root@ssh.intern-ai.org.cn -p 你的 ssh 端口号
```

在输入模型地址并选择好工具后，就可以开始体验了。下图是一个例子：

![](img/934cc7f34dfe84973e28cc0efc453227.png)

![](img/26566c1d4a8e283a2e1864914d166265.png)

## 用 AgentLego 组装智能体“乐高” 自定义工具

前面我们已经使用 `agent` 搭建 web demo 和天气接口的使用了，接下来我们就直接使用 `AgentLego`：

```
wget http://download.openmmlab.com/agentlego/road.jpg
```

```
# 下载demo文件
cd /root/agent
# 安装目标检测基于mmdet算法库中的RTMDet-Large模型依赖
conda activate agent
pip install openmim==0.3.9
mim install mmdet==3.3.0
# 新建使用目标检测工具的代码文件
touch /root/agent/direct_use.py
```

![](img/5aefeb89a48874de9fc08f0140da12b7.png)

![](img/5e167a121d03cd7d14de0c0ac7235ea6.png)

`direct_use.py` 代码如下：

```python
import re

import cv2
from agentlego.apis import load_tool

# load tool
tool = load_tool('ObjectDetection', device='cuda')

# apply tool
visualization = tool('/root/agent/road.jpg')
print(visualization)

# visualize
image = cv2.imread('/root/agent/road.jpg')

preds = visualization.split('\n')
pattern = r'(\w+) \((\d+), (\d+), (\d+), (\d+)\), score (\d+)'

for pred in preds:
    name, x1, y1, x2, y2, score = re.match(pattern, pred).groups()
    x1, y1, x2, y2, score = int(x1), int(y1), int(x2), int(y2), int(score)
    cv2.rectangle(image, (x1, y1), (x2, y2), (0, 255, 0), 1)
    cv2.putText(image, f'{name} {score}', (x1, y1), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 1)

cv2.imwrite('/root/agent/road_detection_direct.jpg', image)
```

![](img/7a7e325a6b1ba67f73316675d38e8a64.png)

接下来我们使用目标检测工具

```
python /root/agent/direct_use.py
```

![](img/33507e70faec6b50d1d777c802026487.png)

## 作为智能体工具使用

### 修改相关文件

由于 `AgentLego` 算法库默认使用 `InternLM2-Chat-20B` 模型，因此我们首先需要修改 `/root/agent/agentlego/webui/modules/agents/lagent_agent.py` 文件的第 105 行位置，将 `internlm2-chat-20b` 修改为 `internlm2-chat-7b`，即

```python
def llm_internlm2_lmdeploy(cfg):
    url = cfg['url'].strip()
    llm = LMDeployClient(
-         model_name='internlm2-chat-20b',
+         model_name='internlm2-chat-7b',
        url=url,
        meta_template=INTERNLM2_META,
        top_p=0.8,
        top_k=100,
        temperature=cfg.get('temperature', 0.7),
        repetition_penalty=1.0,
        stop_words=['<|im_end|>'])
    return llm
```

其中红色表示要删除的内容，绿色表示要添加的内容

![](img/fc9fb30ea5c010dbd25acc8a1ea7a8e0.png)

### 使用 LMDeploy 部署

由于 `AgentLego` 的 WebUI 需要用到 `LMDeploy` 所启动的 `api_server`，因此我们首先按照下图指示在 `vscode` `terminal` 中执行如下代码使用 `LMDeploy` 启动一个 `api_server`
