# Python install from source code

> 来源：[https://learning.feishu.cn/docx/SzfVdrZJmo7vdPx5aSHcOE7anyb](https://learning.feishu.cn/docx/SzfVdrZJmo7vdPx5aSHcOE7anyb)

在老的 Linux OS 上，无法用包管理器安装新 python。比如，

Ubuntu 18.04 不能 apt-get install Python 3.11.

# Last Tested commands

需要 compile from source code。

https://www.build-python-from-source.com/

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev liblzma-dev tk-dev

cd ~/packages
wget https://www.python.org/ftp/python/3.11.6/Python-3.11.6.tgz
tar xzf Python-3.11.6.tgz
cd Python-3.11.6

--enable-loadable-sqlite-extensions --enable-optimizations
sudo ./configure --prefix=/opt/python/3.11.6/ --enable-loadable-sqlite-extensions --enable-optimizations --with-lto --with-computed-gotos --with-system-ffi --enable-shared
sudo make -j &quot;$(nproc)&quot;
sudo ./python3.11 -m test -j &quot;$(nproc)&quot;
sudo make altinstall
# The script pip3.11 is installed in &#x27;/opt/python/3.11.6/bin&#x27; which is not on PATH.
# sudo rm /tmp/Python-3.11.6.tgz

sudo /opt/python/3.11.6/bin/python3.11 -m pip install --upgrade pip setuptools wheel

sudo ln -s /opt/python/3.11.6/bin/python3.11        /opt/python/3.11.6/bin/python3
sudo ln -s /opt/python/3.11.6/bin/python3.11        /opt/python/3.11.6/bin/python
sudo ln -s /opt/python/3.11.6/bin/pip3.11           /opt/python/3.11.6/bin/pip3
sudo ln -s /opt/python/3.11.6/bin/pip3.11           /opt/python/3.11.6/bin/pip
sudo ln -s /opt/python/3.11.6/bin/pydoc3.11         /opt/python/3.11.6/bin/pydoc
sudo ln -s /opt/python/3.11.6/bin/idle3.11          /opt/python/3.11.6/bin/idle
sudo ln -s /opt/python/3.11.6/bin/python3.11-config      /opt/python/3.11.6/bin/python-config
```

```
# virtualenv
/opt/python/3.11.6/bin/python3.11 -m pip install virtualenv

/opt/python/3.11.6/bin/python3.11 -m virtualenv -p /opt/python/3.11.6/bin/python3.11 ~/venv-shared-lib-python3.11

ln -s ~/venv-shared-lib-python3.11 ~/venv-default
```

# Trash

```
wget https://www.python.org/ftp/python/3.11.6/Python-3.11.6.tgz
tar -zxvf Python-3.11.6.tgz
cd Python-3.11.6

./configure CFLAGS=&quot;-fPIC&quot;
./configure --enable-optimizations
make -j $(nproc)

# install Python 3.11
# We have used altinstall is used instead of install to keep the default Python binary path in /usr/bin/python.
sudo make altinstall

# check
python3.11 --version

# install pip for python3.11
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.11
# check pip
pip3.11 -V

# virtualenv
pip3.11 install virtualenv
```