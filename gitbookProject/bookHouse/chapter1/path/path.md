# 测试环境变量

- [查看当前服务器是否配置了公钥这个环境变量(DB_PUBKEY)]()

```python
import os
env_dist = os.environ
print env_dist.get('DB_PUBKEY')
```
### 效果图：
![Alt text](images/配置环境变量.png "说明：这是图片1")
