# junior-network-lab
 이 저장소는 신입 취준생이 Cisco Packet Tracer를 활용하여 네트워크 기초 실습을 수행한 포트폴리오입니다.
 # 목차
1. 1월 실습 1 — PC+Switch
2. 1월 실습 2 — Router + DHCP
3. 1월 실습 3 — 장애 해결
4. 2월 실습 1: VLAN 실습
5. 2월 실습 2: NAT 실습
6.    
# 1월 실습 1 : pc 2대 + switch

# 실습 목표
같은 네트워크면 통신됨
네트워크가 다르면 통신 안 됨
이 판단 기준이 IP + Subnet Mask
## 네트워크 구성 
-pc 2대 
-switch 1eo 2960
-케이블 : Copper Straight-Through
##사용한 프로그램
Cisco Packet Tracer

## 실습 방법
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

# 1월 실습 2 : 1. pc 2 + switch 에 + router (DHCP)

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
## 실습방법 
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

# 1월 실습 3 : 1,2 번 실습 장애해결 (잘못된 ip주소에 의한 장애)

## 실습 목표 : pc에  잘못된 ip주소 할당하고 P 주소 또는 Subnet Mask가 잘못 설정되면 통신이 되지 않음을 확인
ipconfig, ping을 이용해 문제를 진단
올바른 IP 설정으로 장애 해결
## 실습 구성
PC 2대
Switch 2960
(실습 2라면 Router 2911 + DHCP 포함)
케이블: Copper Straight-Through
## 실습하기
1. pc2에 IP주소 잘못 설정하기 (192.168.2.20/24)
2. PC1
IP : 192.168.10.10
Subnet Mask : 255.255.255.0
PC2 (잘못 설정)
IP : 192.168.2.20 ❌
Subnet Mask : 255.255.255.0
👉 네트워크 대역이 서로 다름
3. pc1 에 pc2 ping 보내기
4. 통신 실패 확인후 다시 올바른 IP주소 설정
5. 다시 ping 보내 확인하기

# 2월 실습 1 : VLAN 실습

## 실습 목표 : pc 4대 를 2대씩 VLAN 하나씩 지정
Switch 하나에 여러 PC가 있어도 VLAN을 나누면 서로 다른 네트워크처럼 동작함을 확인
VLAN은 논리적으로 네트워크를 분리하는 기술임을 이해
PC1, PC2 → VLAN 10
PC3, PC4 → VLAN 20
## 네트워크 구성 : 
-PC 4대
- switch 2960-24 한대
- router 2811 한대
-케이블: Copper Straight-Through
## 실습하기 
1.포트연결 
PC1 → Fa0/1
PC2 → Fa0/2
PC3 → Fa0/3
PC4 → Fa0/4
swich - fa0/5
router - fa0/0
2.VLAN 생성
switch 
enable
conf t 
vlan 10 
name A -> exit
vlan 20 
name B -> end 
예: VLAN 10, VLAN 20
3.포트 할당
PC1, PC2 → VLAN 10
PC3, PC4 → VLAN 20
int fa0/1
#switchport mode access
#switchport access vlan 10
#int fa0/2
#switchport mode access
#switchport access vlan 10
#end
conf t
#int range fa0/3-4
#switchport mode access
#switchport access vlan 20
#end
4.트렁크 포트 할당하기
Switch(config)#int fa0/5
Switch(config-if)#switchport mode trunk
5.router ip 설정
interface fa0/0
 no shutdown
 exit

interface fa0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 exit

interface fa0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 exit
end
## 개념설명하기 
### VLAN 
-하나의 물리적 스위치를 논리적으로 분할하여 여러 개의 독립적인 네트워크(가상 LAN)를 만드는 기술
-가상의 공간에서 네트워크를 만들어내는 기술
-주요 역할과 장점 
네트워크 분할 및 성능 향샹 ,보안 강화 ,유연한 네트워크 구성 및 관리 ,트래픽 관리 ,비용 절감 
## 실습자료 출처
https://gwnuysw.github.io/jekyll/update/2019/01/08/ciscoNetwork.html
# 2월 실습 2 : NAT 실습

## 실습 목표 : NAT 설정하기
IPv4의 고갈문제 해결와 사설망 구축하기
## 네트워크 구성 : 
- PC 2대
- Switch 2960 한대
- Router 2911 두대
## 실습하기 
1.포트 연결,IP 주소 재정리
장치	인터페이스	IP 주소	서브넷 마스크	역할
PC1	FastEthernet0	10.0.0.10	255.255.255.0	내부 PC
PC2	FastEthernet0	10.0.0.11	255.255.255.0	내부 PC
R1	Gi0/0	10.0.0.1	255.255.255.0	내부 LAN (NAT inside)
R1	Gi0/1	172.16.0.1	255.255.255.0	외부 LAN (NAT outside)
R2	Gi0/0	172.16.0.2	255.255.255.0	외부 LAN
R2	loopback0	1.1.1.1	255.255.255.255	테스트용 루프백
2.인터페이스 IP 설정
R1 설정 (내부 LAN + NAT) 
내부 LAN 설정 (PC 설정) 
en
config t
interface GigabitEthernet0/0 
ip address 10.0.0.1 255.255.255.0 ip nat inside
no shutdown 
exit 
! 외부 LAN 설정 (R2 연결) 
en
config t
interface GigabitEthernet0/1 
ip address 172.16.0.1 255.255.255.0 
ip nat outside 
no shutdown
exit 
NAT 설정
access-list 1 permit 10.0.0.0 0.0.0.255 
ip nat inside source list 1 interface GigabitEthernet0/1 overload 
! 기본 라우팅 (필요 시) 
ip route 0.0.0.0 0.0.0.0 172.16.0.2
R2 설정 (외부 LAN + 루프백 테스트) 
enable
configure terminal 
! R1 연결 포트 
interface GigabitEthernet0/0 ip address 
172.16.0.2 255.255.255.0 no shutdown exit
! 테스트용 루프백 interface Loopback0 
ip address 1.1.1.1 255.255.255.255 
exit 
!기본 라우팅 (R1로 돌아가는 경로) 
ip route 10.0.0.0 255.255.255.0 172.16.0.1
PC1로 테스트 (대표로 하나) 
PC1에서: ping 1.1.1.1 R1에서: show ip nat translations 
예시: Inside local 10.0.0.10 → Inside global 172.16.0.1 
PC2로 테스트 (PAT 증명용) PC2에서: ping 1.1.1.1
->R1 NAT 테이블에: 10.0.0.10 10.0.0.11

## 개념설명하기 
### NAT이란 ? 
- Network Address Translation, 네트워크 주소 변환
- 라우터나 방화벽 등의 장비에서 사설 IP 주소(Internal)를 공인 IP 주소(Public)로, 또는 그 반대로 1:1 또는 N:1로 변환하는 기술
- 주된 목적: IPv4 주소 고갈 문제를 해결하기 위한 공인 IP 절약과, 내부 네트워크 구조를 숨겨 보안을 강화
## 실습자료 출처
https://m.blog.naver.com/kimdj217/221267672672
