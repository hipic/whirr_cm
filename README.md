whirr_cm
========

Cloudera Manager by Whirr

Cloudera Manager can be launched using whirr as illustrated in whirr-cm [1]. However, it does not work for me so that I update whirr-cm [1] to make it more in detail.
The following is tested at CentOS 5.9 (64bits). You may test your CentOS using 'cat /etc/redhat-release'

1. install Maven if 'which mvn' does not show any 'maven'

wget http://mirror.cc.columbia.edu/pub/software/apache/maven/maven-3/3.0.5/binaries/apache-maven-3.0.5-bin.tar.gz

sudo tar xzf apache-maven-3.0.5-bin.tar.gz -C /usr/local

cd /usr/local

sudo ln -s apache-maven-3.0.5 maven


2. Install git if 'which git' does not show any 'git'

yum groupinstall 'Development Tools'

yum install gettext-devel expat-devel curl-devel zlib-devel openssl-devel

wget http://kernel.org/pub/software/scm/git/git-1.8.3.tar.gz

tar zxf git-1.8.3.tar.gz

cd git-1.8.3

make prefix=/usr/local all

make prefix=/usr/local install


3. Download cm-ec2-hipic.properties:

curl -O https://raw.github.com/hipic/whirr-cm/master/cm-ec2-hipic.properties

4. 

Reference 1. https://github.com/cloudera/whirr-cm
