# Hadoop SETUP Hands-On (on VirtualBox) 

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

- 설치경로 : /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64/jre 

#### (2) JAVA_HOME 설정

| <img src="/VM_Hands_On/images/2-3.png"  width="800" /> | 
| ------------------------------------------------- | 

````
[root@skcc ~]# vi ~/.bash_profile 

export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64
PATH=$PATH:$HOME/.local/bin:$HOME/bin:$JAVA_HOME/bin

````

### 2) Hadoop 계정 생성

#### (1) 계정 생성 및 비밀번호 설정

````
[root@skcc ~]# useradd hadoop

[root@skcc ~]# passwd hadoop
````

#### (2) 로그인

````
[root@skcc ~]# su hadoop
````

#### (3) ssh-key 생성

````
[hadoop@skcc]$ ssh-keygen -t rsa

[hadoop@skcc]$ cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
````

#### (4) Datanode에 key 복사

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


## 3. 하둡 설치

### 1) 설치파일 다운로드

````
[hadoop@skcc ~]$ cd ~

[hadoop@skcc ~]$ wget http://apache.mirror.cdnetworks.com/hadoop/common/hadoop-2.8.5/hadoop-2.8.5.tar.gz

[hadoop@skcc ~]$ tar xvf hadoop-2.8.5.tar.gz
````

### 2) 하둡 클러스터 설정

- 설정파일 경로 : /home/hadoop/hadoop-2.8.5/etc/hadoop

#### (1) hdfs-site.xml

````
[hadoop@skcc]$ vi hdfs-site.xml

<configuration>
  <property>
    <name>dfs.replication</name>
    <value>2</value>
  </property>
</configuration>
````

#### (2) mapred-site.xml

````
[hadoop@skcc]$ vi mapred-site.xml

<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
````

#### (3) yarn-site.xml

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

#### (4) core-site.xml

````
[hadoop@skcc]$ vi core-site.xml

<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://skcc/</value>
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

### 2) 설정파일 

- 설정파일 복사

````
[hadoop@skcc ~]$ scp /home/hadoop/hadoop-2.8.5/etc/hadoop/* hadoop@skcc-data01:/home/hadoop/hadoop-2.8.5/etc/hadoop

[hadoop@skcc ~]$ scp /home/hadoop/hadoop-2.8.5/etc/hadoop/* hadoop@skcc-data02:/home/hadoop/hadoop-2.8.5/etc/hadoop
````


