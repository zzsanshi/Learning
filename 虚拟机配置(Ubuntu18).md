# 1.Python版本转换

[链接](https://www.jb51.net/article/163112.htm)

1. 要以root身份操作:sudo` `su
2. 确认本机下的python默认版本:python
3. 列出所有可用的python 替代版本信息:update-alternatives --list python
4. 更新一下替代列表:update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1 
5. update-alternatives --list python
6.  update-alternatives --config python

# 2.安装pip

1. 更新apt源:sudo apt update
2. 安装pip:sudo apt install python3-pip
3. 查看版本:pip3 --version

# 3.  安装jupyternotebook

1. sudo pip install jupyter
2. jupyter notebook

# 4, 安装tensorflow

1. sudo pip3 install tensorflow==1.11.0

