# Hadoop SETUP Hands-On (on VirtualBox) 


## 1. VirtualBox 설정

#### 1) VirtualBox 설치

- https://www.virtualbox.org/

#### 2) VM Image 다운로드

- NameNode : https://drive.google.com/file/d/1ONOFMtwIQxhYAoqcuKrseXPb2S5KXf35/view?usp=sharing
- DataNode 01 : https://drive.google.com/file/d/1gNKftzDqaz5YfKaE2XFBX5_YMpkOU0Pd/view?usp=sharing
- DataNode 02 : https://drive.google.com/file/d/18HjOEqPRnLCx-VPVdfkfXceZGWAy4vmP/view?usp=sharing

#### 3) 네트워크 설정

<img src="/VM_Hands_On/images/1-1.png"  width="400" /> <img src="/VM_Hands_On/images/1-2.png"  width="400" />


- VirtureBox 네트워크 설정에서 호스트 전용 어뎁터 신규 생성
````
수동으로 어댑터 설정

IPv4 주소 : 192.168.137.1
서브넷 마스크 : 255.255.255.0

DHP 서버는 사용하지 않음 (체크 해제)
````

