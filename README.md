# Cloudera Manager by Whirr
========

Cloudera Manager can be launched using whirr as illustrated in whirr-cm [1]. However, it is a little bit ambiguous to me - actually it did not work for me - so that I update whirr-cm [1] to make it more in detail.

## Install Cloudera Manager from a AWS instance
Download the Cloudera Manager 4.5 installer and execute it on the remote instance:
```bash
$ wget http://archive.cloudera.com/cm4/installer/latest/cloudera-manager-installer.bin
$ chmod +x cloudera-manager-installer.bin
$ sudo ./cloudera-manager-installer.bin
```

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
curl -O https://raw.github.com/hipic/whirr_cm/master/cm-ec2-hipic.properties
curl -O https://raw.github.com/hipic/whirr_cm/master/setupWhirrCM.sh    
curl -O https://raw.github.com/hipic/whirr_cm/master/startWhirrCM.sh
curl -O https://raw.github.com/hipic/whirr_cm/master/stopWhirrCM.sh
```
You need to set up your environement first
```bash
source ./setupWhirrCM.sh
```
The following command will start a cluster with 7 nodes, 1 CM server, 3 master and 3 slave nodes. To change the cluster topology, edit the cm-ec2-hipic.properties file.
```bash
source ./startWhirrCM.sh
```
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
```
## Others: you may refer to cloudera's whirr-cm [1] to manage, use, shutdown, test the CDH cluster with CM

For example, to stop the servers
```bash
source ./stopWhirrCM.sh
```

And, Hadoop and ecosystems' conf files reside at /etc/hadoop/conf, /etc/solr/conf, /etc/zookeeper/conf etc.

## Trouble Shooting at HDFS

If hadoop points at file:/// not hdfs:///, ‘ssh’ to the client node that is the last ssh of whirr. Then, I need to update Hadoop’s core-site.xml by adding its namenode, which is at /etc/hadoop/conf

```bash
<configuration>

<property>
 <name>fs.defaultFS</name>
 <value>hdfs://hadoop-namenode:8020</value>
</property>

<configuration>
```

For example, if a name node’s local ip address is 10.80.221.129

```bash
<configuration>

<property>
 <name>fs.defaultFS</name>
 <value>hdfs://10.80.221.129:8020</value>
</property>

<configuration>
```

Now you can see the HDFS directories and files. If you ssh to other nodes, you have to change other nodes’ core-site.xml too.

You may also see the following error:
```bash
whirr@ip-10-144-65-6:~$ hadoop fs -put foo.txt test/ 
put: org.apache.hadoop.security.AccessControlException: Permission denied: user=root, access=WRITE, inode="":whirr:supergroup:rwxr-xr-x
```
At a jobtracker master node, do the following steps for the user 'whirr':
```bash
$ sudo -u hdfs hadoop fs -mkdir /user/whirr
```
If there is no way to start 'sudo' for the user, do the following first
```bash
$ sudo su
```
```bash
$ sudo -u hdfs hadoop fs -chown whirr:whirr /user/whirr
$ hadoop fs -ls /user/
Found 6 items
drwxr-xr-x   - whirr whirr            0 2013-07-04 05:30 /user/whirr
drwxr-xr-x   - hdfs     supergroup          0 2013-07-04 05:14 /user/hdfs
drwxrwxr-t   - hive     hive                0 2013-07-04 04:50 /user/hive
drwxrwxr-x   - oozie    oozie               0 2013-07-04 04:53 /user/oozie
drwxr-xr-x   - root     root                0 2013-07-04 05:15 /user/root
drwxrwxr-x   - sqoop2   sqoop               0 2013-07-04 04:54 /user/sqoop2

```

For the user root, do the following:
```bash
$ sudo su
$ sudo -u hdfs hadoop fs -mkdir root
$ hadoop fs -ls /user
Found 4 items
drwxr-xr-x   - hdfs   supergroup          0 2013-07-04 05:14 /user/hdfs
drwxrwxr-t   - hive   hive                0 2013-07-04 04:50 /user/hive
drwxrwxr-x   - oozie  oozie               0 2013-07-04 04:53 /user/oozie
drwxrwxr-x   - sqoop2 sqoop               0 2013-07-04 04:54 /user/sqoop2
$ hadoop fs -ls /user/hdfs
Found 1 items
drwxr-xr-x   - hdfs supergroup          0 2013-07-04 05:14 /user/hdfs/root
$ sudo -u hdfs hadoop fs -chown root:root /user
$ hadoop fs -mkdir test
$ hadoop fs -ls /
Found 3 items
drwxr-xr-x   - hbase hbase               0 2013-07-04 04:49 /hbase
drwxrwxrwt   - hdfs  supergroup          0 2013-07-04 04:58 /tmp
drwxr-xr-x   - root  root                0 2013-07-04 05:15 /user
$ hadoop fs -ls /user/
Found 5 items
drwxr-xr-x   - hdfs   supergroup          0 2013-07-04 05:14 /user/hdfs
drwxrwxr-t   - hive   hive                0 2013-07-04 04:50 /user/hive
drwxrwxr-x   - oozie  oozie               0 2013-07-04 04:53 /user/oozie
drwxr-xr-x   - root   root                0 2013-07-04 05:15 /user/root
drwxrwxr-x   - sqoop2 sqoop               0 2013-07-04 04:54 /user/sqoop2
$ hadoop fs -ls /user/root
Found 1 items
drwxr-xr-x   - root root          0 2013-07-04 05:15 /user/root/test
$ vi test.txt
$ hadoop fs -put test.txt test/
$ hadoop fs -cat test.txt
cat: `test.txt': No such file or directory
$ hadoop fs -cat test/test.txt
hello world
HELLO WORLD
```

## Trouble Shooting at Hive
When you run hive command at hive console, you may see Hive errors which is mostly because you don't have a right to write a file and directory such as

```bash
ERROR XBM0H: Directory /var/lib/hive/metastore/metastore_db cannot be created.
```

Then, you have to go to and update the database name directory of /etc/hive/conf/hive-site.xml as follows:

```bash
<!-- Hive Execution Parameters -->

<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:derby:;databaseName=/home/users/whirr/hive/metastore/metastore_db;create=true</value>
  <description>JDBC connect string for a JDBC metastore</description>
</property>
```
The default directory was 'databaseName=/var/lib/hive/metastore/metastore_db'

## Reference 
#### [1]. https://github.com/cloudera/whirr-cm
#### [2]. http://stackoverflow.com/questions/12391226/hadoop-hdfs-points-to-file-not-hdfs?rq=1
#### [3]. http://stackoverflow.com/questions/16008486/after-installing-hadoop-via-cloudera-manager-4-5-hdfs-only-points-to-the-local-f
#### [4]. https://groups.google.com/a/cloudera.org/forum/#!topic/scm-users/JIk3vz6k1ek
#### [5]. http://blog.cloudera.com/blog/2013/03/how-to-create-a-cdh-cluster-on-amazon-ec2-via-cloudera-manager/

