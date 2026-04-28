# 交互式问答或多功能对话助手的代码框架

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/WSZudyUtgog4auxkGM6cj4p3nhh](https://qqs7y1hozd1.feishu.cn/docx/WSZudyUtgog4auxkGM6cj4p3nhh)

```
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
        &quot;&quot;&quot;Initialize session state variables.&quot;&quot;&quot;
        st.session_state[&#x27;assistant&#x27;] = []
        st.session_state[&#x27;user&#x27;] = []

        action_list = [
            WeatherQuery(),
        ]
        st.session_state[&#x27;plugin_map&#x27;] = {
            action.name: action
            for action in action_list
        }
        st.session_state[&#x27;model_map&#x27;] = {}
        st.session_state[&#x27;model_selected&#x27;] = None
        st.session_state[&#x27;plugin_actions&#x27;] = set()
        st.session_state[&#x27;history&#x27;] = []

    def clear_state(self):
        &quot;&quot;&quot;Clear the existing session state.&quot;&quot;&quot;
        st.session_state[&#x27;assistant&#x27;] = []
        st.session_state[&#x27;user&#x27;] = []
        st.session_state[&#x27;model_selected&#x27;] = None
        st.session_state[&#x27;file&#x27;] = set()
        if &#x27;chatbot&#x27; in st.session_state:
            st.session_state[&#x27;chatbot&#x27;]._session_history = []


class StreamlitUI:

    def __init__(self, session_state: SessionState):
        self.init_streamlit()
        self.session_state = session_state

    def init_streamlit(self):
        &quot;&quot;&quot;Initialize Streamlit&#x27;s UI settings.&quot;&quot;&quot;
        st.set_page_config(
            layout=&#x27;wide&#x27;,
            page_title=&#x27;lagent-web&#x27;,
            page_icon=&#x27;./docs/imgs/lagent_icon.png&#x27;)
        st.header(&#x27;:robot_face: :blue[Lagent] Web Demo &#x27;, divider=&#x27;rainbow&#x27;)
        st.sidebar.title(&#x27;模型控制&#x27;)
        st.session_state[&#x27;file&#x27;] = set()
        st.session_state[&#x27;ip&#x27;] = None

    def setup_sidebar(self):
        &quot;&quot;&quot;Setup the sidebar for model and plugin selection.&quot;&quot;&quot;
        # model_name = st.sidebar.selectbox(&#x27;模型选择：&#x27;, options=[&#x27;internlm&#x27;])
        model_name = st.sidebar.text_input(&#x27;模型名称：&#x27;, value=&#x27;internlm2-chat-7b&#x27;)
        meta_prompt = st.sidebar.text_area(&#x27;系统提示词&#x27;, value=META_CN)
        da_prompt = st.sidebar.text_area(&#x27;数据分析提示词&#x27;, value=INTERPRETER_CN)
        plugin_prompt = st.sidebar.text_area(&#x27;插件提示词&#x27;, value=PLUGIN_CN)
        model_ip = st.sidebar.text_input(&#x27;模型IP：&#x27;, value=&#x27;10.140.0.220:23333&#x27;)
        if model_name != st.session_state[
                &#x27;model_selected&#x27;] or st.session_state[&#x27;ip&#x27;] != model_ip:
            st.session_state[&#x27;ip&#x27;] = model_ip
            model = self.init_model(model_name, model_ip)
            self.session_state.clear_state()
            st.session_state[&#x27;model_selected&#x27;] = model_name
            if &#x27;chatbot&#x27; in st.session_state:
                del st.session_state[&#x27;chatbot&#x27;]
        else:
            model = st.session_state[&#x27;model_map&#x27;][model_name]

        plugin_name = st.sidebar.multiselect(
            &#x27;插件选择&#x27;,
            options=list(st.session_state[&#x27;plugin_map&#x27;].keys()),
            default=[],
        )
        da_flag = st.sidebar.checkbox(
            &#x27;数据分析&#x27;,
            value=False,
        )
        plugin_action = [
            st.session_state[&#x27;plugin_map&#x27;][name] for name in plugin_name
        ]

        if &#x27;chatbot&#x27; in st.session_state:
            if len(plugin_action) &gt; 0:
                st.session_state[&#x27;chatbot&#x27;]._action_executor = ActionExecutor(
                    actions=plugin_action)
            else:
                st.session_state[&#x27;chatbot&#x27;]._action_executor = None
            if da_flag:
                st.session_state[
                    &#x27;chatbot&#x27;]._interpreter_executor = ActionExecutor(
                        actions=[IPythonInterpreter()])
            else:
                st.session_state[&#x27;chatbot&#x27;]._interpreter_executor = None
            st.session_state[&#x27;chatbot&#x27;]._protocol._meta_template = meta_prompt
            st.session_state[&#x27;chatbot&#x27;]._protocol.plugin_prompt = plugin_prompt
            st.session_state[
                &#x27;chatbot&#x27;]._protocol.interpreter_prompt = da_prompt
        if st.sidebar.button(&#x27;清空对话&#x27;, key=&#x27;clear&#x27;):
            self.session_state.clear_state()
        uploaded_file = st.sidebar.file_uploader(&#x27;上传文件&#x27;)

        return model_name, model, plugin_action, uploaded_file, model_ip

    def init_model(self, model_name, ip=None):
        &quot;&quot;&quot;Initialize the model based on the input model name.&quot;&quot;&quot;
        model_url = f&#x27;http://{ip}&#x27;
        st.session_state[&#x27;model_map&#x27;][model_name] = LMDeployClient(
            model_name=model_name,
            url=model_url,
            meta_template=META,
            max_new_tokens=1024,
            top_p=0.8,
            top_k=100,
            temperature=0,
            repetition_penalty=1.0,
            stop_words=[&#x27;&lt;|im_end|&gt;&#x27;])
        return st.session_state[&#x27;model_map&#x27;][model_name]

    def initialize_chatbot(self, model, plugin_action):
        &quot;&quot;&quot;Initialize the chatbot with the given model and plugin actions.&quot;&quot;&quot;
        return Internlm2Agent(
            llm=model,
            protocol=Internlm2Protocol(
                tool=dict(
                    begin=&#x27;{start_token}{name}\n&#x27;,
                    start_token=&#x27;&lt;|action_start|&gt;&#x27;,
                    name_map=dict(
                        plugin=&#x27;&lt;|plugin|&gt;&#x27;, interpreter=&#x27;&lt;|interpreter|&gt;&#x27;),
                    belong=&#x27;assistant&#x27;,
                    end=&#x27;&lt;|action_end|&gt;\n&#x27;,
                ), ),
            max_turn=7)

    def render_user(self, prompt: str):
        with st.chat_message(&#x27;user&#x27;):
            st.markdown(prompt)

    def render_assistant(self, agent_return):
        with st.chat_message(&#x27;assistant&#x27;):
            for action in agent_return.actions:
                if (action) and (action.type != &#x27;FinishAction&#x27;):
                    self.render_action(action)
            st.markdown(agent_return.response)

    def render_plugin_args(self, action):
        action_name = action.type
        args = action.args
        import json
        parameter_dict = dict(name=action_name, parameters=args)
        parameter_str = &#x27;```json\n&#x27; + json.dumps(
            parameter_dict, indent=4, ensure_ascii=False) + &#x27;\n```&#x27;
        st.markdown(parameter_str)

    def render_interpreter_args(self, action):
        st.info(action.type)
        st.markdown(action.args[&#x27;text&#x27;])

    def render_action(self, action):
        st.markdown(action.thought)
        if action.type == &#x27;IPythonInterpreter&#x27;:
            self.render_interpreter_args(action)
        elif action.type == &#x27;FinishAction&#x27;:
            pass
        else:
            self.render_plugin_args(action)
        self.render_action_results(action)

    def render_action_results(self, action):
        &quot;&quot;&quot;Render the results of action, including text, images, videos, and
        audios.&quot;&quot;&quot;
        if (isinstance(action.result, dict)):
            if &#x27;text&#x27; in action.result:
                st.markdown(&#x27;```\n&#x27; + action.result[&#x27;text&#x27;] + &#x27;\n```&#x27;)
            if &#x27;image&#x27; in action.result:
                # image_path = action.result[&#x27;image&#x27;]
                for image_path in action.result[&#x27;image&#x27;]:
                    image_data = open(image_path, &#x27;rb&#x27;).read()
                    st.image(image_data, caption=&#x27;Generated Image&#x27;)
            if &#x27;video&#x27; in action.result:
                video_data = action.result[&#x27;video&#x27;]
                video_data = open(video_data, &#x27;rb&#x27;).read()
                st.video(video_data)
            if &#x27;audio&#x27; in action.result:
                audio_data = action.result[&#x27;audio&#x27;]
                audio_data = open(audio_data, &#x27;rb&#x27;).read()
                st.audio(audio_data)
        elif isinstance(action.result, list):
            for item in action.result:
                if item[&#x27;type&#x27;] == &#x27;text&#x27;:
                    st.markdown(&#x27;```\n&#x27; + item[&#x27;content&#x27;] + &#x27;\n```&#x27;)
                elif item[&#x27;type&#x27;] == &#x27;image&#x27;:
                    image_data = open(item[&#x27;content&#x27;], &#x27;rb&#x27;).read()
                    st.image(image_data, caption=&#x27;Generated Image&#x27;)
                elif item[&#x27;type&#x27;] == &#x27;video&#x27;:
                    video_data = open(item[&#x27;content&#x27;], &#x27;rb&#x27;).read()
                    st.video(video_data)
                elif item[&#x27;type&#x27;] == &#x27;audio&#x27;:
                    audio_data = open(item[&#x27;content&#x27;], &#x27;rb&#x27;).read()
                    st.audio(audio_data)
        if action.errmsg:
            st.error(action.errmsg)


def main():
    # logger = get_logger(__name__)
    # Initialize Streamlit UI and setup sidebar
    if &#x27;ui&#x27; not in st.session_state:
        session_state = SessionState()
        session_state.init_state()
        st.session_state[&#x27;ui&#x27;] = StreamlitUI(session_state)

    else:
        st.set_page_config(
            layout=&#x27;wide&#x27;,
            page_title=&#x27;lagent-web&#x27;,
            page_icon=&#x27;./docs/imgs/lagent_icon.png&#x27;)
        st.header(&#x27;:robot_face: :blue[Lagent] Web Demo &#x27;, divider=&#x27;rainbow&#x27;)
    _, model, plugin_action, uploaded_file, _ = st.session_state[
        &#x27;ui&#x27;].setup_sidebar()

    # Initialize chatbot if it is not already initialized
    # or if the model has changed
    if &#x27;chatbot&#x27; not in st.session_state or model != st.session_state[
            &#x27;chatbot&#x27;]._llm:
        st.session_state[&#x27;chatbot&#x27;] = st.session_state[
            &#x27;ui&#x27;].initialize_chatbot(model, plugin_action)
        st.session_state[&#x27;session_history&#x27;] = []

    for prompt, agent_return in zip(st.session_state[&#x27;user&#x27;],
                                    st.session_state[&#x27;assistant&#x27;]):
        st.session_state[&#x27;ui&#x27;].render_user(prompt)
        st.session_state[&#x27;ui&#x27;].render_assistant(agent_return)

    if user_input := st.chat_input(&#x27;&#x27;):
        with st.container():
            st.session_state[&#x27;ui&#x27;].render_user(user_input)
        st.session_state[&#x27;user&#x27;].append(user_input)
        # Add file uploader to sidebar
        if (uploaded_file
                and uploaded_file.name not in st.session_state[&#x27;file&#x27;]):

            st.session_state[&#x27;file&#x27;].add(uploaded_file.name)
            file_bytes = uploaded_file.read()
            file_type = uploaded_file.type
            if &#x27;image&#x27; in file_type:
                st.image(file_bytes, caption=&#x27;Uploaded Image&#x27;)
            elif &#x27;video&#x27; in file_type:
                st.video(file_bytes, caption=&#x27;Uploaded Video&#x27;)
            elif &#x27;audio&#x27; in file_type:
                st.audio(file_bytes, caption=&#x27;Uploaded Audio&#x27;)
            # Save the file to a temporary location and get the path

            postfix = uploaded_file.name.split(&#x27;.&#x27;)[-1]
            # prefix = str(uuid.uuid4())
            prefix = hashlib.md5(file_bytes).hexdigest()
            filename = f&#x27;{prefix}.{postfix}&#x27;
            file_path = os.path.join(root_dir, filename)
            with open(file_path, &#x27;wb&#x27;) as tmpfile:
                tmpfile.write(file_bytes)
            file_size = os.stat(file_path).st_size / 1024 / 1024
            file_size = f&#x27;{round(file_size, 2)} MB&#x27;
            # st.write(f&#x27;File saved at: {file_path}&#x27;)
            user_input = [
                dict(role=&#x27;user&#x27;, content=user_input),
                dict(
                    role=&#x27;user&#x27;,
                    content=json.dumps(dict(path=file_path, size=file_size)),
                    name=&#x27;file&#x27;)
            ]
        if isinstance(user_input, str):
            user_input = [dict(role=&#x27;user&#x27;, content=user_input)]
        st.session_state[&#x27;last_status&#x27;] = AgentStatusCode.SESSION_READY
        for agent_return in st.session_state[&#x27;chatbot&#x27;].stream_chat(
                st.session_state[&#x27;session_history&#x27;] + user_input):
            if agent_return.state == AgentStatusCode.PLUGIN_RETURN:
                with st.container():
                    st.session_state[&#x27;ui&#x27;].render_plugin_args(
                        agent_return.actions[-1])
                    st.session_state[&#x27;ui&#x27;].render_action_results(
                        agent_return.actions[-1])
            elif agent_return.state == AgentStatusCode.CODE_RETURN:
                with st.container():
                    st.session_state[&#x27;ui&#x27;].render_action_results(
                        agent_return.actions[-1])
            elif (agent_return.state == AgentStatusCode.STREAM_ING
                  or agent_return.state == AgentStatusCode.CODING):
                # st.markdown(agent_return.response)
                # 清除占位符的当前内容，并显示新内容
                with st.container():
                    if agent_return.state != st.session_state[&#x27;last_status&#x27;]:
                        st.session_state[&#x27;temp&#x27;] = &#x27;&#x27;
                        placeholder = st.empty()
                        st.session_state[&#x27;placeholder&#x27;] = placeholder
                    if isinstance(agent_return.response, dict):
                        action = f&quot;\n\n {agent_return.response[&#x27;name&#x27;]}: \n\n&quot;
                        action_input = agent_return.response[&#x27;parameters&#x27;]
                        if agent_return.response[
                                &#x27;name&#x27;] == &#x27;IPythonInterpreter&#x27;:
                            action_input = action_input[&#x27;command&#x27;]
                        response = action + action_input
                    else:
                        response = agent_return.response
                    st.session_state[&#x27;temp&#x27;] = response
                    st.session_state[&#x27;placeholder&#x27;].markdown(
                        st.session_state[&#x27;temp&#x27;])
            elif agent_return.state == AgentStatusCode.END:
                st.session_state[&#x27;session_history&#x27;] += (
                    user_input + agent_return.inner_steps)
                agent_return = copy.deepcopy(agent_return)
                agent_return.response = st.session_state[&#x27;temp&#x27;]
                st.session_state[&#x27;assistant&#x27;].append(
                    copy.deepcopy(agent_return))
            st.session_state[&#x27;last_status&#x27;] = agent_return.state


if __name__ == &#x27;__main__&#x27;:
    root_dir = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    root_dir = os.path.join(root_dir, &#x27;tmp_dir&#x27;)
    os.makedirs(root_dir, exist_ok=True)
    main()
```