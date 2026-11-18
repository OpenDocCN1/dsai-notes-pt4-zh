
# 交互式问答或多功能对话助手的代码框架

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/WSZudyUtgog4auxkGM6cj4p3nhh](https://qqs7y1hozd1.feishu.cn/docx/WSZudyUtgog4auxkGM6cj4p3nhh)

```python
import copy
import hashlib
import json
import os

import streamlit as st

from lagent.actions import ActionExecutor, IPythonInterpreter
from lagent.actions.weather import WeatherQuery
from lagent.agents.internlm2_agent import INTERPRETER_CN, META_CN, PLUGIN_CN, Internlm2Agent, Internlm2Protocol
from lagent.llms.lmdepoly_wrapper import LMDeployClient
from lagent.llms.meta_template import INTERNLM2_META as META
from lagent.schema import AgentStatusCode

# from streamlit.logger import get_logger


class SessionState:

    def init_state(self):
        """Initialize session state variables."""
        st.session_state['assistant'] = []
        st.session_state['user'] = []

        action_list = [
            WeatherQuery(),
        ]
        st.session_state['plugin_map'] = {
            action.name: action
            for action in action_list
        }
        st.session_state['model_map'] = {}
        st.session_state['model_selected'] = None
        st.session_state['plugin_actions'] = set()
        st.session_state['history'] = []

    def clear_state(self):
        """Clear the existing session state."""
        st.session_state['assistant'] = []
        st.session_state['user'] = []
        st.session_state['model_selected'] = None
        st.session_state['file'] = set()
        if 'chatbot' in st.session_state:
            st.session_state['chatbot']._session_history = []


class StreamlitUI:

    def __init__(self, session_state: SessionState):
        self.init_streamlit()
        self.session_state = session_state

    def init_streamlit(self):
        """Initialize Streamlit's UI settings."""
        st.set_page_config(
            layout='wide',
            page_title='lagent-web',
            page_icon='./docs/imgs/lagent_icon.png')
        st.header(':robot_face: :blue[Lagent] Web Demo ', divider='rainbow')
        st.sidebar.title('模型控制')
        st.session_state['file'] = set()
        st.session_state['ip'] = None

    def setup_sidebar(self):
        """Setup the sidebar for model and plugin selection."""
        # model_name = st.sidebar.selectbox('模型选择：', options=['internlm'])
        model_name = st.sidebar.text_input('模型名称：', value='internlm2-chat-7b')
        meta_prompt = st.sidebar.text_area('系统提示词', value=META_CN)
        da_prompt = st.sidebar.text_area('数据分析提示词', value=INTERPRETER_CN)
        plugin_prompt = st.sidebar.text_area('插件提示词', value=PLUGIN_CN)
        model_ip = st.sidebar.text_input('模型IP：', value='10.140.0.220:23333')
        if model_name != st.session_state[
                'model_selected'] or st.session_state['ip'] != model_ip:
            st.session_state['ip'] = model_ip
            model = self.init_model(model_name, model_ip)
            self.session_state.clear_state()
            st.session_state['model_selected'] = model_name
            if 'chatbot' in st.session_state:
                del st.session_state['chatbot']
        else:
            model = st.session_state['model_map'][model_name]

        plugin_name = st.sidebar.multiselect(
            '插件选择',
            options=list(st.session_state['plugin_map'].keys()),
            default=[],
        )
        da_flag = st.sidebar.checkbox(
            '数据分析',
            value=False,
        )
        plugin_action = [
            st.session_state['plugin_map'][name] for name in plugin_name
        ]

        if 'chatbot' in st.session_state:
            if len(plugin_action) > 0:
                st.session_state['chatbot']._action_executor = ActionExecutor(
                    actions=plugin_action)
            else:
                st.session_state['chatbot']._action_executor = None
            if da_flag:
                st.session_state[
                    'chatbot'] ._interpreter_executor = ActionExecutor(
                        actions=[IPythonInterpreter()])
            else:
                st.session_state['chatbot']._interpreter_executor = None
            st.session_state['chatbot']._protocol._meta_template = meta_prompt
            st.session_state['chatbot']._protocol.plugin_prompt = plugin_prompt
            st.session_state[
                'chatbot']._protocol.interpreter_prompt = da_prompt
        if st.sidebar.button('清空对话', key='clear'):
            self.session_state.clear_state()
        uploaded_file = st.sidebar.file_uploader('上传文件')

        return model_name, model, plugin_action, uploaded_file, model_ip

    def init_model(self, model_name, ip=None):
        """Initialize the model based on the input model name."""
        model_url = f'http://{ip}'
        st.session_state['model_map'][model_name] = LMDeployClient(
            model_name=model_name,
            url=model_url,
            meta_template=META,
            max_new_tokens=1024,
            top_p=0.8,
            top_k=100,
            temperature=0,
            repetition_penalty=1.0,
            stop_words=['<|im_end|>'])
        return st.session_state['model_map'][model_name]

    def initialize_chatbot(self, model, plugin_action):
        """Initialize the chatbot with the given model and plugin actions."""
        return Internlm2Agent(
            llm=model,
            protocol=Internlm2Protocol(
                tool=dict(
                    begin='{start_token}{name}\n',
                    start_token='<|action_start|>',
                    name_map=dict(
                        plugin='<|plugin|>', interpreter='<|interpreter|>'),
                    belong='assistant',
                    end='<|action_end|>\n',
                ), ),
            max_turn=7)

    def render_user(self, prompt: str):
        with st.chat_message('user'):
            st.markdown(prompt)

    def render_assistant(self, agent_return):
        with st.chat_message('assistant'):
            for action in agent_return.actions:
                if (action) and (action.type != 'FinishAction'):
                    self.render_action(action)
            st.markdown(agent_return.response)

    def render_plugin_args(self, action):
        action_name = action.type
        args = action.args
        import json
        parameter_dict = dict(name=action_name, parameters=args)
        parameter_str = '```json\n' + json.dum
```
