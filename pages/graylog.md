---
layout: page
title: "graylog"
subheadline: "graylog"
meta_title: "graylog"
subheadline: "graylog"
teaser: ""
permalink: "/graylog/"
---





### Graylog部署安装

## 0x01 概要

```
$ sudo rpm -Uvh https://packages.graylog2.org/repo/packages/graylog-2.3-repository_latest.rpm
$ sudo yum install graylog-server
```



```
show dbs
use bip
db.bip.insert({"key":"graylog"})
db.createUser({·
    user:'user',
    pwd:'password',
    roles:[
        {role:'readWrite',db:'admin'}
    ]
})
show users
```


