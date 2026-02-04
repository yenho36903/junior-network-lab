# junior-network-lab
 이 저장소는 신입 취준생이 Cisco Packet Tracer를 활용하여 네트워크 기초 실습을 수행한 포트폴리오입니다.
 
# 기본 실습 1 : pc 2대 + switch

# 실습 목표
같은 네트워크면 통신됨
네트워크가 다르면 통신 안 됨
이 판단 기준이 IP + Subnet Mask
##네트워크 구성 
-pc 2대 
-switch 1eo 2960
-케이블 : Copper Straight-Through
##사용한 프로그램
Cisco Packet Tracer

##실습 방법
1.처음에 장비 배치하고 케이블 연결하기 
 Copper Straight-Through 선택
	PC0 → Switch (Fa0/1) , PC1 → Switch (Fa0/2)
2.IP 수동 설정
PC1
	• Desktop → IP Configuration
	• IP Address : 192.168.10.10
	• Subnet Mask : 255.255.255.0
	• Default Gateway : 비워둠
PC2
	• IP Address : 192.168.10.20
	• Subnet Mask : 255.255.255.0
	• Default Gateway : 비워둠
3.ping 결과 확인하기 
pc1 -> pc2 , pc2 -> pc1

#기본 실습 2 : 1. pc 2 + switch 에 + router (DHCP)

## 실습 목표 
같은 네트워크에서는 통신 가능
통신 여부 판단 기준: **IP Address + Subnet Mask**
Router를 **Default Gateway + DHCP 서버**로 사용
#사용한 프로그램 
Cisco Packet Tracer
##  네트워크 구성
- PC 2대
- Switch 1대 2960
- Router 1대 2911
- 케이블: Copper Straight-Through
#실습방법 
기본 실습 1번에 routter 2911 하나 추가하기
1.수동 설정
enable
configure terminal
interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
2.동 설정 (DHCP 서버 설정)
configure terminal
! DHCP로 주지 않을 주소 제외 (Gateway 등)
ip dhcp excluded-address 192.168.10.1 192.168.10.99
! DHCP Pool 생성
ip dhcp pool LAN_POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit
PC1 / PC2 공통
Desktop → IP Configuration
→ DHCP 선택
PC → Command Prompt
## 개념설명하기
### Default Gateway
- 서로 다른 네트워크로 통신하기 위해 반드시 필요한 장비
- 이 실습에서는 Router의 LAN 인터페이스 IP가 Default Gateway 역할을 함
- PC는 자신의 네트워크가 아닌 목적지로 패킷을 보낼 때 Router로 전달함
### DHCP (Dynamic Host Configuration Protocol)
- 네트워크 장비에 IP 정보를 자동으로 할당하는 프로토콜
- Router에서 DHCP 서버 기능을 활성화하여 PC에 다음 정보를 자동 제공
  - IP Address
  - Subnet Mask
  - Default Gateway
  - DNS Server
### Subnet Mask
- IP 주소에서 네트워크 영역과 호스트 영역을 구분하는 기준
- PC1과 PC2는 같은 Subnet Mask(/24)를 사용하므로 같은 네트워크로 판단됨
### 통신 흐름 요약
1. PC가 DHCP 요청(DHCP Discover)을 전송
2. Router가 IP 정보를 자동 할당
3. PC는 Router를 Default Gateway로 설정
4. 같은 네트워크 내에서는 Switch를 통해 직접 통신
##ip 확인하기
ipconfig
✅ 정상일 때
	• IP Address : 192.168.10.xxx
	• Subnet Mask : 255.255.255.0
	• Default Gateway : 192.168.10.1
🔁 통신 테스트
ping 192.168.10.1
ping PC1 ↔ PC2

