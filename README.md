# pgsqlparser

功能：

定制postgres9.6源码，输入SQL文件进行SQL语法检查，并识别出哪些表有变更，提高SQL上线时备份的效率。

编译安装

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


git clone https://github.com/zjjxxlgb/pgsqlparser.git

gzip -d pgsqlparser.gz

cd pgsqlparser

开始编译安装

./configure --prefix=/home/pgsqlparser/pgsql/  --with-perl --with-python --with-libxml --with-libxslt

 make
 
 make install

/home/pgsqlparser/pgsql/bin/initdb -D /home/pgsqlparser/pgdata

/home/pgsqlparser/pgsql/bin/createdb mytestdb


语法检测

postgres  <test.sql mytestdb 2>&1|grep "syntax error"

备份表识别（alter,drop,truncate,delete,update)

postgres  <test.sql mytestdb 2>&1|grep dbtablename



