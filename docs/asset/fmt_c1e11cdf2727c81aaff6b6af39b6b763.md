
# `LangGraph`接入DeepSeek模型流程

```
Collecting langgraph
  Downloading langgraph-0.4.8-py3-none-any.whl.metadata (6.8 kB)
Requirement already satisfied: langchain-core>=0.1 in /root/anaconda3/lib/python3.12/site-packages (from langgraph) (0.3.65)
Collecting langgraph-checkpoint>=2.0.26 (from langgraph)
  Downloading langgraph_checkpoint-2.1.0-py3-none-any.whl.metadata (4.2 kB)
Collecting langgraph-prebuilt>=0.2.0 (from langgraph)
  Downloading langgraph_prebuilt-0.2.2-py3-none-any.whl.metadata (4.5 kB)
Collecting langgraph-sdk>=0.1.42 (from langgraph)
  Downloading langgraph_sdk-0.1.70-py3-none-any.whl.metadata (1.5 kB)
Requirement already satisfied: pydantic>=2.7.4 in /root/anaconda3/lib/python3.12/site-packages (from langgraph) (2.11.4)
Requirement already satisfied: xxhash>=3.5.0 in /root/anaconda3/lib/python3.12/site-packages (from langgraph) (3.5.0)
Requirement already satisfied: langsmith<0.4,>=0.3.45 in /root/anaconda3/lib/python3.12/site-packages (from langchain-core>=0.1->langgraph) (0.3.45)
Requirement already satisfied: tenacity!=8.4.0,<10.0.0,>=8.1.0 in /root/anaconda3/lib/python3.12/site-packages (from langchain-core>=0.1->langgraph) (8.5.0)
Requirement already satisfied: jsonpatch<2.0,>=1.33 in /root/anaconda3/lib/python3.12/site-packages (from langchain-core>=0.1->langgraph) (1.33)
Requirement already satisfied: PyYAML>=5.3 in /root/anaconda3/lib/python3.12/site-packages (from langchain-core>=0.1->langgraph) (6.0.1)
Requirement already satisfied: packaging<25,>=23.2 in /root/anaconda3/lib/python3.12/site-packages (from langchain-core>=0.1->langgraph) (24.1)
Requirement already satisfied: typing-extensions>=4.7 in /root/anaconda3/lib/python3.12/site-packages (from langchain-core>=0.1->langgraph) (4.13.2)
Collecting ormsgpack>=1.10.0 (from langgraph-checkpoint>=2.0.26->langgraph)
  Downloading ormsgpack-1.10.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (43 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 43.7/43.7 kB 14.2 kB/s eta 0:00:00 0:00:01
Requirement already satisfied: httpx>=0.25.2 in /root/anaconda3/lib/python3.12/site-packages (from langgraph-sdk>=0.1.42->langgraph) (0.28.1)
Requirement already satisfied: orjson>=3.10.13 in /root/anaconda3/lib/python3.12/site-packages (from langgraph-sdk>=0.1.42->langgraph) (3.10.13)
Requirement already satisfied: annotated-types>=0.6.0 in /root/anaconda3/lib/python3.12/site-packages (from pydantic>=2.7.4->langgraph) (0.6.0)
Requirement already satisfied: pydantic-core==2.33.2 in /root/anaconda3/lib/python3.12/site-packages (from pydantic>=2.7.4->langgraph) (2.33.2)
Requirement already satisfied: typing-inspection>=0.4.0 in /root/anaconda3/lib/python3.12/site-packages (from pydantic>=2.7.4->langgraph) (0.4.0)
Requirement already satisfied: anyio in /root/anaconda3/lib/python3.12/site-packages (from httpx>=0.25.2->langgraph-sdk>=0.1.42->langgraph) (4.8.0)
Requirement already satisfied: certifi in /root/anaconda3/lib/python3.12/site-packages (from httpx>=0.25.2->langgraph-sdk>=0.1.42->langgraph) (2024.2.2)
Requirement already satisfied: httpcore==1.* in /root/anaconda3/lib/python3.12/site-packages (from httpx>=0.25.2->langgraph-sdk>=0.1.42->langgraph) (1.0.7)
Requirement already satisfied: idna in /root/anaconda3/lib/python3.12/site-packages (from httpx>=0.25.2->langgraph-sdk>=0.1.42->langgraph) (3.7)
Requirement already satisfied: h11<0.15,>=0.13 in /root/anaconda3/lib/python3.12/site-packages (from httpcore==1.*->httpx>=0.25.2->langgraph-sdk>=0.1.42->langgraph) (0.14.0)
Requirement already satisfied: jsonpointer>=1.9 in /root/anaconda3/lib/python3.12/site-packages (from jsonpatch<2.0,>=1.33->langchain-core>=0.1->langgraph) (2.1)
Requirement already satisfied: requests<3,>=2 in /root/anaconda3/lib/python3.12/site-packages (from langsmith<0.4,>=0.3.45->langchain-core>=0.1->langgraph) (2.32.3)
Requirement already satisfied: requests-toolbelt<2.0.0,>=1.0.0 in /root/anaconda3/lib/python3.12/site-packages (from langsmith<0.4,>=0.3.45->langchain-core>=0.1->langgraph) (1.0.0)
Requirement already satisfied: zstandard<0.24.0,>=0.23.0 in /root/anaconda3/lib/python3.12/site-packages (from langsmith<0.4,>=0.3.45->langchain-core>=0.1->langgraph) (0.23.0)
Requirement already satisfied: charset-normalizer<4,>=2 in /root/anaconda3/lib/python3.12/site-packages (from requests<3,>=2->langsmith<0.4,>=0.3.45->langchain-core>=0.1->langgraph) (2.0.4)
Requirement already satisfied: urllib3<3,>=1.21.1 in /root/anaconda3/lib/python3.12/site-packages (from requests<3,>=2->langsmith<0.4,>=0.3.45->langchain-core>=0.1->langgraph) (2.2.2)
Requirement already satisfied: sniffio>=1.1 in /root/anaconda3/lib/python3.12/site-packages (from anyio->httpx>=0.25.2->langgraph-sdk>=0.1.42->langgraph) (1.3.0)
Downloading langgraph-0.4.8-py3-none-any.whl (152 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 152.4/152.4 kB 6.7 kB/s eta 0:00:00 0:00:02
Downloading langgraph_checkpoint-2.1.0-py3-none-any.whl (43 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 43.8/43.8 kB 26.7 kB/s eta 0:00:00 0:00:01
Downloading langgraph_prebuilt-0.2.2-py3-none-any.whl (23 kB)
Downloading langgraph_sdk-0.1.70-py3-none-any.whl (49 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 50.0/50.0 kB 27.4 kB/s eta 0:00:00 0:00:01
Downloading ormsgpack-1.10.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (216 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 216.7/216.7 kB 10.7 kB/s eta 0:00:00 00:01 00:02
Installing collected packages: ormsgpack, langgraph-sdk, langgraph-checkpoint, langgraph-prebuilt, langgraph
Successfully installed langgraph-0.4.8 langgraph-checkpoint-2.1.0 langgraph-prebuilt-0.2.2 langgraph-sdk-0.1.70 ormsgpack-1.10.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
```

```
! pip show langgraph
```

```
Name: langgraph
Version: 0.4.8
Summary: Building stateful, multi-actor applications with LLMs
Home-page: 
Author: 
Author-email: 
License: 
Location: /root/anaconda3/lib/python3.12/site-packages
Requires: langchain-core, langgraph-checkpoint, langgraph-prebuilt, langgraph-sdk, pydantic, xxhash
Required-by: 
```

接下来以DeepSeek为例，介绍`LangGraph`接入大模型流程。

```
! pip install python-dotenv openai
```
