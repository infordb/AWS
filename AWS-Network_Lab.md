

## 1장

CloudFormation 템플릿   
http://bit.ly/cnbl0102

## 2장

Private Subnet 접속을 위해 계정 접속 허용 설정
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
