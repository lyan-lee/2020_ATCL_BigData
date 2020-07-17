# Hadoop SETUP Hands-On (on VirtualBox) 


## 1. VirtualBox 설정

#### 1) VirtualBox 설치

- https://www.virtualbox.org/

#### 2) VirtualBox 네트워크 설정

- VirtureBox 네트워크 설정에서 호스트 전용 어뎁터 신규 생성
<img src="/VM_Hands_On/images/1-1.png"  width="400" /> <img src="/VM_Hands_On/images/1-2.png"  width="400" />

````
수동으로 어댑터 설정

IPv4 주소 : 192.168.137.1
서브넷 마스크 : 255.255.255.0

DHP 서버는 사용하지 않음 (체크 해제)
````

#### 3) VM Image 다운로드

- NameNode : https://drive.google.com/file/d/1ONOFMtwIQxhYAoqcuKrseXPb2S5KXf35/view?usp=sharing
- DataNode 01 : https://drive.google.com/file/d/1gNKftzDqaz5YfKaE2XFBX5_YMpkOU0Pd/view?usp=sharing
- DataNode 02 : https://drive.google.com/file/d/18HjOEqPRnLCx-VPVdfkfXceZGWAy4vmP/view?usp=sharing


#### 4) VM Image 불러오기 후 네트워크 설정 

<img src="/VM_Hands_On/images/1-3.png"  width="400" /> <img src="/VM_Hands_On/images/1-4.png"  width="400" />

- 어댑터 1 : 호스트 전용 어댑터 (VM간 통신)
- 어댑터 2 : 어댑터에 브리지 (외부 인터넷 연결)
- NameNode, DataNode 모두 동일하게 설정

#### 5) VM 실행 후 네트워크 확인
- ID : root / PW : skcc1234
- IP 주소 확인
<img src="/VM_Hands_On/images/1-5.png"  width="600" />
````
enp0s17에 192.168.137.100이 매핑 되었는지 확인

NAT 설정한 경우 enp0s8에 10번 대역이 설정되었는지 확인 
````
- 외부 네트워크 확인
<img src="/VM_Hands_On/images/1-6.png"  width="600" />

