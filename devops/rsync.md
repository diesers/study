将本地/data目录同步到远程服务器上的/data目录
```sh
rsync -e "ssh -p22" -avpz  /data/ root@10.0.4.212:/data/
```
将远程服务器上的目录同步到本地
```sh
rsync -av username@remote_host:source/ destination
```
--delete #删除远程服务器上的目录再同步

快速删除文件
```sh
rsync -a --delete emptydir/ test/
```

