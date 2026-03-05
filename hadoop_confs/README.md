# Hadoop configurations

This Readme has the configuration files in hadoop, That I changed and modified to run the hadoop cluster:

- hdfs-site.xml
- core-site.xml
- yarn-site.xml
- hadoop-env.sh
- /etc/profile.d/hadoop.sh

----

I will also provide detailed documnetation about overview of the concept and project, setup of cluster, configuration files and some challenges that I faced:
## 1) Overview of project

- Explain the concept of HA and how to apply it
- My architecture design where it defines the location of each processes in each node
- Architecture Design advantages and disadvantage
- Some concerns to note about

## 2) Setup of my HDFS cluster 

- Mainly concerned about setup steps to download the prerequisite software before configuring HDFS, YARN, and Zookeeper


## 3) Configuration of HA HDFS

- Talks about what I have done mainly in configuring hadoop
- setup of hadoop Paths and exports like hadoop-env.sh, /etc/profile.d/hadoop.sh, ...etc
- setting up the configuraions file like hdfs-site.xml, core-site.xml, yarn-site.xml
- Steps and commands to run the cluster
- output of my cluster with testing failover


## 4) COnfiguration of HA Zookeeper

- Setup of Zookeeper and zoo.cfg file
- myid file purpose
- zkfc command meaning
