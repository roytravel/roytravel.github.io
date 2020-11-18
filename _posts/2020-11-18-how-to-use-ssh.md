---
date: 2020-11-18
layout: post
title: SSH(Secure Shell)란?
subtitle: How to use SSH?
description:
image: /assets/img/SSH.jpg
optimized_image:
  
category: network
tags:
  - SSH
  - Secure Shell
  - network
  
author: Roytravel
paginate: true
---

이전 포스팅 : <a href="https://roytravel.github.io/yolo-using-python-and-opencv/">yolo-using-python-and-opencv</a><br>


### About SSH
AWS나 GCP와 같은 클라우드 플랫폼을 사용하다보면 주로 SSH를 통해 접속하고 작업을 진행하게 된다. SSH 사용에 대한 이해는 암호학에서 나오는 **공개키와 비밀키의 관계를 이해**하는 것이 핵심이다. 본 포스팅에서는 Windows 10과 Windows Server 2016간의 예시를 통해 이러한 SSH의 공개키와 비밀키가 어떻게 사용되는지를 설명하고자한다.


### SSH 설치
Windows Server 2016의 경우 Windows 10과 달리 기본적으로 내장되어 있는 ssh가 없어서 별도의 OpenSSH와 같은 어플리케이션을 설치해주어야 한다. Winodws 10의 경우에도 빌드 버전에 따라 내장된 ssh가 없을 수 있으나 만약 없을 경우 설정 > 앱 > 선택적 기능 > 기능 추가 > OpenSSH 서버를 선택하여 설치하면 된다. OpenSSH에 대한 설치 과정과 환경 설정은 본 포스팅에서는 언급하지 않는다.

![alt text](/assets/img/add_feature_openssh.png)

### 키 생성

**Windows 10 : Client**<br>
**Windows Server 2016 : Server**<br>

만약 Local인 Windows 10에서 Remote인 Windows Server 2016에 SSH를 접속하고 싶을 경우 접속을 위해 키 페어를 생성해야한다.

키 페어를 생성하여 SSH 통신을 하기 위해서는 실제로 작업을 진행하는 Local에서 공개키/비밀키 쌍을 생성하여 공개키를 Remote에 두어야 한다.

키 페어를 생성하기 위해 아래와 같이 **ssh-keygen -t rsa** 명령을 사용하면 어떠한 이름의 키 페어를 생성할 것인지를 확인하며 입력 없이 Enter를 누르게되면 default 이름인 id_rsa로 설정된다. (-t 옵션은 type을 의미하고 추가적으로 -C 옵션은 comment, -b 옵션은 bit 수를 지정이 가능. default bit수는 2048임)
![alt text](/assets/img/ssh_keygen.png)

이후 비밀키를 특정 passphrase로 암호화 할 것인지에 대해 확인하며 입력 없이 Enter를 누르게 되면 키 페어가 생성된다.
![alt text](/assets/img/enter_passphrase.png)

생성된 키 페어는 사용자 폴더 하위의 .ssh 폴더에 생성되며 아래와 같이 비밀키인 id_rsa와 공개키인 id_rsa.pub이 생성된다.
![alt text](/assets/img/key_pair.png)

생성된 키 페어 중 공개키인 id_rsa.pub을 remote로 작업할 Windows Server 2016의 .ssh 폴더 하위에 아래와 같이 authorized_keys에 추가해주어야 한다. 

![alt text](/assets/img/authorized_keys.png)
authorized_keys가 없을 경우 파일을 생성하고 id_rsa.pub의 데이터를 동일하게 복붙해주면 된다.

### SSH 사용
SSH를 사용하기 위해서는 3가지 파일이 필요하다.<br>
Local : id_rsa, config<br>
Remote : authorized_keys

이 중 id_rsa와 authorized_keys의 경우 키 페어 생성을 통해 얻을 수 있는 파일이며, 마지막으로 config 파일이 필요한데 별도로 존재하지 않을 경우 아래와 같이 작성해주어야 한다.
![alt text](/assets/img/ssh_config_file.png)

Host 명의 경우 추후 ssh 명령을 사용할 때 축약하여 사용할 수 있는 것으로 임의 설정이 가능하다. HostName의 경우 접속하고자 하는 remote의 IP이며, User의 경우 remote 환경의 사용자 계정 이름을 의미한다. 여기서는 Windows Server 2016의 사용자 계정인 Administrator라 볼 수 있다.

이후 중요한 것은 config 파일의 사용 권한 설정이다. 일반적으로는 아래와 같이 3개의 보안 주체가 있는 것을 확인할 수 있을 것이다. 
![alt text](/assets/img/advanced_security_setting.png)


여기에서 모든 보안 주체에 대해 상속을 사용하지 않음을 설정해주고 아래와 같이 모두 제거를 진행하여야 한다.
![alt text](/assets/img/advanced_security_setting_2.png)

이후 아래와 같이 **ssh [User]@[IP]**와 같은 명령어를 작성할 경우 비밀번호 입력창이 뜨는 것을 확인할 수 있다.
![alt text](/assets/img/ssh_access.png)

이후 remote로 접속하는 비밀번호를 입력해주면 아래와 같이 접속이 완료된 것을 확인할 수 있다.
![alt text](/assets/img/ssh_connection.png)

만약 비밀번호 없이 바로 접속하고 싶다면 아래와 같이 -i 옵션을 통해 비밀키인 id_rsa를 입력해주면 비밀번호 없이 remote로 접속이 가능하다.

![alt text](/assets/img/connection_without_password.png)

만약 remote에 있는 파일을 실행하고 싶을 경우 아래와 같은 명령을 통해 파이썬 파일을 실행할 수 있다.
![alt text](/assets/img/ssh_command_execution.png)

또한 scp 명령을 통해 remote와 local간의 데이터를 주고 받을 수 있는데 remote로부터 local로 데이터를 가져오는 명령은 다음과 같다.
![alt text](/assets/img/usage_scp.png)

아래와 같이 파이썬 스크립트를 통해서도 SSH 통신으로 명령을 내리고 결과를 받아오는 것이 가능하다.
![alt text](/assets/img/ssh_python_script.png)

사용한 파이썬 코드는 다음과 같다.
```python
# -*- coding:utf-8 -*-
import paramiko as paramiko
from paramiko import AutoAddPolicy

server=""
username="Administrator"
password = ""
cmd_to_execute = "cd C:\\Users\Administrator\\Desktop\\test\\ & python test.py & dir"

ssh = paramiko.SSHClient()
ssh.load_system_host_keys()
ssh.set_missing_host_key_policy(AutoAddPolicy())

ssh.connect(server, username=username, password=password, sock=None, port=22)
ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd_to_execute)

err = ''.join(ssh_stderr.readlines())
out = ''.join(ssh_stdout.readlines())
final_output = str(out)+str(err)
print(final_output)

```

### Reference
[1] <a href="https://www.makeuseof.com/tag/4-easy-ways-to-use-ssh-in-windows/">How to Use SSH in Windows: 5 Easy Ways</a><br>
