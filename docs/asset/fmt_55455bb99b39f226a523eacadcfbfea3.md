
```
import hvac

class VaultManager:
    def __init__(self, url='http://localhost:8200', token=None):
        self.client = hvac.Client(url=url, token=token)
    
    def store_secret(self, path, secret_data):
        """存储密钥"""
        self.client.secrets.kv.v2.create_or_update_secret(
            path=path,
            secret=secret_data
        )
    
    def retrieve_secret(self, path):
        """检索密钥"""
        response = self.client.secrets.kv.v2.read_secret_version(path=path)
        return response['data']['data']
    
    def delete_secret(self, path):
        """删除密钥"""
        self.client.secrets.kv.v2.delete_metadata_and_all_versions(path=path)

# 使用示例
vault = VaultManager(token='your-vault-token')

# 存储密钥
vault.store_secret(
    'openclaw/api-keys',
    {
        'openai_key': 'sk-xxx',
        'anthropic_key': 'sk-ant-xxx'
    }
)

# 检索密钥
keys = vault.retrieve_secret('openclaw/api-keys')
```

### 环境变量最佳实践

[如果使用环境变量](https://nhimg.org/api-key-rotation-best-practices/)，遵循这些原则：

```
# ✅ 好的做法
# 1. 使用专门的 .env 文件,不要提交到版本控制
echo ".env" >> .gitignore

# 2. 使用前缀区分不同环境
PROD_OPENAI_API_KEY=sk-xxx
DEV_OPENAI_API_KEY=sk-xxx

# 3. 设置严格的文件权限
chmod 600 .env

# 4. 使用加密工具
# 安装 git-crypt
git-crypt init
echo ".env filter=git-crypt diff=git-crypt" >> .gitattributes
git-crypt add-gpg-user your@email.com

# ❌ 避免的做法
# 1. 不要硬编码在代码中
api_key = "sk-xxx"  # 危险!

# 2. 不要在命令行中暴露
curl -H "Authorization: Bearer sk-xxx" https://api.openai.com  # 会记录在历史中

# 3. 不要在日志中打印
print(f"Using API key: {api_key}")  # 危险!
```

### 密钥加密存储

如果必须在本地存储密钥，确保加密：

```
from cryptography.fernet import Fernet
import os
import json

class EncryptedKeyStore:
    def __init__(self, key_file='.keystore.enc'):
        self.key_file = key_file
        self.master_key_file = '.master.key'
        
        # 加载或生成主密钥
        if os.path.exists(self.master_key_file):
            with open(self.master_key_file, 'rb') as f:
                self.master_key = f.read()
        else:
            self.master_key = Fernet.generate_key()
            with open(self.master_key_file, 'wb') as f:
                f.write(self.master_key)
            os.chmod(self.master_key_file, 0o600)
        
        self.cipher = Fernet(self.master_key)
    
    def store_keys(self, keys_dict):
        """加密存储密钥"""
        json_data = json.dumps(keys_dict)
        encrypted_data = self.cipher.encrypt(json_data.encode())
        
        with open(self.key_file, 'wb') as f:
            f.write(encrypted_data)
        
        os.chmod(self.key_file, 0o600)
    
    def load_keys(self):
        """加载并解密密钥"""
        if not os.path.exists(self.key_file):
            return {}
        
        with open(self.key_file, 'rb') as f:
            encrypted_data = f.read()
        
        decrypted_data = self.cipher.decrypt(encrypted_data)
        return json.loads(decrypted_data.decode())
    
    def add_key(self, key_name, key_value):
        """添加新密钥"""
        keys = self.load_keys()
        keys[key_name] = key_value
        self.store_keys(keys)
    
    def get_key(self, key_name):
        """获取密钥"""
        keys = self.load_keys()
        return keys.get(key_name)
    
    def delete_key(self, key_name):
        """删除密钥"""
        keys = self.load_keys()
        if key_name in keys:
            del keys[key_name]
            self.store_keys(keys)

# 使用示例
keystore = EncryptedKeyStore()

# 存储密钥
keystore.add_key('openai_api_key', 'sk-xxx')
keystore.add_key('anthropic_api_key', 'sk-ant-xxx')

# 获取密钥
openai_key = keystore.get_key('openai_api_key')
print(f"OpenAI Key: {openai_key[:10]}...")  # 只显示前缀
```

### 13.4.2 密钥轮换策略

[定期轮换 API](https://help.openai.com/en/articles/5112595-best-practices-for-api-key-safety) 密钥是关键的安全实践。

#### 自动化密钥轮换
