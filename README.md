# junior-network-lab
 이 저장소는 신입 취준생이 Cisco Packet Tracer를 활용하여 네트워크 기초 실습을 수행한 포트폴리오입니다.
 
# 기본 실습 : pc 2대 + switch

# 실습 목표
같은 네트워크면 통신됨
네트워크가 다르면 통신 안 됨
이 판단 기준이 IP + Subnet Mask

#사용한 프로그램
Cisco Packet Tracer

#실습 방법
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
