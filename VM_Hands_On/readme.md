# Hadoop SETUP Hands-On (on VirtualBox) 

- NameNode 1대, DataNode 2대 클러스터 구성  
        

## 1. VirtualBox 설정

### 1) VirtualBox 설치

- https://www.virtualbox.org/



### 2) VirtualBox 네트워크 설정

- VirtureBox 네트워크 설정에서 호스트 전용 어뎁터 신규 생성

| <img src="/VM_Hands_On/images/1-1.png"  width="400" /> | <img src="/VM_Hands_On/images/1-2.png"  width="400" /> |
| ------------------------------------------------- | ------------------------------------------------- |

````
- 수동으로 어댑터 설정
  IPv4 주소 : 192.168.137.1
  서브넷 마스크 : 255.255.255.0
- DHP 서버는 사용하지 않음 (체크 해제)
````

### 3) VM Image 다운로드

- NameNode : [다운로드](https://drive.google.com/file/d/1ONOFMtwIQxhYAoqcuKrseXPb2S5KXf35/view?usp=sharing)
- DataNode 01 : [다운로드](https://drive.google.com/file/d/1gNKftzDqaz5YfKaE2XFBX5_YMpkOU0Pd/view?usp=sharing)
- DataNode 02 : [다운로드](https://drive.google.com/file/d/18HjOEqPRnLCx-VPVdfkfXceZGWAy4vmP/view?usp=sharing)


### 4) VM Image 불러오기 후 네트워크 설정 

| <img src="/VM_Hands_On/images/1-3.png"  width="400" /> | <img src="/VM_Hands_On/images/1-4.png"  width="400" /> |
| ------------------------------------------------- | ------------------------------------------------- |

````
- 어댑터 1 : 호스트 전용 어댑터 (VM간 통신)
- 어댑터 2 : 어댑터에 브리지 (외부 인터넷 연결)
  ※ 어댑터에 브리지는 공유기 환경에서만 가능, 그 외 환경에서는 NAT로 연결
- NameNode, DataNode 모두 동일하게 설정
````

### 5) VM Image 실행 및 네트워크 확인

- ID : root / PW : skcc1234

- IP 주소 확인

| <img src="/VM_Hands_On/images/1-5.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# ip addr

- enp0s17에 192.168.137.100이 매핑 되었는지 확인
- NAT 설정한 경우 enp0s8에 10번 대역이 설정되었는지 확인 
````

- 외부 네트워크 확인

| <img src="/VM_Hands_On/images/1-6.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# ping google.com
````


## 2. 사전작업

- NameNode, DataNode 모든 서버에서 작업 필요.

### 1) JAVA 설치 

#### (1) JDK 1.8 설치

| <img src="/VM_Hands_On/images/2-1.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# yum install java-1.8.0-openjdk-devel
````

| <img src="/VM_Hands_On/images/2-2.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# java -version
````

#### (3) JAVA 설치경로 확인

````
[root@skcc ~]# which javac
/usr/bin/javac

[root@skcc ~]# readlink -f /usr/bin/javac
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64/bin/javac
````

- 설치경로 : /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64

#### (4) bash_profile에 java_home경로 export

| <img src="/VM_Hands_On/images/2-3.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# vi ~/.bash_profile 

export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64
PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin
````

#### (5) bash_profile 값 적용

````
[root@skcc ~]# source ~/.bash_profile
````

### 2) Hadoop 계정 생성

#### (1) 계정 생성 및 비밀번호 설정

| <img src="/VM_Hands_On/images/2-4.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# useradd hadoop

[root@skcc ~]# passwd hadoop
````

#### (2) 로그인


| <img src="/VM_Hands_On/images/2-5.png"  width="800" /> | 
| ------------------------------------------------- | 


````
[root@skcc ~]# su hadoop
````

#### (3) ssh-key 생성


| <img src="/VM_Hands_On/images/2-6.png"  width="800" /> | 
| ------------------------------------------------- | 


````
[hadoop@skcc]$ ssh-keygen -t rsa

[hadoop@skcc]$ cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
````

#### (4) Datanode에 key 복사

| <img src="/VM_Hands_On/images/2-7.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[hadoop@skcc ~]$ ssh hadoop@skcc-data01 'mkdir -p ~/.ssh'
[hadoop@skcc ~]$ ssh hadoop@skcc-data01 'cat >> ~/.ssh/authorized_keys' < ~/.ssh/id_rsa.pub
[hadoop@skcc ~]$ ssh hadoop@skcc-data01 'chmod 755 ~/.ssh | chmod 600 ~/.ssh/authorized_keys'

[hadoop@skcc ~]$ ssh hadoop@skcc-data02 'mkdir -p ~/.ssh'
[hadoop@skcc ~]$ ssh hadoop@skcc-data02 'cat >> ~/.ssh/authorized_keys' < ~/.ssh/id_rsa.pub
[hadoop@skcc ~]$ ssh hadoop@skcc-data02 'chmod 755 ~/.ssh | chmod 600 ~/.ssh/authorized_keys'
````

- Namenode -> Datanode 접속시 로그인 불필요
- 필요시 Datanode -> Namenode, Datanode -> Datanode 에도 복사



## 3. Hadoop

### 1) 설치파일 다운로드

````
[hadoop@skcc ~]$ cd ~

[hadoop@skcc ~]$ wget http://apache.mirror.cdnetworks.com/hadoop/common/hadoop-2.8.5/hadoop-2.8.5.tar.gz

[hadoop@skcc ~]$ tar xvf hadoop-2.8.5.tar.gz
````

### 2) Hadoop 환경설정

````
[hadoop@skcc ~]$ vi ~/.bash_profile

export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64
export HADOOP_HOME=/home/hadoop/hadoop-2.8.5

PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

````

### 3) Hadoop 클러스터 설정

- 설정파일 경로 : /home/hadoop/hadoop-2.8.5/etc/hadoop

#### (1) core-site.xml

````
[hadoop@skcc]$ vi core-site.xml

<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://skcc/</value>
  </property>
</configuration>
````

#### (2) hdfs-site.xml

````
[hadoop@skcc]$ vi hdfs-site.xml

<configuration>
  <property>
    <name>dfs.replication</name>
    <value>2</value>
  </property>
</configuration>
````

#### (3) mapred-site.xml

````
[hadoop@skcc]$ cp mapred-site.xml.template mapred-site.xml
[hadoop@skcc]$ vi mapred-site.xml

<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
````

#### (4) yarn-site.xml

````
[hadoop@skcc]$ vi yarn-site.xml

<configuration>

<!-- Site specific YARN configuration properties -->
  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>skcc</value>
  </property>
  <property>
    <name>yarn.nodemanager.aux-service</name>
    <value>mapreduce_shuffle</value>
  </property>
</configuration>
````

#### (5) slaves

- datanode 리스트 

````
[hadoop@skcc]$ vi slaves

skcc-data01
skcc-data02
````

#### (6) hadoop-env.sh

````
[hadoop@skcc]$ vi hadoop-env.sh

# The java implementation to use.
export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64
````

#### (7) 클러스터 설정파일 복사 

````
[hadoop@skcc ~]$ scp /home/hadoop/hadoop-2.8.5/etc/hadoop/* hadoop@skcc-data01:/home/hadoop/hadoop-2.8.5/etc/hadoop

[hadoop@skcc ~]$ scp /home/hadoop/hadoop-2.8.5/etc/hadoop/* hadoop@skcc-data02:/home/hadoop/hadoop-2.8.5/etc/hadoop
````


### 4) HDFS 파일 시스템 포맷하기 (NameNode)

````
[hadoop@skcc ~]$ hdfs namenode -format
````


### 5) Hadoop 구동

#### (1) 구동

| <img src="/VM_Hands_On/images/3-1.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[hadoop@skcc ~]$ start-dfs.sh
[hadoop@skcc ~]$ start-yarn.sh
````

| <img src="/VM_Hands_On/images/3-7.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[hadoop@skcc ~]$ mr-jobhistory-daemon.sh start historyserver
````

#### (2) Hadoop 데몬 정상기동여부 확인

| <img src="/VM_Hands_On/images/3-2.png"  width="800" /> | 
| ------------------------------------------------- | 
| <img src="/VM_Hands_On/images/3-3.png"  width="800" /> | 
| <img src="/VM_Hands_On/images/3-4.png"  width="800" /> | 


````
[hadoop@skcc ~]$ jps
````

#### (3) WEB UI

- NameNode : http://localhost:50070

| <img src="/VM_Hands_On/images/3-5.png"  width="800" /> | 
| ------------------------------------------------- | 

- ResourceManager : http://localhost:8088

| <img src="/VM_Hands_On/images/3-6.png"  width="800" /> | 
| ------------------------------------------------- | 

- HistoryServer : http://localhost:19888

| <img src="/VM_Hands_On/images/3-8.png"  width="800" /> | 
| ------------------------------------------------- | 


#### (4) 중지

````
[hadoop@skcc ~]$ stop-dfs.sh
[hadoop@skcc ~]$ stop-yarn.sh
[hadoop@skcc ~]$ mr-jobhistory-daemon.sh stop historyserver
````


### 6) Hadoop 예제

- 파일을 HDFS에 저장 후 WEB UI 통해서 확인하기
- Target Path : hdfs:///data/1500000_Sales_Records.csv
- 예제파일 : [다운로드](https://drive.google.com/file/d/1LAjD-n4lON0wuiI7mk2CBdbSC944E8Xx/view?usp=sharing)

#### (1) data 디렉토리 생성

| <img src="/VM_Hands_On/images/3-9.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[hadoop@skcc ~]$ hdfs dfs -mkdir /data
[hadoop@skcc ~]$ hdfs dfs -chmod 755 /data
````

#### (2) HDFS 저장

| <img src="/VM_Hands_On/images/3-10.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[hadoop@skcc ~]$ hdfs dfs -put ~/1500000_Sales_Records.csv /data 
[hadoop@skcc ~]$ hdfs dfs -ls /data
````

#### (3) WEB UI 확인

-  NameNode : http://localhost:50070/explorer.html#/data

| <img src="/VM_Hands_On/images/3-11.png"  width="800" /> | 
| ------------------------------------------------- | 