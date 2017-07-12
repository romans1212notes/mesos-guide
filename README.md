# Mesos-Guide

## Setup
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E56151BF
DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
CODENAME=$(lsb_release -cs)
```

## Add the repository
```
echo "deb http://repos.mesosphere.com/${DISTRO} ${CODENAME} main" | \
sudo tee /etc/apt/sources.list.d/mesosphere.list
sudo apt-get -y update
```

## Install mesos and marathon
```
sudo apt-get -y install mesos marathon
```

## Start mesos master
```
sudo /usr/sbin/mesos-master --ip=127.0.0.1 --work_dir=/tmp/
```
After mesos master is up, you can go to the web UI: http://localhost:5050

## Start mesos agent
```
sudo /usr/sbin/mesos-agent --master=127.0.0.1:5050 --work_dir=/tmp/
```

## Start marathon
```
# marathon needs zookeeper even if we don't specify (it will use a default one)
# check whether zookeeper is running: 
#     ps aux | grep zookeeper
# If zookeeper is not running, see the next section.
sudo /usr/bin/marathon --master=localhost:5050
```
After marathon is up, you can go to the web UI: http://localhost:8080

## Problem with Zookeeper on Ubuntu 16.04
If you couldn't find the zookeeper process, e.g. "ps aux | grep zookeeper" doesn't show the zookeeper process. Try this:
```
$ sudo su
# apt-get remove openjdk-9-jre-headless
# echo server.1=$(hostname):2888:3888 >> /etc/zookeeper/conf/zoo.cfg

# systemctl restart zookeeper
# systemctl status zookeeper
● zookeeper.service - LSB: centralized coordination service
   Loaded: loaded (/etc/init.d/zookeeper; bad; vendor preset: enabled)
   Active: active (running) since Tue 2017-07-11 19:29:21 UTC; 3s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 6605 ExecStop=/etc/init.d/zookeeper stop (code=exited, status=0/SUCCESS)
  Process: 6616 ExecStart=/etc/init.d/zookeeper start (code=exited, status=0/SUCCESS)
    Tasks: 15
   Memory: 41.8M
      CPU: 588ms
   CGroup: /system.slice/zookeeper.service
           └─6630 /usr/bin/java -cp /etc/zookeeper/conf:/usr/share/java/jline.jar:/usr/share/java/log4j-1.2.jar:/usr/share/java/xercesImpl.jar:/usr/share/java/x

# ps aux | grep zookeeper
zookeep+  6630  3.8  4.6 2173024 46928 ?       Sl   19:29   0:00 /usr/bin/java -cp /etc/zookeeper/conf:/usr/share/java/jline.jar:/usr/share/java/log4j-1.2.jar:/usr/share/java/xercesImpl.jar:/usr/share/java/xmlParserAPIs.jar:/usr/share/java/netty.jar:/usr/share/java/slf4j-api.jar:/usr/share/java/slf4j-log4j12.jar:/usr/share/java/zookeeper.jar -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.local.only=false -Dzookeeper.log.dir=/var/log/zookeeper -Dzookeeper.root.logger=INFO,ROLLINGFILE org.apache.zookeeper.server.quorum.QuorumPeerMain /etc/zookeeper/conf/zoo.cfg
```

## Start with nohup
```
nohup /usr/sbin/mesos-master --ip=xx.xx.xx.xx --log_dir=/var/log/mesos --work_dir=/var/lib/mesos &
nohup /usr/sbin/mesos-slave --master=xx.xx.xx.xx:5050 --work_dir=/var/lib/mesos & 
nohup /usr/bin/marathon --master=xx.xx.xx.xx:5050 &
```
