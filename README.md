# hadoop_2
How to install hadoop 2 on ubuntu (It may cause faliure on hadoop 3)
We install 1 master and 2 slaves(data1 and data2) here.

Reference: 
1.Hadoop+sprak大數據巨量分析與機器學習 整合開發實戰
2.大數據分析與應用 使用hadoop與spark

Step 1
install virtulbox

Step 2
1.install ubuntu 64bit and name it as 'master'
(sometimes there have no ubuntu-64bit in the default menu, google 'ubuntu 64bit virtualbox' to modify computer setting)
2.go to master's setting -> network
choose "Bridged Adapter" in "Attached to" column

Step 3: install JAVA on master
1.type "sudo apt-get update"
2.type "sudo apt-get install default-jdk"
3.type "java -version" to make sure we have installed java
4.type "update-alternatives --display java" to know where java-installed folder is, and remember this folder's path
(we will use it in ~/.bashrc file)

Step 4: install SSH on master
1.type "sudo apt-get install ssh"
2.type "ssh-keygen -t dsa -P '' f ~/.ssh/id_dsa" to produce key file
3.type "ll ~/.ssh" to see the key file
4.type "cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys"

Step 5: install Hadoop 2 series on master (we use hadoop-2.8.3 as example)
1.go to http://ftp.twaren.net/Unix/Web/apache/hadoop/common/
2.copy link of hadoop0-2.8.3.tar.gz
3.type "wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.8.3/hadoop-2.8.3.tar.gz"
4.type "sudo tar -zxvf hadoop0-2.8.3.tar.gz"
5.type "sudo mv hadoop-2.8.3 /usr/local/hadoop" to move hadoop to destination folder
6.type "ll /usr/local/hadoop" to check files of hadoop
7.type "sudo gedit ~/.bashrc" to edit eviroment path (the file's content can be found in this repository)
8.type "source ~/.bashrc"
9.type "sudo gedit /usr/local/hadoop/etc/hadoop/hadoop-env.sh"(the file's content can be found in this repository)
10.type "sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml"(the file's content can be found in this repository)
11.type "sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml"(the file's content can be found in this repository)
12.type "sudo cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml"
13.type "sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml"(the file's content can be found in this repository)
14.type "sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml"(the file's content can be found in this repository)
15.type "sudo mkdir -p /usr/local/hadoop/hadoop_data/hdfs/namenode"
16.type "sudo chown -R hduser:hduser /usr/local/hadoop" ("hduser" is your ubuntu's name or your username)
17.type "sudo gedit /usr/local/hadoop/etc/hadoop/master" and then enter "master"
18.type "sudo gedit /usr/local/hadoop/etc/hadoop/slaves" (the file's content can be found in this repository)
19.type "sudo gedit /etc/hostname" to modify the content as 'master'
20.type "ifconfig" to know the ip
21.type "sudo gedit /etc/network/interfaces"(the file's content can be found in this repository)
22.type "sudo gedit /etc/hosts"(the file's content can be found in this repository)
23.turn off machine

Setp 6: Create data1
1.right click on master and choose "clone"
2.make sure "Reinitialize...all network cards" is checked

Setp 7: Setting data1
1.type "sudo gedit /etc/hostname" to modify the content as "data1"
2.type "ifconfig" to know the ip
3.type "sudo gedit /etc/network/interfaces" to modify the ip of data1
4.type "sudo gedit /etc/hosts" to modify the ip of data1
5.type "sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml"(the file's content can be found in this repository)
6.type "sudo mkdir -p /usr/local/hadoop/hadoop_data/hdfs/datanode"
7.type "sudo chown -R hduser:hduser /usr/local/hadoop" ("hduser" is your ubuntu's name or your username)
8.turn off

Step 8: Create and Set data2
(same as those in Step6 and 7)

Step 9: Connect master and both data1 and data2
1.turn on master, data1 and data2
below are in master:
2.type "scp ~/.ssh/authorized_keys hduser@data1:/home/hduser/.ssh" to send authorized file to data1
3.type "scp ~/.ssh/authorized_keys hduser@data2:/home/hduser/.ssh" to send authorized file to data2
4.tye "ssh data1", "ssh data2" to try connection; type "exit" to exit connection
5.type "hdfs namenode -format"
6.type "start-all.sh" or "start-dfs.sh" + "start-yarn.sh"
7.type "jps" to see what services are acitvated('Namenode','JPS',resoucemManager' should been activated)
8.type "slaves.sh jps" to see what services on data1/data2 are activated('Datanode','JPS',resoucemManager' should been activated)
