
# 本地源码启动

> 来源：https://xmgtoagi.feishu.cn/docx/F5xEd7G35oajnyxwC93cyyCcnVm

下载 PostgreSQL 源码：从官网（`https://www.postgresql.org/ftp/source/`）下载 PostgreSQL 源码

解压了 PostgreSQL 源码到 `/wljt/pgsql/postgresql-15.0` 目录，接下来可以按照以下步骤进行编译、安装和部署：

好的，以下是基于你提供的源码包路径 `/opt/postgresql-15.0.tar.gz`，在 Ubuntu 20.04 下离线安装 PostgreSQL 15.0 的完整步骤：

## 安装步骤

### 1. 解压源码包

将下载的源码包解压到指定目录：

```
tar -zxf /opt/postgresql-15.0.tar.gz -C /opt
```

### 2. 安装编译依赖

安装编译 PostgreSQL 所需的依赖库：

```
sudo apt update
sudo apt install make gcc tar pkg-config libicu-dev libreadline-dev zlib1g-dev
```

### 3. 配置编译选项

进入解压后的源码目录，执行配置脚本：

```
cd /opt/postgresql-15.0
sudo ./configure --prefix=/opt/postgresql-15.0
```

### 4. 编译并安装

运行以下命令进行编译和安装：

```
sudo make
sudo make install
```

### 5. 创建数据目录和日志目录

创建 PostgreSQL 的数据目录和日志目录：

```
sudo mkdir -p /opt/postgresql-15.0/data
sudo mkdir -p /opt/postgresql-15.0/logs
```

### 6. 创建 PostgreSQL 用户

为了安全起见，建议为 PostgreSQL 创建一个专用用户：

```
sudo adduser postgres
```

### 7. 授权数据目录和日志目录

将数据目录和日志目录的所有权授予 PostgreSQL 用户：

```
sudo chown -R postgres:postgres /opt/postgresql-15.0/data
sudo chown -R postgres:postgres /opt/postgresql-15.0/logs
```

### 8. 初始化数据库

切换到 PostgreSQL 用户，然后初始化数据库：

```
su - postgres
/opt/postgresql-15.0/bin/initdb -D /opt/postgresql-15.0/data
```

### 9. 配置 PostgreSQL

#### 修改 `postgresql.conf` 文件

编辑 PostgreSQL 的主配置文件 `postgresql.conf`：

```
nano /opt/postgresql-15.0/data/postgresql.conf
```

根据需要修改配置项，比如：

```
listen_addresses = '*'          # 允许所有地址连接
port = 5432                     # 默认端口号
timezone = 'Asia/Shanghai'      # 设置时区
log_directory = '/opt/postgresql-15.0/logs'  # 日志目录
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'  # 日志文件名格式
```

#### 修改 `pg_hba.conf` 文件

编辑客户端认证配置文件 `pg_hba.conf`，设置客户端连接的认证方式：

```
nano /opt/postgresql-15.0/data/pg_hba.conf
```

例如，允许本地无需密码连接，允许远程连接需密码：

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
# IPv4 remote connections:
host    all             all             0.0.0.0/0               scram-sha-256
```

### 10. 启动 PostgreSQL 服务

#### 创建 systemd 服务单元文件

为了方便管理 PostgreSQL 服务，可以创建一个 systemd 服务单元文件：

```
sudo nano /etc/systemd/system/postgresql.service
```

添加以下内容：

```
[Unit]
Description=PostgreSQL
After=network-online.target

[Service]
Type=forking
WorkingDirectory=/opt/postgresql-15.0/bin
User=postgres
ExecStart=/opt/postgresql-15.0/bin/pg_ctl -D /opt/postgresql-15.0/data start
ExecStop=/opt/postgresql-15.0/bin/pg_ctl -D /opt/postgresql-15.0/data stop
Restart=always
RestartSec=40s
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=postgresql
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

#### 启动并管理服务

让 systemd 重新加载配置文件，然后启动 PostgreSQL 服务：

```
sudo systemctl daemon-reload
sudo systemctl start postgresql
sudo systemctl enable postgresql  # 设置开机自启
sudo systemctl status postgresql  # 查看服务状态
```

### 11. 测试连接

切换到 PostgreSQL 用户，尝试连接到数据库：

```
su - postgres
/opt/postgresql-15.0/bin/psql
```

如果能够成功进入 PostgreSQL 的交互界面，说明安装和配置成功。

### 12. 添加用户和授权

在 PostgreSQL 中创建新用户并授权：

```
CREATE USER root WITH PASSWORD '123456';
GRANT CONNECT ON DATABASE postgres TO root;
ALTER USER root WITH SUPERUSER;
```

### 13. 使用命令行连接测试

使用命令行工具连接到 PostgreSQL 数据库：

```
/opt/postgresql-15.0/bin/psql -h localhost -p 5432 -U root -d postgres -W 123456
```

### 14. 可选：部署 pgAdmin4

如果需要图形化管理工具，可以部署 pgAdmin4：

```
sudo docker pull dpage/pgadmin4
sudo docker run -d -p 8081:8081 -e PGADMIN_DEFAULT_EMAIL=test@your.com -e PGADMIN_DEFAULT_PASSWORD=123456 dpage/pgadmin4
```

然后在浏览器访问 `http://服务器IP:8081`，使用设置的邮箱和密码登录。

以上步骤完成了 PostgreSQL 的离线部署和基本配置，你可以根据实际需求进一步调整和优化配置。

## 如何设置 PostgreSQL 的用户名和密码

在 PostgreSQL 中，可以通过以下步骤设置用户名和密码：

### 1. 创建新用户

在 PostgreSQL 中，可以使用 `CREATE USER` 命令来创建新用户。以下是创建用户的语法：

```
CREATE USER username WITH PASSWORD 'password';
```

### 2. 授权用户

创建用户后，需要授予该用户对特定数据库或表的访问权限。以下是授予用户权限的语法：

```
GRANT CONNECT ON DATABASE dbname TO username;
```

### 3. 设置用户为超级用户（可选）

如果需要将用户设置为超级用户，可以使用以下命令：

```
ALTER USER username WITH SUPERUSER;
```

### 示例

假设你想创建一个名为 `root` 的用户，并设置密码为 `123456`，并授予其对 `test` 数据库的访问权限：

1. 切换到 PostgreSQL 交互界面：

```
su - postgres
/opt/postgresql-15.0/bin/psql
```

1. 创建用户：

```
CREATE USER root WITH PASSWORD '123456';
```

1. 授权用户：

```
GRANT CONNECT ON DATABASE test TO root;
```

1. 设置用户为超级用户（可选）：

```
ALTER USER root WITH SUPERUSER;
```

### 4. 使用新用户连接到数据库

创建并授权用户后，可以使用新用户连接到数据库：

```
/opt/postgresql-15.0/bin/psql -h localhost -p 5432 -U root -d test
```

### 5. 修改现有用户的密码

如果需要修改现有用户的密码，可以使用以下命令：

```
ALTER USER username WITH PASSWORD 'newpassword';
```

### 6. 删除用户

如果需要删除用户，可以使用以下命令：

```
DROP USER username;
```

通过以上步骤，你可以轻松管理 PostgreSQL 中的用户和权限。

# 虚拟环境

根据你提供的信息，看起来你在尝试激活虚拟环境时遇到了问题。错误信息 `-bash: ./venv/Scripts/activate: No such file or directory` 表明 Bash 找不到你指定的激活脚本。这可能是因为路径不正确或者虚拟环境没有正确创建。

## 问题分析

1. **路径问题**：在 Linux 系统中，虚拟环境的激活脚本通常位于 `venv/bin/activate`，而不是 `venv/Scripts/activate`。`venv/Scripts/activate` 是 Windows 系统下的路径。

1. **虚拟环境未正确创建**：虽然你提到 `/wljt/dify/api/venv` 目录存在，但可能虚拟环境没有正确创建，导致激活脚本缺失。

## 解决步骤

### 1. 确认虚拟环境路径

确保你当前所在的目录是 `/wljt/dify/api`，并且虚拟环境确实存在于该目录下：

```
cd /wljt/dify/api
ls -l venv
```

### 2. 使用正确的激活命令

在 Linux 系统中，激活虚拟环境的命令应该是：

```
source venv/bin/activate
```

或者

```
. venv/bin/activate
```

### 3. 重新创建虚拟环境（如果必要）

如果虚拟环境没有正确创建，可以尝试重新创建：

```
cd /wljt/dify/api
python -m venv venv
source venv/bin/activate
```

### 4. 检查虚拟环境是否完整

如果重新创建虚拟环境后仍然无法激活，检查虚拟环境目录是否完整：

```
ls venv/bin
```

你应该能看到 `activate` 脚本和其他相关文件。

### 5. 使用绝对路径

如果仍然无法激活，尝试使用绝对路径：

```
source /wljt/dify/api/venv/bin/activate
```

### 示例

假设你的当前目录是 `/wljt/dify/api`，以下是完整的操作步骤：

```
cd /wljt/dify/api
ls -l venv  # 确认 venv 目录存在
source venv/bin/activate  # 激活虚拟环境
```

## 其他注意事项

+ 确保你使用的是正确的 Python 版本来创建虚拟环境。
+ 如果你使用的是 Python 3，确保命令是 `python3 -m venv venv`。
+ 如果你仍然遇到问题，可以尝试删除并重新创建虚拟环境。

通过以上步骤，你应该能够成功激活虚拟环境并继续部署 dify。如果问题仍然存在，请提供更多详细信息以便进一步诊断。

## 连接 PostgreSQL 并设置密码

既然你已经成功安装了 PostgreSQL 15 客户端，并且可以使用 `psql` 命令，现在可以按照以下步骤连接到 PostgreSQL 数据库并设置密码：

### 1. 连接到 PostgreSQL 数据库

使用以下命令连接到 PostgreSQL 数据库：

```
psql -U postgres -h localhost -p 5432
```

### 2. 设置 PostgreSQL 用户密码

在连接到 PostgreSQL 数据库后，你可以使用以下 SQL 命令来设置 `postgres` 用户的密码：

```
ALTER USER postgres WITH ENCRYPTED PASSWORD 'your_new_password';
```

将 `your_new_password` 替换为你想要设置的密码。

### 3. 验证密码设置

设置密码后，你可以退出并重新连接以验证密码是否生效：

```
\q
psql -U postgres -h localhost -p 5432 -W
```

在提示时输入你刚刚设置的密码。

通过以上步骤，你应该能够成功设置 PostgreSQL 用户的密码。如果还有其他问题，请随时告诉我。

# 创建名为 dify 的数据库

好的，以下是在 PostgreSQL 中创建名为 `dify` 的数据库的步骤：

### 1. 连接到 PostgreSQL 数据库

首先，使用 `psql` 命令连接到 PostgreSQL 数据库：

```
psql -U postgres -h localhost -p 5432
```

### 2. 创建新数据库

在 `psql` 提示符下，使用以下命令创建名为 `dify` 的数据库：

```
CREATE DATABASE dify;
```

### 3. 验证数据库创建

你可以通过以下命令列出所有数据库，验证 `dify` 数据库是否已创建：

```
\l
```

### 4. 切换到新数据库

如果你想切换到刚创建的 `dify` 数据库，可以使用以下命令：

```
\c dify
```

### 5. 创建用户并授权（可选）

如果你需要为 `dify` 数据库创建一个专门的用户并授予权限，可以按照以下步骤操作：

#### 创建新用户

```
CREATE USER dify_user WITH ENCRYPTED PASSWORD 'your_secure_password';
```

#### 授予用户权限

```
GRANT ALL PRIVILEGES ON DATABASE dify TO dify_user;
```

### 6. 退出 psql 提示符

完成操作后，可以使用以下命令退出 `psql` 提示符：

```
\q
```

### 总结

通过以上步骤，你已经成功创建了名为 `dify` 的数据库，并可以根据需要创建用户并授予权限。如果需要进一步操作，可以随时重新连接到数据库。
