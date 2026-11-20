
# Gradio 依赖包安装日志

```
Collecting gradio
  Downloading gradio-5.33.0-py3-none-any.whl.metadata (16 kB)
Collecting aiofiles<25.0,>=22.0 (from gradio)
  Using cached aiofiles-24.1.0-py3-none-any.whl.metadata (10 kB)
Requirement already satisfied: anyio<5.0,>=3.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (4.9.0)
Collecting fastapi<1.0,>=0.115.2 (from gradio)
  Using cached fastapi-0.115.12-py3-none-any.whl.metadata (27 kB)
Collecting ffmpy (from gradio)
  Using cached ffmpy-0.6.0-py3-none-any.whl.metadata (2.9 kB)
Collecting gradio-client==1.10.2 (from gradio)
  Using cached gradio_client-1.10.2-py3-none-any.whl.metadata (7.1 kB)
Collecting groovy~=0.1 (from gradio)
  Using cached groovy-0.1.2-py3-none-any.whl.metadata (6.1 kB)
Requirement already satisfied: httpx>=0.24.1 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (0.28.1)
Collecting huggingface-hub>=0.28.1 (from gradio)
  Downloading huggingface_hub-0.32.4-py3-none-any.whl.metadata (14 kB)
Collecting jinja2<4.0 (from gradio)
  Using cached jinja2-3.1.6-py3-none-any.whl.metadata (2.9 kB)
Collecting markupsafe<4.0,>=2.0 (from gradio)
  Using cached MarkupSafe-3.0.2-cp312-cp312-win_amd64.whl.metadata (4.1 kB)
Collecting numpy<3.0,>=1.0 (from gradio)
  Downloading numpy-2.3.0-cp312-cp312-win_amd64.whl.metadata (60 kB)
Requirement already satisfied: orjson~=3.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (3.10.18)
Requirement already satisfied: packaging in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (24.2)
Collecting pandas<3.0,>=1.0 (from gradio)
  Downloading pandas-2.3.0-cp312-cp312-win_amd64.whl.metadata (19 kB)
Collecting pillow<12.0,>=8.0 (from gradio)
  Using cached pillow-11.2.1-cp312-cp312-win_amd64.whl.metadata (9.1 kB)
Requirement already satisfied: pydantic<2.12,>=2.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (2.11.5)
Collecting pydub (from gradio)
  Using cached pydub-0.25.1-py2.py3-none-any.whl.metadata (1.4 kB)
Collecting python-multipart>=0.0.18 (from gradio)
  Using cached python_multipart-0.0.20-py3-none-any.whl.metadata (1.8 kB)
Requirement already satisfied: pyyaml<7.0,>=5.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (6.0.2)
Collecting ruff>=0.9.3 (from gradio)
  Downloading ruff-0.11.13-py3-none-win_amd64.whl.metadata (26 kB)
Collecting safehttpx<0.2.0,>=0.1.6 (from gradio)
  Using cached safehttpx-0.1.6-py3-none-any.whl.metadata (4.2 kB)
Collecting semantic-version~=2.0 (from gradio)
  Using cached semantic_version-2.10.0-py2.py3-none-any.whl.metadata (9.7 kB)
Collecting starlette<1.0,>=0.40.0 (from gradio)
  Downloading starlette-0.47.0-py3-none-any.whl.metadata (6.2 kB)
Collecting tomlkit<0.14.0,>=0.12.0 (from gradio)
  Downloading tomlkit-0.13.3-py3-none-any.whl.metadata (2.8 kB)
Collecting typer<1.0,>=0.12 (from gradio)
  Using cached typer-0.16.0-py3-none-any.whl.metadata (15 kB)
Requirement already satisfied: typing-extensions~=4.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from gradio) (4.14.0)
Collecting uvicorn>=0.14.0 (from gradio)
  Downloading uvicorn-0.34.3-py3-none-any.whl.metadata (6.5 kB)
Collecting fsspec (from gradio-client==1.10.2->gradio)
  Using cached fsspec-2025.5.1-py3-none-any.whl.metadata (11 kB)
Collecting websockets<16.0,>=10.0 (from gradio-client==1.10.2->gradio)
  Using cached websockets-15.0.1-cp312-cp312-win_amd64.whl.metadata (7.0 kB)
Requirement already satisfied: idna>=2.8 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from anyio<5.0,>=3.0->gradio) (3.10)
Requirement already satisfied: sniffio>=1.1 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from anyio<5.0,>=3.0->gradio) (1.3.1)
Collecting starlette<1.0,>=0.40.0 (from gradio)
  Using cached starlette-0.46.2-py3-none-any.whl.metadata (6.2 kB)
Requirement already satisfied: python-dateutil>=2.8.2 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from pandas<3.0,>=1.0->gradio) (2.9.0.post0)
Collecting pytz>=2020.1 (from pandas<3.0,>=1.0->gradio)
  Using cached pytz-2025.2-py2.py3-none-any.whl.metadata (22 kB)
Collecting tzdata>=2022.7 (from pandas<3.0,>=1.0->gradio)
  Using cached tzdata-2025.2-py2.py3-none-any.whl.metadata (1.4 kB)
Requirement already satisfied: annotated-types>=0.6.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from pydantic<2.12,>=2.0->gradio) (0.7.0)
Requirement already satisfied: pydantic-core==2.33.2 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from pydantic<2.12,>=2.0->gradio) (2.33.2)
Requirement already satisfied: typing-inspection>=0.4.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from pydantic<2.12,>=2.0->gradio) (0.4.1)
Collecting click>=8.0.0 (from typer<1.0,>=0.12->gradio)
  Using cached click-8.2.1-py3-none-any.whl.metadata (2.5 kB)
Collecting shellingham>=1.3.0 (from typer<1.0,>=0.12->gradio)
  Using cached shellingham-1.5.4-py2.py3-none-any.whl.metadata (3.5 kB)
Collecting rich>=10.11.0 (from typer<1.0,>=0.12->gradio)
  Using cached rich-14.0.0-py3-none-any.whl.metadata (18 kB)
Requirement already satisfied: colorama in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from click>=8.0.0->typer<1.0,>=0.12->gradio) (0.4.6)
Requirement already satisfied: certifi in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from httpx>=0.24.1->gradio) (2025.4.26)
Requirement already satisfied: httpcore==1.* in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from httpx>=0.24.1->gradio) (1.0.9)
Requirement already satisfied: h11>=0.16 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from httpcore==1.*->httpx>=0.24.1->gradio) (0.16.0)
Collecting filelock (from huggingface-hub>=0.28.1->gradio)
  Using cached filelock-3.18.0-py3-none-any.whl.metadata (2.9 kB)
Requirement already satisfied: requests in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from huggingface-hub>=0.28.1->gradio) (2.32.3)
Requirement already satisfied: tqdm>=4.42.1 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from huggingface-hub>=0.28.1->gradio) (4.67.1)
Requirement already satisfied: six>=1.5 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from python-dateutil>=2.8.2->pandas<3.0,>=1.0->gradio) (1.17.0)
Collecting markdown-it-py>=2.2.0 (from rich>=10.11.0->typer<1.0,>=0.12->gradio)
  Using cached markdown_it_py-3.0.0-py3-none-any.whl.metadata (6.9 kB)
Requirement already satisfied: pygments<3.0.0,>=2.13.0 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from rich>=10.11.0->typer<1.0,>=0.12->gradio) (2.19.1)
Collecting mdurl~=0.1 (from markdown-it-py>=2.2.0->rich>=10.11.0->typer<1.0,>=0.12->gradio)
  Using cached mdurl-0.1.2-py3-none-any.whl.metadata (1.6 kB)
Requirement already satisfied: charset-normalizer<4,>=2 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from requests->huggingface-hub>=0.28.1->gradio) (3.4.2)
Requirement already satisfied: urllib3<3,>=1.21.1 in e:\01_木羽研发\11_trafficvideo\langchain_venv\lib\site-packages (from requests->huggingface-hub>=0.28.1->gradio) (2.4.0)
Downloading gradio-5.33.0-py3-none-any.whl (54.2 MB)
   ---------------------------------------- 0.0/54.2 MB ? eta -:--:--
   ---------------------------------------- 0.0/54.2 MB ? eta -:--:--
   ---------------------------------------- 0.5/54.2 MB 2.4 MB/s eta 0:00:23
   - -------------------------------------- 1.6/54.2 MB 4.0 MB/s eta 0:00:14
   --- ------------------------------------ 4.5/54.2 MB 7.9 MB/s eta 0:00:07
   ------ --------------------------------- 8.4/54.2 MB 11.3 MB/s eta 0:00:05
   --------- ------------------------------ 13.1/54.2 MB 13.7 MB/s eta 0:00:04
   ------------ --------------------------- 17.3/54.2 MB 14.7 MB/s eta 0:00:03
   --------------- ------------------------ 21.5/54.2 MB 15.4 MB/s eta 0:00:03
   ------------------- -------------------- 26.0/54.2 MB 16.3 MB/s eta 0:00:02
   ---------------------- ----------------- 30.4/54.2 MB 16.9 MB/s eta 0:00:02
   ------------------------- -------------- 34.9/54.2 MB 17.3 MB/s eta 0:00:02
   ---------------------------- ----------- 39.1/54.2 MB 17.7 MB/s eta 0:00:01
   -------------------------------- ------- 43.5/54.2 MB 18.0 MB/s eta 0:00:01
   ----------------------------------- ---- 48.2/54.2 MB 18.3 MB/s eta 0:00:01
   -------------------------------------- - 52.4/54.2 MB 18.3 MB/s eta 0:00:01
   ---------------------------------------- 54.2/54.2 MB 18.1 MB/s eta 0:00:00
Using cached gradio_client-1.10.2-py3-none-any.whl (323 kB)
Using cached aiofiles-24.1.0-py3-none-any.whl (15 kB)
Using cached fastapi-0.115.12-py3-none-any.whl (95 kB)
Using cached groovy-0.1.2-py3-none-any.whl (14 kB)
Using cached jinja2-3.1.6-py3-none-any.whl (134 kB)
Using cached MarkupSafe-3.0.2-cp312-cp312-win_amd64.whl (15 kB)
Downloading numpy-2.3.0-cp312-cp312-win_amd64.whl (12.7 MB)
   ---------------------------------------- 0.0/12.7 MB ? eta -:--:--
   ------------------ --------------------- 6.0/12.7 MB 28.4 MB/s eta 0:00:01
   ---------------------------- ----------- 9.2/12.7 MB 24.8 MB/s eta 0:00:01
   ---------------------------------------- 12.7/12.7 MB 20.5 MB/s eta 0:00:00
Downloading pandas-2.3.0-cp312-cp312-win_amd64.whl (11.0 MB)
   ---------------------------------------- 0.0/11.0 MB ? eta -:--:--
   ------------------ --------------------- 5.0/11.0 MB 25.1 MB/s eta 0:00:01
   --------------------------------- ------ 9.2/11.0 MB 24.8 MB/s eta 0:00:01
   ---------------------------------------- 11.0/11.0 MB 21.4 MB/s eta 0:00:00
Using cached pillow-11.2.1-cp312-cp312-win_amd64.whl (2.7 MB)
Using cached safehttpx-0.1.6-py3-none-any.whl (8.7 kB)
Using cached semantic_version-2.10.0-py2.py3-none-any.whl (15 kB)
Using cached starlette-0.46.2-py3-none-any.whl (72 kB)
Downloading tomlkit-0.13.3-py3-none-any.whl (38 kB)
Using cached typer-0.16.0-py3-none-any.whl (46 kB)
Using cached websockets-15.0.1-py3-none-any.whl (176 kB)
Using cached click-8.2.1-py3-none-any.whl (102 kB)
Downloading huggingface_hub-0.32.4-py3-none-any.whl (512 kB)
Using cached fsspec-2025.5.1-py3-none-any.whl (199 kB)
Using cached python_multipart-0.0.20-py3-none-any.whl (24 kB)
Using cached pytz-2025.2-py2.py3-none-any.whl (509 kB)
Using cached rich-14.0.0-py3-none-any.whl (243 kB)
Using cached markdown_it_py-3.0.0-py3-none-any.whl (87 kB)
Using cached mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Downloading ruff-0.11.13-py3-none-win_amd64.whl (11.5 MB)
   ---------------------------------------- 0.0/11.5 MB ? eta -:--:--
   -------------------- ------------------- 6.0/11.5 MB 28.4 MB/s eta 0:00:01
   ------------------------------- -------- 9.2/11.5 MB 25.9 MB/s eta 0:00:01
   ---------------------------------------- 11.5/11.5 MB 22.6 MB/s eta 0:00:00
Using cached shellingham-1.5.4-py2.py3-none-any.whl (9.8 kB)
Using cached tzdata-2025.2-py2.py3-none-any.whl (347 kB)
Downloading uvicorn-0.34.3-py3-none-any.whl (62 kB)
Using cached ffmpy-0.6.0-py3-none-any.whl (5.5 kB)
Using cached filelock-3.18.0-py3-none-any.whl (16 kB)
Using cached pydub-0.25.1-py2.py3-none-any.whl (32 kB)
Installing collected packages: pytz, pydub, websockets, tzdata, tomlkit, shellingham, semantic-version, ruff, python-multipart, pillow, numpy, mdurl, markupsafe, groovy, fsspec, filelock, ffmpy, click, aiofiles, uvicorn, starlette, pandas, markdown-it-py, jinja2, huggingface-hub, safehttpx, rich, gradio-client, fastapi, typer, gradio

   ----------------------------------------  0/31 [pytz]
   --- ------------------------------------  3/31 [tzdata]
   --- ------------------------------------  3/31 [tzdata]
   --------- ------------------------------  7/31 [ruff]
   ----------- ----------------------------  9/31 [pillow]
   ----------- ----------------------------  9/31 [pillow]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ------------ --------------------------- 10/31 [numpy]
   ---------------- ----------------------- 13/31 [groovy]
   ------------------ --------------------- 14/31 [fsspec]
   ------------------------ --------------- 19/31 [uvicorn]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   --------------------------- ------------ 21/31 [pandas]
   ---------------------------- ----------- 22/31 [markdown-it-py]
   ------------------------------ --------- 24/31 [huggingface-hub]
   ------------------------------ --------- 24/31 [huggingface-hub]
   -------------------------------- ------- 25/31 [safehttpx]
   --------------------------------- ------ 26/31 [rich]
   ------------------------------------ --- 28/31 [fastapi]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   -------------------------------------- - 30/31 [gradio]
   ---------------------------------------- 31/31 [gradio]

Successfully installed aiofiles-24.1.0 click-8.2.1 fastapi-0.115.12 ffmpy-0.6.0 filelock-3.18.0 fsspec-2025.5.1 gradio-5.33.0 gradio-client-1.10.2 groovy-0.1.2 huggingface-hub-0.32.4 jinja2-3.1.6 markdown-it-py-3.0.0 markupsafe-3.0.2 mdurl-0.1.2 numpy-2.3.0 pandas-2.3.0 pillow-11.2.1 pydub-0.25.1 python-multipart-0.0.20 pytz-2025.2 rich-14.0.0 ruff-0.11.13 safehttpx-0.1.6 semantic-version-2.10.0 shellingham-1.5.4 starlette-0.46.2 tomlkit-0.13.3 typer-0.16.0 tzdata-2025.2 uvicorn-0.34.3 websockets-15.0.1
```
