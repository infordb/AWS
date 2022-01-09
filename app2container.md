

### install ec2 + sg + httpd
```
Parameters:
  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instances. Linked to AWS Parameter
    Type: AWS::EC2::KeyPair::KeyName
    ConstraintDescription: must be the name of an existing EC2 KeyPair.
  LatestAmiId:
    Description: (DO NOT CHANGE)
    Type: 'AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>'
    Default: '/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2'
    AllowedValues:
      - /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref LatestAmiId
      InstanceType: t2.micro
      KeyName: !Ref KeyName
      Tags:
        - Key: Name
          Value: WebServer
      SecurityGroups:
        - !Ref MySG
      UserData:
        Fn::Base64:
          !Sub |
            #!/bin/bash
            yum install httpd -y
            systemctl start httpd && systemctl enable httpd
            echo "<h1>Test Web Server</h1>" > /var/www/html/index.html

  MySG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable HTTP access via port 80 and SSH access via port 22
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: 0.0.0.0/0
      - IpProtocol: tcp
        FromPort: 22
        ToPort: 22
        CidrIp: 0.0.0.0/0
      - IpProtocol: tcp
        FromPort: 8080
        ToPort: 8080
        CidrIp: 0.0.0.0/0
 ```

### install java
https://kitty-geno.tistory.com/25?category=929861  
   
   
   
### install tomcat
https://kitty-geno.tistory.com/26

### app2container inventory

```

[root@ip-172-31-32-91 work]# app2container inventory
{
                "java-tomcat-38390f84": {
                                "processId": 8983,
                                "cmdline": "/usr/lib/jvm/java-1.8.0-openjdk/bin/java ... -Dcatalina.home=/usr/local/tomcat8.5 -Djava.io.tmpdir=/usr/local/tomcat8.5/temp org.apache.catalina.startup.Bootstrap start ",
                                "applicationType": "java-tomcat",
                                "webApp": ""
                }
}

```


```
[root@ip-172-31-32-91 java-tomcat-38390f84]# cat /root/work/java-tomcat-38390f84/analysis.json
{
       "a2CTemplateVersion": "1.0",
       "createdTime": "2022-01-09 02:30:389",
       "containerParameters": {
              "_comment1": "*** EDITABLE: The below section can be edited according to the application requirements. Please see the analysisInfo section below for details discovered regarding the application. ***",
              "imageRepository": "java-tomcat-38390f84",
              "imageTag": "latest",
              "containerBaseImage": "public.ecr.aws/amazonlinux/amazonlinux:2",
              "appExcludedFiles": [
                     "/root/.aws"
              ],
              "appSpecificFiles": [],
              "applicationPort": 8080,
              "applicationMode": true,
              "logLocations": [],
              "enableDynamicLogging": false,
              "dependencies": []
       },
       "analysisInfo": {
              "_comment2": "*** NON-EDITABLE: Analysis Results ***",
              "processId": 8983,
              "appId": "java-tomcat-38390f84",
              "userId": "0",
              "groupId": "0",
              "cmdline": [
                     "/usr/lib/jvm/java-1.8.0-openjdk/bin/java",
                     "-Djava.util.logging.config.file=/usr/local/tomcat8.5/conf/logging.properties",
                     "-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager",
                     "-Djdk.tls.ephemeralDHKeySize=2048",
                     "-Djava.protocol.handler.pkgs=org.apache.catalina.webresources",
                     "-Dorg.apache.catalina.security.SecurityListener.UMASK=0027",
                     "-Dignore.endorsed.dirs=",
                     "-classpath",
                     "/usr/local/tomcat8.5/bin/bootstrap.jar:/usr/local/tomcat8.5/bin/tomcat-juli.jar",
                     "-Dcatalina.base=/usr/local/tomcat8.5",
                     "-Dcatalina.home=/usr/local/tomcat8.5",
                     "-Djava.io.tmpdir=/usr/local/tomcat8.5/temp",
                     "org.apache.catalina.startup.Bootstrap",
                     "start"
              ],
              "webApp": "",
              "osData": {
                     "ANSI_COLOR": "0;33",
                     "CPE_NAME": "cpe:2.3:o:amazon:amazon_linux:2",
                     "HOME_URL": "https://amazonlinux.com/",
                     "ID": "amzn",
                     "ID_LIKE": "centos rhel fedora",
                     "NAME": "Amazon Linux",
                     "PRETTY_NAME": "Amazon Linux 2",
                     "VERSION": "2",
                     "VERSION_ID": "2"
              },
              "osName": "amzn",
              "ports": [
                     {
                            "localPort": 8080,
                            "protocol": "tcp6"
                     },
                     {
                            "localPort": 8005,
                            "protocol": "tcp6"
                     }
              ],
              "Properties": {
                     "catalina.base": "/usr/local/tomcat8.5",
                     "catalina.home": "/usr/local/tomcat8.5",
                     "classpath": "/usr/local/tomcat8.5/bin/bootstrap.jar:/usr/local/tomcat8.5/bin/tomcat-juli.jar",
                     "ignore.endorsed.dirs": "",
                     "java.io.tmpdir": "/usr/local/tomcat8.5/temp",
                     "java.protocol.handler.pkgs": "org.apache.catalina.webresources",
                     "java.util.logging.config.file": "/usr/local/tomcat8.5/conf/logging.properties",
                     "java.util.logging.manager": "org.apache.juli.ClassLoaderLogManager",
                     "jdk.tls.ephemeralDHKeySize": "2048",
                     "jdkVersion": "1.8.0_312",
                     "org.apache.catalina.security.SecurityListener.UMASK": "0027"
              },
              "applicationType": "java-tomcat",
              "AdvancedAppInfo": {
                     "Directories": {
                            "base": "/usr/local/tomcat8.5",
                            "bin": "/usr/local/tomcat8.5/bin",
                            "conf": "/usr/local/tomcat8.5/conf",
                            "home": "/usr/local/tomcat8.5",
                            "lib": "/usr/local/tomcat8.5/lib",
                            "logConfig": "/usr/local/tomcat8.5/conf/logging.properties",
                            "logs": "/usr/local/tomcat8.5/logs",
                            "tempDir": "/usr/local/tomcat8.5/temp",
                            "webapps": "/usr/local/tomcat8.5/webapps",
                            "work": "/usr/local/tomcat8.5/work"
                     },
                     "jdkVersion": "1.8.0_312"
              },
              "env": {
                     "CATALINA_HOME": "/usr/local/tomcat8.5",
                     "CLASSPATH": "/usr/local/tomcat8.5/bin/bootstrap.jar:/usr/local/tomcat8.5/bin/tomcat-juli.jar",
                     "HISTCONTROL": "ignoredups",
                     "HISTSIZE": "1000",
                     "HOME": "/root",
                     "HOSTNAME": "ip-172-31-32-91.ap-northeast-2.compute.internal",
                     "JAVA_HOME": "/usr/lib/jvm/java-1.8.0-openjdk",
                     "JDK_JAVA_OPTIONS": " --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED",
                     "LANG": "en_US.UTF-8",
                     "LESSOPEN": "||/usr/bin/lesspipe.sh %s",
                     "LOGNAME": "root",
                     "LS_COLORS": "rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=01;05;37;41:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.axv=01;35:*.anx=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=01;36:*.au=01;36:*.flac=01;36:*.mid=01;36:*.midi=01;36:*.mka=01;36:*.mp3=01;36:*.mpc=01;36:*.ogg=01;36:*.ra=01;36:*.wav=01;36:*.axa=01;36:*.oga=01;36:*.spx=01;36:*.xspf=01;36:",
                     "MAIL": "/var/spool/mail/root",
                     "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/usr/lib/jvm/java-1.8.0-openjdk/bin:/usr/lib/jvm/java-1.8.0-openjdk/bin",
                     "PWD": "/root",
                     "SHELL": "/bin/bash",
                     "SHLVL": "3",
                     "TERM": "xterm",
                     "USER": "root",
                     "XDG_SESSION_ID": "1",
                     "_": "/usr/lib/jvm/java-1.8.0-openjdk/bin/java"
              },
              "cwd": "/root",
              "exe": "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/bin/java",
              "procUID": {
                     "euid": "0",
                     "suid": "0",
                     "fsuid": "0",
                     "ruid": "0"
              },
              "procGID": {
                     "egid": "0",
                     "sgid": "0",
                     "fsgid": "0",
                     "rgid": "0"
              },
              "userNames": {
                     "0": "root"
              },
              "groupNames": {
                     "0": "root"
              },
              "fileDescriptors": [
                     "/usr/lib/jvm/java-1.8.0-openjdk",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/ext/cldrdata.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/ext/localedata.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/ext/sunec.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/ext/sunjce_provider.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/jce.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/jfr.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/jsse.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/resources.jar",
                     "/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.amzn2.0.2.x86_64/jre/lib/rt.jar",
                     "/usr/local/tomcat8.5/bin/bootstrap.jar",
                     "/usr/local/tomcat8.5/bin/commons-daemon.jar",
                     "/usr/local/tomcat8.5/bin/tomcat-juli.jar",
                     "/usr/local/tomcat8.5/lib/annotations-api.jar",
                     "/usr/local/tomcat8.5/lib/catalina-ant.jar",
                     "/usr/local/tomcat8.5/lib/catalina-ha.jar",
                     "/usr/local/tomcat8.5/lib/catalina-storeconfig.jar",
                     "/usr/local/tomcat8.5/lib/catalina-tribes.jar",
                     "/usr/local/tomcat8.5/lib/catalina.jar",
                     "/usr/local/tomcat8.5/lib/ecj-4.6.3.jar",
                     "/usr/local/tomcat8.5/lib/el-api.jar",
                     "/usr/local/tomcat8.5/lib/jasper-el.jar",
                     "/usr/local/tomcat8.5/lib/jasper.jar",
                     "/usr/local/tomcat8.5/lib/jaspic-api.jar",
                     "/usr/local/tomcat8.5/lib/jsp-api.jar",
                     "/usr/local/tomcat8.5/lib/servlet-api.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-api.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-coyote.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-dbcp.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-de.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-es.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-fr.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-ja.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-ko.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-ru.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-i18n-zh-CN.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-jdbc.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-jni.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-util-scan.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-util.jar",
                     "/usr/local/tomcat8.5/lib/tomcat-websocket.jar",
                     "/usr/local/tomcat8.5/lib/websocket-api.jar",
                     "/usr/local/tomcat8.5/logs/catalina.2022-01-09.log",
                     "/usr/local/tomcat8.5/logs/catalina.out",
                     "/usr/local/tomcat8.5/logs/host-manager.2022-01-09.log",
                     "/usr/local/tomcat8.5/logs/localhost.2022-01-09.log",
                     "/usr/local/tomcat8.5/logs/localhost_access_log.2022-01-09.txt",
                     "/usr/local/tomcat8.5/logs/manager.2022-01-09.log",
                     "/usr/share/log4j-cve-2021-44228-hotpatch/jdk8/Log4jHotPatch.jar"
              ],
              "dependencies": {}
       }
```

   
   
