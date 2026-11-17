
[最简单的方法是启](https://slashdot.org/content/how-to-protect-your-data-from-openclaw-security-vulnerabilities/)用操作系统的磁盘加密。citation

#### Linux (LUKS):

```
# 加密 OpenClaw 数据目录
sudo cryptsetup luksFormat /dev/sdX
sudo cryptsetup open /dev/sdX openclaw_encrypted

# 创建文件系统
sudo mkfs.ext4 /dev/mapper/openclaw_encrypted

# 挂载
sudo mount /dev/mapper/openclaw_encrypted ~/.openclaw
```

#### macOS (FileVault):

```
# 启用 FileVault
sudo fdesetup enable

# 或为特定目录创建加密磁盘映像
hdiutil create -size 10g -encryption AES-256 -volname "OpenClaw" openclaw.dmg
hdiutil attach openclaw.dmg
```

#### Windows (BitLocker):

```
# 启用 BitLocker
Enable-BitLocker -MountPoint "C:" -EncryptionMethod Aes256
```

### 应用级加密

对敏感文件进行单独加密：

```
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2
import base64
import os

class SecureStorage:
    def __init__(self, password, salt=None):
        """初始化加密存储"""
        if salt is None:
            salt = os.urandom(16)
        
        # 从密码派生密钥
        kdf = PBKDF2(
            algorithm=hashes.SHA256(),
            length=32,
            salt=salt,
            iterations=100000,
        )
        key = base64.urlsafe_b64encode(kdf.derive(password.encode()))
        
        self.cipher = Fernet(key)
        self.salt = salt
    
    def encrypt_file(self, input_path, output_path):
        """加密文件"""
        with open(input_path, 'rb') as f:
            data = f.read()
        
        encrypted_data = self.cipher.encrypt(data)
        
        with open(output_path, 'wb') as f:
            # 写入盐值和加密数据
            f.write(self.salt)
            f.write(encrypted_data)
    
    def decrypt_file(self, input_path, output_path):
        """解密文件"""
        with open(input_path, 'rb') as f:
            # 读取盐值和加密数据
            salt = f.read(16)
            encrypted_data = f.read()
        
        decrypted_data = self.cipher.decrypt(encrypted_data)
        
        with open(output_path, 'wb') as f:
            f.write(decrypted_data)

# 使用示例
storage = SecureStorage("your-strong-password")
storage.encrypt_file(
    "~/.openclaw/workspace/sensitive.json",
    "~/.openclaw/workspace/sensitive.json.enc"
)
```

### 环境变量加密

OpenClaw 鼓励在 `.env`[ 文件中存储 A](https://atomicmail.io/blog/using-openclaw-ai-safely-full-privacy-security-guide)PI 密钥，但这些文件通常是明文。citation

#### 解决方案：

```
# 1. 使用加密的环境变量管理器
# 安装 sops (Secrets OPerationS)
brew install sops

# 创建加密的 .env 文件
sops -e .env > .env.enc

# 使用时解密
sops -d .env.enc > .env.tmp
source .env.tmp
rm .env.tmp

# 2. 使用密钥管理服务
# AWS Secrets Manager
aws secretsmanager get-secret-value --secret-id openclaw/api-keys

# HashiCorp Vault
vault kv get secret/openclaw/api-keys

# 3. 使用加密脚本
cat > load_secrets.sh << 'EOF'
#!/bin/bash
# 从加密文件加载密钥
gpg --decrypt ~/.openclaw/secrets.gpg | while IFS='=' read -r key value; do
    export "$key=$value"
done
EOF

chmod +x load_secrets.sh
source load_secrets.sh
```

### 数据库加密

如果 OpenClaw 连接到数据库，确保数据加密：

```
# SQLite 加密示例
from sqlcipher3 import dbapi2 as sqlite

# 创建加密数据库
conn = sqlite.connect('openclaw.db')
conn.execute("PRAGMA key='your-encryption-key'")
conn.execute("PRAGMA cipher='aes-256-cbc'")

# 创建表
conn.execute('''
    CREATE TABLE IF NOT EXISTS secrets (
        id INTEGER PRIMARY KEY,
        key TEXT NOT NULL,
        value TEXT NOT NULL,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    )
''')

# 插入加密数据
conn.execute("INSERT INTO secrets (key, value) VALUES (?, ?)", 
             ("API_KEY", "sk-xxx"))
conn.commit()
```

### 13.2.3 传输安全保障

保护数据在传输过程中的安全同样重要。

#### 强制 HTTPS

OpenClaw Gateway 默[认使用 HTTP](https://docs.openclaw.ai/gateway/security)，必须配置 HTTPS.citation

```
# ~/.openclaw/openclaw.yaml
gateway:
  port: 443
  tls:
    enabled: true
    cert: /path/to/cert.pem
    key: /path/to/key.pem
```

#### 生成自签名证书 （开发环境）:

```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

#### 使用 Let‘s Encrypt （生产环境）:

```
# 安装 certbot
sudo apt install certbot

# 获取证书
sudo certbot certonly --standalone -d your-domain.com

# 配置自动续期
sudo certbot renew --dry-run
```

#### API 请求加密

确保所有 API 调用使用 TLS:

```
import requests

# ✅ 正确: 使用 HTTPS
response = requests.get(
    "https://api.example.com/data",
    headers={"Authorization": f"Bearer {api_key}"},
    verify=True  # 验证 SSL 证书
)

# ❌ 错误: 使用 HTTP (不安全)
response = requests.get(
    "http://api.example.com/data",
    headers={"Authorization": f"Bearer {api_key}"}
)

# ❌ 错误: 禁用证书验证 (不安全)
response = requests.get(
    "https://api.example.com/data",
    verify=False  # 危险!
)
```

#### 端到端加密

对于高度敏感的数据，实施端到端加密：

```
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import serialization, hashes

class E2EEncryption:
    def __init__(self):
        # 生成密钥对
        self.private_key = rsa.generate_private_key(
            public_exponent=65537,
            key_size=2048
        )
        self.public_key = self.private_key.public_key()
    
    def encrypt_message(self, message, recipient_public_key):
        """使用接收方的公钥加密消息"""
        encrypted = recipient_public_key.encrypt(
            message.encode(),
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        return encrypted
    
    def decrypt_message(self, encrypted_message):
        """使用私钥解密消息"""
        decrypted = self.private_key.decrypt(
            encrypted_message,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        return decrypted.decode()

# 使用示例
alice = E2EEncryption()
bob = E2EEncryption()

# Alice 发送加密消息给 Bob
message = "这是机密信息"
encrypted = alice.encrypt_message(message, bob.public_key)

# Bob 解密消息
decrypted = bob.decrypt_message(encrypted)
print(decrypted)
```

#### VPN 和网络隔离

对于敏感部署，使用 VPN 限制访问：

```
# 使用 WireGuard 创建 VPN
# 服务器配置
cat > /etc/wireguard/wg0.conf << EOF
[Interface]
PrivateKey = <server_private_key>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <client_public_key>
AllowedIPs = 10.0.0.2/32
EOF

# 启动 VPN
wg-quick up wg0

# 配置防火墙,只允许 VPN 访问 OpenClaw
sudo ufw allow 51820/udp
sudo ufw deny 18789  # OpenClaw Gateway 端口
sudo ufw allow from 10.0.0.0/24 to any port 18789
```

`</client_public_key></server_private_key>`

## 13.3 访问控制与权限管理

### 13.3.1 身份认证机制

强大的身份认证是安全的基础。

#### 多因素认证 (MFA)

为 OpenClaw 访问添加 MFA:
