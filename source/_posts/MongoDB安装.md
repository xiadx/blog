---
title: MongoDB安装
date: 2019-09-18 12:45:00
tags: MongoDB
categories: MongoDB
---
MongoDB安装

## Mac安装

```bash
cd /usr/local
sudo curl -O https://fastdl.mongodb.org/osx/mongodb-osx-ssl-x86_64-4.0.9.tgz
sudo tar -zxvf mongodb-osx-ssl-x86_64-4.0.9.tgz
sudo mv mongodb-osx-x86_64-4.0.9/ mongodb
```

## Mac运行

```bash
sudo mkdir -p /usr/local/mongodb/data/db
cd /usr/local/mongodb/bin
sudo ./mongod --dbpath=/usr/local/mongodb/data/db
```

## Mac测试

```bash
cd /usr/local/mongodb/bin
sudo ./mongo

> db
> db.test.insert({"name":"xiadingxin"})
> show dbs
> show tables
> db.test.find().pretty()
> db.createCollection("user")
> db.user.insert({"name":"xiadingxin"})
> db.test.drop()
> show collections
```

## Linux安装

```bash
cd /usr/local
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz 
mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb 
```

## Linux运行

```bash
sudo mkdir -p /usr/local/mongodb/data/db
sudo ./mongod --dbpath=/usr/local/mongodb/data/db
```

## Linux测试

```bash
cd /usr/local/mongodb/bin
sudo ./mongo

> db
> db.test.insert({"name":"xiadingxin"})
> show dbs
> show tables
> db.test.find().pretty()
> db.createCollection("user")
> db.user.insert({"name":"xiadingxin"})
> db.test.drop()
> show collections
```

## 线上生产环境测试

已在推荐平台线上GPU机器10.63.1.130安装MongoDB的shell客户端，可查看线上电商爬虫视频信息数据

```bash
> 1. /usr/local/mongodb/bin/mongo 192.168.2.230:28116
> 2. use admin
> 3. db.auth("readany", "Mfw09uygt")
> 4. rs.slaveOk()
> 5. show dbs
> 6. use mspider_price
> 7. show tables
> 8. db.video_info.findOne()
```