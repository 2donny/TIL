# AWS VPC

## What VPC(Virtual private cloud) stands for?
It's my own network in the cloud.

---

## 학습 배경
- EC2 인스턴스 설정할 때 'Network', 'Subnet' 탭에서 VPC가 언급이 되어서 공부를 하게되었다.

## 쓰는 이유
- 사용자가 가상 네트워크로 AWS 리소스를 사용할 수 있다.

<br />

## 다이어그램
<img src="./스크린샷%202021-09-02%20오후%207.38.48.png"/>

<br />

## 용어
1. Region - 인스턴스가 위치하고 있는 물리적인 지역
2. AZ(Availability Zone) - 인스턴스가 위치해있는 Data center 빌딩
3. Subnet - VPC 안에 있는 Sub network이다.
   1. 하나의 Subnet은 하나의 AZ가 될 수 있다.
   2. VPC에 대한 IP주소 범위이다
   3. 인터넷 통신이 필요할 때는 public subnet을 사용하고, 인터넷 연결이 필요하지 않는 서버에 경우 private subnet을 사용한다.
4. CIDR - VPC에 대한 IP주소 범위를 지정하는 블록이다.
   1. VPC 생성시에 만든다.
   2. 만약 10.0.0.0/24으로 설정하면 10.0.x.x로 시작하는 IP가 들어올 수 있다.



