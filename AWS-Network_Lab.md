# AWS Network Lab
https://www.notion.so/gasidaseo/CloudNet-Blog-c9dfa44a27ff431dafdd2edacc8a1863

## 1장

CloudFormation 템플릿   
http://bit.ly/cnbl0102

## 2장

Private Subnet 접속을 위해 계정 접속 허용 설정   
http://bit.ly/cnbl0202
```
#!/bin/bash
(
echo "qwe123"
echo "qwe123"
) | passwd --stdin root
sed -i "s/^PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
sed -i "s/^#PermitRootLogin yes/PermitRootLogin yes/g" /etc/ssh/sshd_config
service sshd restart
```

## 3장 VPC 
https://www.notion.so/VPC-3-c561be69fa064d38ae6191687dc8994b
### 게이트웨이 엔드포인트와 인터페이스 엔드포인트   
https://www.notion.so/ongja/VPC-d52f592d463e40628ab44555f240e060#5cd143e125b64bc9add4448030bc3225

http://bit.ly/cnbl0301   

### 엔드포인트 서비스
https://www.notion.so/ongja/VPC-d52f592d463e40628ab44555f240e060#59318a438fb4483b9641c207a1be53ab   
http://bit.ly/cnbl0301 

## 4장 인터넷 연결
https://www.notion.so/Internet-Connectivity-5-e43c78a8e81347a3bc4d2cc218d9782a   

### NAT 인스턴스
http://bit.ly/cnbl0401


## 5장 부하 분산
### ELB 
https://www.notion.so/ongja/ELB-Elastic-Load-Balancing-6f5de514ca074ea08477e0c2c635a4fc   
http://bit.ly/cnbl0401


