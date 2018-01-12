问题1
```linux
root@guoling:~/source/flask_note# mkvirtualenv python_env
mkvirtualenv: command not found
```
问题1--答案：
```
mkvirtualenv command not found解决

在guoling阿里云服务器的家目录下创建python_env虚拟环境后，使用mkvirtualenv python_env命令，没有提示，输完回车报下面错误，
mkvirtualenv command not found
仔细检查后发现
1.
sudo pip install virtualenv 
sudo pip install virtualenvwrapper

没问题。
2.
.bashrc也修改了，并在最后面也加了下面两句话
export WORKON_HOME=~/.environments
source /usr/local/bin/virtualenvwrapper.sh
3.
结果发现，忘了重新加载.bashrc文件了，配置文件没有生效。
需要 source ~/.bashrc。或者关闭终端再重新连接终端。
```
