```sh
 rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch
 ```
```sh
cat > /etc/yum.repo.d/filebeat.repo <<EOF
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```
```sh
yum install filebeat -y
```
