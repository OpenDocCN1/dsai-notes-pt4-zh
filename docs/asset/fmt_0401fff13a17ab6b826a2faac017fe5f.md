
```python
import hmac
import base64
import urllib.parse
from dingtalk_stream import AckMessage
import dingtalk_stream

class DingTalkAdapter(ChannelAdapter):
    """钉钉渠道适配器"""
    
    def __init__(self, app_key: str, app_secret: str, robot_code: str):
        self.app_key = app_key
        self.app_secret = app_secret
        self.robot_code = robot_code
        self.client = dingtalk_stream.DingTalkStreamClient(app_key, app_secret)
        self._setup_handlers()
    
    def _setup_handlers(self):
        """设置消息处理器"""
        @self.client.register_callback_handler(dingtalk_stream.ChatbotMessage.TOPIC)
        async def process_message(message: dingtalk_stream.ChatbotMessage):
            """处理钉钉消息"""
            
            # 解析消息内容
            content = message.text.content if message.text else ""
            
            user_message = UserMessage(
                message_id=message.msg_id,
                user_id=message.sender_id,
                channel=ChannelType.DINGTALK,
                content=content,
                message_type="text",
                timestamp=message.create_at / 1000,  # 转换为秒
                session_id=""
            )
            
            # 调用智能客服处理
            agent_response = await self._process_with_agent(user_message)
            
            # 发送响应
            await self.send_response(agent_response, message.conversation_id)
            
            return AckMessage.STATUS_OK, 'OK'
    
    async def _process_with_agent(self, user_message: UserMessage) -> AgentResponse:
        """使用智能客服处理消息"""
        # 这里会调用主要的Agent处理逻辑
        # 暂时返回模拟响应
        return AgentResponse(
            response_id=str(uuid.uuid4()),
            content="正在为您处理，请稍候...",
            message_type="text"
        )
    
    async def send_response(self, response: AgentResponse, conversation_id: str) -> bool:
        """发送钉钉响应"""
        try:
            await self.client.send_message(
                conversation_id=conversation_id,
                content=response.content
            )
            return True
        except Exception as e:
            print(f"❌ 发送钉钉消息失败: {e}")
            return False
    
    def get_channel_type(self) -> ChannelType:
        return ChannelType.DINGTALK
    
    def start_listening(self):
        """开始监听消息"""
        self.client.start_forever()
```

### 3.3 渠道统一管理

![](`img/e8c5f124eaca80fddcef3a5edacff783.png`)

```python
class ChannelManager:
    """渠道管理器"""
    
    def __init__(self):
        self.adapters: Dict[ChannelType, ChannelAdapter] = {}
        self.message_queue = asyncio.Queue()
        self.session_manager = SessionManager(redis_client)
        self.agent = IntelligentCustomerServiceAgent()
    
    def register_adapter(self, adapter: ChannelAdapter):
        """注册渠道适配器"""
        channel_type = adapter.get_channel_type()
        self.adapters[channel_type] = adapter
        print(f"✅ 注册渠道适配器: {channel_type.value}")
    
    async def route_message(self, message: UserMessage) -> AgentResponse:
        """路由消息到对应处理器"""
        try:
            # 获取或创建会话
            session = await self.session_manager.get_session(
                message.user_id, 
                message.channel
            )
            message.session_id = session["session_id"]
            
            # 调用智能客服Agent处理
            response = await self.agent.process_message(message, session)
            
            # 更新会话状态
            await self.session_manager.update_session(session, message, response)
            
            return response
            
        except Exception as e:
            print(f"❌ 消息路由处理失败: {e}")
            return AgentResponse(
                response_id=str(uuid.uuid4()),
                content="抱歉，系统暂时出现问题，请稍后再试。",
                message_type="text"
            )
    
    async def start_all_adapters(self):
        """启动所有渠道适配器"""
        tasks = []
        for adapter in self.adapters.values():
            if hasattr(adapter, 'start_listening'):
                tasks.append(asyncio.create_task(adapter.start_listening()))
        
        if tasks:
            await asyncio.gather(*tasks)
```

## 智能客服Agent核心实现

### 4.1 意图识别与分类
