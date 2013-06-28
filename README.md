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
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa_whirr
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
Whirr Handler [CMClusterProvision] follow live at http://ec2-50-16-12-129.compute-1.amazonaws.com:7180
```
and post-provision:

```bash
Whirr Handler [CMClusterProvision] CM AGENTS
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@23.22.38.202
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@23.23.48.115
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@50.16.12.129
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@50.16.36.169
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@54.226.54.19
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@54.226.90.179
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@54.227.35.91
Whirr Handler [CMClusterProvision] CM SERVER
Whirr Handler [CMClusterProvision]   http://ec2-50-16-12-129.compute-1.amazonaws.com:7180
Whirr Handler [CMClusterProvision]   ssh -o StrictHostKeyChecking=no -i /home/dalgual/.ssh/id_rsa_whirr whirr@50.16.12.129
```
You are able to log into the CM Web Console (or hosts) at any stage and observe proceedings, via the async, real time UI.

The default admin user credentials are:
```bash
Username: admin 
Password: admin 
```
And, you can ssh to remotely open a client node
```bash
[cm-agent+cm-cdh-jobtracker+cm-cdh-hivemetastore+cm-cdh-hiveserver2+cm-cdh-hcatalog+cm-cdh-impala-statestore+cm-cdh-hue-server+cm-cdh-hue-beeswaxserver+cm-cdh-sqoop-server+cm-cdh-zookeeper]: ssh -i /home/dalgual/.ssh/id_rsa_whirr -o "UserKnownHostsFile /dev/null" -o StrictHostKeyChecking=no whirr@54.227.35.91
To destroy cluster, run 'whirr destroy-cluster' with the same options used to launch it.
[dalgual@Linux-26-64bit whirr]$  ssh -i /home/dalgual/.ssh/id_rsa_whirr -o "UserKnownHostsFile /dev/null" -o StrictHostKeyChecking=no whirr@54.227.35.91
Warning: Permanently added '54.227.35.91' (RSA) to the list of known hosts.
Welcome to Ubuntu 12.04.2 LTS (GNU/Linux 3.2.0-40-virtual x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Fri Jun 28 05:32:11 UTC 2013

  System load:  0.01              Processes:           80
  Usage of /:   28.2% of 7.87GB   Users logged in:     0
  Memory usage: 38%               IP address for eth0: 10.38.178.63
  Swap usage:   0%

  Graph this data and manage this system at https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

  Use Juju to deploy your cloud instances and workloads:
    https://juju.ubuntu.com/#cloud-precise

Last login: Fri Jun 28 03:24:25 2013 from 121.138.74.134
whirr@ip-10-38-178-63:~$ ls
configure-cm-agent_cm-cdh-jobtracker_cm-cdh-hivemetastore_cm-cdh-hiveserver2_cm-cdh-hcatalog_cm-cdh-impala-statestore_cm-cdh-hue-server_cm-cdh-hue-beeswaxserver_cm-cdh-sqoop-server_cm-cdh-zookeeper
w
```
## 

## Reference 
[1]. https://github.com/cloudera/whirr-cm
