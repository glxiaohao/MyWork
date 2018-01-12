# 文件操作

## 例1 在当前项目下创建 guoling\\中文haha 这样的路径，并在次路径下创建一个jianxi_xml.txt文件
```python
# coding:utf-8
import os
import sys
reload(sys)
sys.setdefaultencoding("utf-8")
# 获取当前文件的路径
cur_path = os.path.abspath(os.curdir)
print "当前文件的路径:",cur_path

# 先定义一个带路径的文件夹
new_path ='guoling\\中文haha'     # 创建多级目录 guoling\中文haha
# new_path = '中文blahblah'          # 创建一级目录
print "新创建的路径:",new_path
goal_path = cur_path + '\\' + new_path.encode('gb2312')
print "新创建的文件即将所在的路径:",goal_path
# os.mkdir(goal_path)

# 判断目录是否存在，若不存在则创建
if not os.path.exists(goal_path):
    os.makedirs(goal_path)
    print "目录不存在"

    # 目录创建好了之后，再在这个目录下创建文件
    # 先定义一个带路径的文件
    filename = goal_path+"\\test.txt"     # /cur_path/guoling/test.txt
    print "filename:",filename

    # 将文件路径分割出来
    file_dir = os.path.split(filename)[0]
    print "文件路径(不包括文件本身):", file_dir

    # 然后再判断文件是否存在，如果不存在，则在你刚创建的目录下创建jianxi_xml.txt 这个文件
    if not os.path.exists(filename):
        fp = open(file_dir+"\\jianxi_xml.txt", 'w')
        print "jianxi_xml.txt 文件创建成功"

else:
    pass
    print "目录已存在，不创建目录"
```