---
layout: page
title: "flask"
subheadline: "flask"
meta_title: "flask"
subheadline: "flask"
teaser: ""
permalink: "/flask/"
---


# 1.安装基本的依赖库

```shell
pip install -r requirements.txt
gunicorn -w 3 run:app -b 0.0.0.0:8080
CFLAGS=-Qunused-arguments CPPFLAGS=-Qunused-arguments pip install -r requirements.txt
```


```shell
brew install mysql
PATH=$PATH:/usr/local/mysql/bin
mysql.server start
```

# 2.修改my.cnf配置

```shell
vim /usr/local/etc/my.cnf
default-authentication-plugin=mysql_native_password
```



# 3.python-mysql的依赖问题解决
```shell
 _mysql.c:44:10: fatal error: 'my_config.h' file not found
    #include "my_config.h"
             ^~~~~~~~~~~~~
    1 error generated.
    error: command 'cc' failed with exit status 1

    ----------------------------------------
Command ""/Users/nitish/gitProjects/Vision Backlog/vb_env/bin/python" -u -c "import setuptools, tokenize;__file__='/private/var/folders/ql/_w2_rlvs2351pdcnzhn04sf40000gn/T/pip-install-M4ue9E/mysql-python/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /private/var/folders/ql/_w2_rlvs2351pdcnzhn04sf40000gn/T/pip-record-7OCzf1/install-record.txt --single-version-externally-managed --compile --install-headers "/Users/nitish/gitProjects/Vision Backlog/vb_env/include/site/python2.7/mysql-python"" failed with error code 1 in /private/var/folders/ql/_w2_rlvs2351pdcnzhn04sf40000gn/T/pip-install-M4ue9E/mysql-python/
```


```shell
Collecting MySQL-python
  Using cached https://files.pythonhosted.org/packages/a5/e9/51b544da85a36a68debe7a7091f068d802fc515a3a202652828c73453cad/MySQL-python-1.2.5.zip
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/private/var/folders/ql/_w2_rlvs2351pdcnzhn04sf40000gn/T/pip-install-X6b4rU/MySQL-python/setup.py", line 17, in <module>
        metadata, options = get_config()
      File "setup_posix.py", line 53, in get_config
        libraries = [ dequote(i[2:]) for i in libs if i.startswith(compiler_flag("l")) ]
      File "setup_posix.py", line 8, in dequote
        if s[0] in "\"'" and s[0] == s[-1]:
    IndexError: string index out of range
```


# 4.安装Mysql依赖库

```shell
brew install mysql-connector-c
pip install MySQL-python
```

# 5.修改编译选项

```shell
vim /usr/local/bin/mysql_config

#Create options
Libs = "-L $pkglibdir"
Libs = " $libs  -l"

#Create options
Libs = "- L $pkglibdir"
Libs = "$libs -lmysqlclient -lssl -lcrypto"


LDFLAGS=-L/usr/local/opt/openssl/lib pip install mysql-python
pip install mysql-connector
```


# 6.创建Mysql数据库

```shell
create database test
create user 'testuser'@'localhost' identified by 'mima';
grant all privileges on `testdb`.* to 'testuser'@'localhost';  
flush privileges;
show grants;
```
   


# 7.book安装生成summary目录
```shell
npm install -g gitbook-summary
book sm
```

# 8.数据库权限问题
```shell
SQLALCHEMY_DATABASE_URI='mysql://testuser:mima@localhost:3306/testdb?charset=utf8mb4'
```

```shell
django.db.utils.OperationalError: (2059, "Authentication plugin 'caching_sha2_password' cannot be loaded: dlopen(/usr/local/Cellar/mysql-connector-c/6.1.11/lib/plugin/caching_sha2_password.so, 2): image not found")
```

# 9.给root用户添加密码
```shell
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'testmima'
```

```shell
Unable to load plugin 'caching_sha2_password'
ALTER USER 'username'@'ip_address' IDENTIFIED WITH mysql_native_password BY 'password';

    [mysqld]
    default_authentication_plugin=mysql_native_password
```



#  10.sqlalchemy因为mysql版本问题报错升级
```shell
（1193, "Unknown system variable 'tx_isolation'"）
```

```shell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --upgrade sqlalchemy --ignore-installed
```




# 11.让数据库字段支持中文 

```shell
ALTER TABLE database.tables CONVERT TO CHARACTER SET gbk COLLATE gbk_chinese_ci ​​​​ 
```


