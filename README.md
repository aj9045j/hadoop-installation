
# Hadoop installation steps

OS --> Ubuntu 

Hadoop version --> 3.3.6

MAKE SURE YOU ARE IN /home/{username} directory

Update and upgrade your Ubuntu system for the latest security patches and software updates.

# Refresh the local package index 😊
```bash
sudo apt update
```

# Upgrade installed packages to their latest versions 😊
```bash
sudo apt upgrade
```

## SSH Key Setup
SSH keys are cryptographic key pairs used for secure authentication in Hadoop and other distributed systems. They enhance security by eliminating the need for passwords, enabling automated processes, securing data transfer, and facilitating cluster setup and configuration. SSH keys consist of a public key (shared) and a private key (kept secret) for secure communication between nodes in a Hadoop cluster.
```bash
sudo apt install openssh-server openssh-client -y
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```
Confirm the changes and you will be able to easily connect to your localhost at all times 
```bash
ssh localhost
```
# Install JAVA
```bash
sudo apt install openjdk-11-jdk -y
```

# Download Hadoop
```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
```
After completion extract file and move it to /usr/local/hadoop
```bash
tar xzf hadoop-3.3.6.tar.gz
sudo mv hadoop-3.3.6 /usr/local/hadoop
```
# Configure bashrc file 😮‍💨
run
```bash
gedit ~/.bashrc
```
at the last paste this path and save it
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PDSH_RCMD_TYPE=ssh
```
now run this to save changes
```bash
source ~/.bashrc
```
to verify run
```bash
hadoop version
```
# Configure hadoop files 😮‍💨😮‍💨
note : it may vary as per your Requirements
run this to go to hadoop directory
```bash
cd /usr/local/hadoop/etc/hadoop
```
a) now open core-site.xml by
```bash
gedit core-site.xml
```
and paste this in configuration section
```bash
<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>
```
save the file and close it

b) now open hdfs-site.xml by
```bash
gedit hdfs-site.xml
```
and paste this in configuration section
```bash
<property> 
 <name>dfs.replication</name> 
 <value>1</value> 
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>/home/{username}/hadoop2-dir/namenode-dir</value>
</property>
<property>
<name>dfs.datanode.data.dir</name>
<value>/home/{username}/hadoop2-dir/datanode-dir</value>
</property>
```
make sure to edit the path and save it then close it

c) now open mapred-site.xml by
```bash
gedit mapred-site.xml
```
and paste this in configuration section
```bash
<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=/usr/local/hadoop/etc/hadoop</value>
</property>
<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=/usr/local/hadoop/etc/hadoop</value>
</property>
<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=/usr/local/hadoop/etc/hadoop</value>
</property>
```
save it then close it

d) now open yarn-site.xml by
```bash
gedit yarn-site.xml
```
and paste this in configuration section
```bash
<property>
<name>yarn.nodemanager.aux-services</name> <value>mapreduce_shuffle</value>
</property>
<property>
<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name> <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
<description>The hostname of the RM.</description>
<name>yarn.resourcemanager.hostname</name>
<value>localhost</value>
</property>
<property>
<description>The address of the applications manager interface in the RM.</description>
<name>yarn.resourcemanager.address</name>
<value>${yarn.resourcemanager.hostname}:8032</value>
</property>
```
save it, then close it

e) now open hadoop-env.sh by
```bash
gedit hadoop-env.sh
```
on this paste this and save it
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
f) open mapred-env.sh
```bash
gedit mapred-env.sh
```
and paste this ,then save it
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
g) open yarn-env.sh
```bash
gedit yarn-env.sh
```
and paste this ,then save it
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
ALL the file configuration was done.

# run this to configure all changes 
first go to
```bash
cd /usr/local/hadoop
```
now run this to format hdfs
```bash
hdfs namenode -format
```
if it does show success format check for the error

now to start run all services run
```bash
start-all.sh
```
after starting all services,now open the browser

for namenode 
```bash
http://localhost:9870
```
for ResourceManager
```bash
http://localhost:8088
```

# Hadoop setup complete successfully 😎

to stop all services run
```bash
stop-all.sh
```
