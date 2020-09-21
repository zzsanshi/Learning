# 将本地的项目上传到github仓库中

## 1. 创建链接



1. 创建仓库
2. 复制仓库的地址
3. 把github上的仓库克隆到本地: git clone 仓库地址
4. 进入目录:cd sell-app
5. 将sell-app文件夹里面的文件都添加进来:git add .
6. 填写修改信息:git commit -m  '修改信息'
7. 把本地仓库push到github上（要填写用户名和密码）:git push -u origin master

## 2.更新

1. 更新全部文件：git add .
2. 输入更新说明：git commit -m '更新说明'
3. push到远程master分支上：git push （将本地的代码同步到线上的分支上）