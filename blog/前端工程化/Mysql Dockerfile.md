# Mysql Dockerfile

groupadd -r mysql && useradd -r -g mysql mysql

yum update && yum install -y make gcc-c++ cmake wget

cd /usr/local/src

wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.10-linux-glibc2.5-x86_64.tar.gz

tar -zxvf mysql-5.7.10-linux-glibc2.5-x86_64.tar.gz

rm -f mysql-5.7.10-linux-glibc2.5-x86_64.tar.gz






