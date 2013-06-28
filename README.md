# Cloudera Manager by Whirr
========

Cloudera Manager can be launched using whirr as illustrated in whirr-cm [1]. However, it does not work for me so that I update whirr-cm [1] to make it more in detail.
The following is tested at CentOS 5.9 (64bits). You may test your CentOS using 'cat /etc/redhat-release'

## install Maven if 'which mvn' does not show any 'maven'

```bash
wget http://mirror.cc.columbia.edu/pub/software/apache/maven/maven-3/3.0.5/binaries/apache-maven-3.0.5-bin.tar.gz

sudo tar xzf apache-maven-3.0.5-bin.tar.gz -C /usr/local

cd /usr/local

sudo ln -s apache-maven-3.0.5 maven
```


## Install git if 'which git' does not show any 'git'

```bash
yum groupinstall 'Development Tools'

yum install gettext-devel expat-devel curl-devel zlib-devel openssl-devel

wget http://kernel.org/pub/software/scm/git/git-1.8.3.tar.gz

tar zxf git-1.8.3.tar.gz

cd git-1.8.3

make prefix=/usr/local all

make prefix=/usr/local install
```

## Install Whirr:

### install the whirr package and create some environment variables, eg for RHEL/CentOS:

```bash
yum install whirr
export WHIRR_HOME=/usr/lib/whirr
export PATH=$WHIRR_HOME/bin:$PATH
```

## Create a password-less SSH keypair for Whirr to use:

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa_cm
```

## Download cm-ec2-hipic.properties and place it to your current location

```bash
curl -O https://raw.github.com/hipic/whirr-cm/master/cm-ec2-hipic.properties
```

The following command will start a cluster with 7 nodes, 1 CM server, 3 master and 3 slave nodes. To change the cluster topology, edit the cm-ec2.properties file.

whirr launch-cluster --config cm-ec2-hipic.properties

Whirr will report progress to the console as it runs and will exit once complete.

During the various phases of execution, the Whirr CM plugin will report the CM Web Console URL, eg pre-provision

```bash
Whirr Handler -----------------------------------------------------------------
Whirr Handler [CMClusterProvision] 
Whirr Handler -----------------------------------------------------------------
Whirr Handler 
Whirr Handler [CMClusterProvision] follow live at http://37.188.114.234:7180
```
and post-provision:

```bash
Whirr Handler [CMClusterProvision] CM AGENTS
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /root/.ssh/whirr whirr@37.188.114.209
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /root/.ssh/whirr whirr@37.188.114.210
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /root/.ssh/whirr whirr@37.188.114.225
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /root/.ssh/whirr whirr@37.188.114.234
Whirr Handler [CMClusterProvision] CM SERVER
Whirr Handler [CMClusterProvision]   http://ec2-54-216-175-183.eu-west-1.compute.amazonaws.com:7180
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /root/.ssh/whirr whirr@37.188.114.234
```
You are able to log into the CM Web Console (or hosts) at any stage and observe proceedings, via the async, real time UI.

The default admin user credentials are:
```bash
Username: admin 
Password: admin 
```
## 

## Reference 
[1]. https://github.com/cloudera/whirr-cm
