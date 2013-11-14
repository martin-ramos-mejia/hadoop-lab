hadoop-lab
==========

This project aims to provide a [hadoop](http://hadoop.apache.org) lab environment for the students of 75.06 FIUBA (University of Buenos Aires).

Originally from https://github.com/rkrol/vagrant-hadoop

Creating the cluster:
---------------------

- clone this repository
- [install vagrant](http://www.vagrantup.com/) 
- vagrant box add base-hadoop http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210.box
- vagrant up

Architecture Layout:
--------------------

![HaddopLabLayout](https://raw.github.com/idontdomath/hadoop-lab/hadoop-2.2.0/doc/hadoop-lab-layout.png "Haddop Lab Layout")

Cluster configuration:
----------------------


### Setting up the services:

### Namenode daemon (master machine):

- vagrant ssh master
- cd /opt/hadoop-2.2.0
- sudo bin/hdfs namenode -format //to format the hdfs volume if needed
- sudo sbin/hadoop-daemon.sh --config conf/ --script hdfs start namenode

### Starting DataNode (slaves: haddop1, haddop2, haddop3 machines)
- vagrant ssh haddop*
- cd /opt/hadoop-2.2.0
- sudo sbin/hadoop-daemon.sh --config conf/ --script hdfs start datanode

### Starting Resourcer Manager (backup machine)
- vagrant ssh master
- cd /opt/hadoop-2.2.0
- sudo sbin/yarn-daemon.sh --config conf/ start resourcemanager

### Starting Node Managers (slaves: haddop1, haddop2, haddop3 machines)
- sudo sbin/yarn-daemon.sh --config conf/ start nodemanager

To access the admin interface of HDFS http://192.168.1.10:50070

We also recommend to introduce the name of the machines in the /etc/hosts

```
192.168.1.10    master 
192.168.1.11    backup 
192.168.1.12    hadoop1 
192.168.1.13    hadoop2 
192.168.1.14    haddop3 
```

Some snippets:
--------------

### Adding data to the HDFS

- vagrant ssh backup
- cd /opt/hadoop-2.2.0
- bin/hdfs dfs -mkdir /gutenberg
- bin/hdfs dfs -copyFromLocal /home/vagrant/data/huck-finn.txt /gutenberg/huck-finn.txt
- bin/hdfs dfs -copyFromLocal /home/vagrant/data/gutenberg/tom-sawyer.txt /gutenberg/tom-sawyer.txt

### Running a map reduce job

- bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.2.0.jar wordcount /gutenberg /gutenberg-out
