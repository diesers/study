# 实现代码提交到gitlab，自动发布到k8s上。

| 开发服务器 | gitlab服务器 | harbor | k8s-master | k8s-node1 | k8s-node2 |
|  ----  | ----  | ---- | ---- | ---- | ---- |
| 192.168.10.123  | 10.0.10.177 | |10.0.10.177 | 10.0.10.168| 10.0.10.171 | 10.0.10.179|


涉及到的应用：
- gitlab
- gitlab-runner
- harbor
- docker
- kubernetes cluster