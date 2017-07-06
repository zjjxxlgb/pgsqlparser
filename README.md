# pgsqlparser

## 功能：

定制postgres9.6源码，输入SQL文件进行SQL语法检查，并识别出哪些表有变更，提高SQL上线时备份的效率。

## 安装准备
```Bash
yum install -y perl-ExtUtils-Embed readline-devel zlib-devel pam-devel libxml2-devel libxslt-devel openldap-devel Python-devel gcc-c++   openssl-devel cmake
groupadd pgsqlparser
useradd -g pgsqlparser pgsqlparser

mkdir -p /home/pgsqlparser/pgsql
mkdir -p /home/pgsqlparser/data
chown -R pgsqlparser:pgsqlparser /home/pgsqlparser/

vi /home/pgsqlparser/.bash_profile

export PGHOME=/home/pgsqlparser/pgsql/
export PGDATA=/home/pgsqlparser/pgdata
export PGDATABASE=pgsqlparser
export PGUSER=pgsqlparser
export PGPORT=5432
export MANPATH=$PGHOME/share/man:$MANPATH
export LD_LIBRARY_PATH=$PGHOME/lib
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/lib:/usr/lib:/usr/local/lib
export PATH=$PGHOME/bin:$PATH
export TEMP=/tmp
export TMPDIR=/tmp
```

## 开始编译安装
```Bash
git clone https://github.com/zjjxxlgb/pgsqlparser.git
cd pgsqlparser
chmod u+x configure
./configure --prefix=/home/pgsqlparser/pgsql/  --with-perl --with-python --with-libxml --with-libxslt
make
make install

mv pgsqlparser/pgdata.tar.gz /home/pgsqlparser/
cd ..
tar -zxf pgdata.tar.gz
```
## 语法检测
```Bash
postgres  <test2.sql postgres 2>&1|grep 'FATAL'
```
## 备份表识别（alter,drop,truncate,delete,update)
```Bash
postgres  <test.sql postgres 2>&1|grep dbtablename
```


 ![image](https://github.com/zjjxxlgb/pgsqlparser/blob/master/readme.JPG)
