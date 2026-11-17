
```python
import os
import json
import requests
from pydantic import BaseModel, Field
from typing import List, Union, Iterator

# Set DEBUG to True to enable detailed logging
DEBUG = False

class Pipe:
    class Valves(BaseModel):
        openai_API_KEY: str = Field(default="none")  # Optional API key if needed
        DEFAULT_MODEL: str = Field(default="DeepSeek-R1")  # Default model identifier

    def __init__(self):
        self.id = "DeepSeek-R1"
        self.type = "manifold"
        self.name = "KT: "
        self.valves = self.Valves(
            **{
                "openai_API_KEY": os.getenv("openai_API_KEY", "none"),
                "DEFAULT_MODEL": os.getenv("openai_DEFAULT_MODEL", "DeepSeek-R1"),
            }
        )
        # Self-hosted FastAPI server details
        self.api_url = "http://localhost:8000/v1/chat/completions"  # FastAPI server endpoint
        self.headers = {"Content-Type": "application/json"}

    def get_openai_models(self):
        """Return available models - for openai we'll return a fixed list"""
        return [{"id": "KT", "name": "DeepSeek-R1"}]

    def pipes(self) -> List[dict]:
        return self.get_openai_models()

    def pipe(self, body: dict) -> Union[str, Iterator[str]]:
        try:
            # Use default model ID since openai has a single endpoint
            model_id = self.valves.DEFAULT_MODEL

            messages = []

            # Process messages including system, user, and assistant messages
            for message in body["messages"]:
                if isinstance(message.get("content"), list):
                    # For openai, we'll join multiple content parts into a single text
                    text_parts = []
                    for content in message["content"]:
                        if content["type"] == "text":
                            text_parts.append(content["text"])
                        elif content["type"] == "image_url":
                            # openai might not support image inputs - add a note about the image
                            text_parts.append(f"[Image: {content['image_url']['url']}]")
                    messages.append(
                        {"role": message["role"], "content": " ".join(text_parts)}
                    )
                else:
                    # Handle simple text messages
                    messages.append(
                        {"role": message["role"], "content": message["content"]}
                    )

            if DEBUG:
                print("FastAPI API request:")
                print("  Model:", model_id)
                print("  Messages:", json.dumps(messages, indent=2))

            # Prepare the API call parameters
            payload = {
                "model": model_id,
                "messages": messages,
                "temperature": body.get("temperature", 0.7),
                "top_p": body.get("top_p", 0.9),
                "max_tokens": body.get("max_tokens", 8192),
                "stream": body.get("stream", True),
            }

            # Add stop sequences if provided
            if body.get("stop"):
                payload["stop"] = body["stop"]

            # Sending request to local FastAPI server
            if body.get("stream", False):
                # Streaming response
                def stream_generator():
                    try:
                        response = requests.post(self.api_url, json=payload, headers=self.headers, stream=True)
                        for line in response.iter_lines():
                            if line:
                                yield line.decode("utf-8")
                    except Exception as e:
                        if DEBUG:
                            print(f"Streaming error: {e}")
                        yield f"Error during streaming: {str(e)}"

                return stream_generator()
            else:
                # Regular response
                response = requests.post(self.api_url, json=payload, headers=self.headers)
                if response.status_code == 200:
                    generated_content = response.json().get("choices", [{}])[0].get("message", {}).get("content", "")
                    return generated_content
                else:
                    return f"Error: {response.status_code}, {response.text}"

        except Exception as e:
            if DEBUG:
                print(f"Error in pipe method: {e}")
            return f"Error: {e}"

    def health_check(self) -> bool:
        """Check if the openai API (local FastAPI service) is accessible"""
        try:
            # Simple health check with a basic prompt
            response = requests.post(
                self.api_url,
                json={
                    "model": self.valves.DEFAULT_MODEL,
                    "messages": [{"role": "user", "content": "Hello"}],
                    "max_tokens": 5,
                },
                headers=self.headers,
            )
            return response.status_code == 200
        except Exception as e:
            if DEBUG:
                print(f"Health check failed: {e}")
            return False
```

即可进行调用了：

![](img/c2c36affc4797b5f9ed903c4dbd9abeb.png)

实际对话效果如下：

![](img/ea2e5522c4c57aa393e24b9dac103637.png)
