---
date: 2020-11-22
layout: post
title: 소켓 프로그래밍<br>(Socket Programming)
subtitle: 파이썬을 활용한 소켓 프로그래밍 구현
description:
image: /assets/img/network_cable.jpg
optimized_image:
  
category: network
tags:
  - TCP
  - Socket
  - Socket Programming
  
author: Roytravel
paginate: true
---

이전 포스팅 : <a href="https://roytravel.github.io/how-to-use-ssh/">how-to-use-ssh</a><br>


## About Socket
소켓이란 네트워크 프로그래밍을 하기 위해 사용하는 것으로 소켓을 통해 유저 또는 어플리케이션 간의 데이터를 주고받을 수 있게 된다. 소켓은 사전적으로는 연결, 콘센트 등의 의미를 가지는 것으로 네트워크 소켓은 네트워크 환경에 연결할 수 있게 만들어진 콘센트라할 수 있다. 

데이터를 주고 받기 위해서는 먼저 클라이언트가 서버로 연결하여야 하는데 이 때 네트워크 소켓을 사용하며, 일종의 인터페이스나 연결장치와 같다고 볼 수 있다. 네트워크 소켓을 이용하면 보내고자 하는 측의 컴퓨터에서 TCP/IP 소프트웨어를 처리하여 랜카드로 보내고 받는 측은 반대로 랜카드를 통해 받아들인 뒤 TCP/IP 소프트웨어를 거쳐 데이터가 전송되게 된다.


## Socket 통신 절차
네트워크 소켓을 이용한 통신 주체는 크게 클라이언트와 서버로 나뉘며, 소켓은 일반적으로 아래와 같은 절차를 가진다.
![alt text](/assets/img/socket_flow.png)
서버측에서는 socket 함수를 사용하여 소켓을 생성한 뒤, bind 함수를 사용하여 IP와 PORT 정보를 소켓에 할당한다. 이후 listen 함수를 사용하여 클라이언트 측으로부터 연결 요청을 받을 준비를 마쳤다는 사실을 알리고, 요청이 오기를 기다린다. 클라이언트로부터 연결 요청이 올 경우 accept 함수를 통해 요청 받으며 send와 recv 함수를 통해 데이터를 주고 받고 종료하게 된다.


## 비유로 이해하는 Socket
1. 휴대폰 구매 (소켓을 생성한다 --> **socket**)

2. 휴대폰에 전화번호 할당 (소켓에 IP와 PORT 번호 할당 --> **bind**)

3. 휴대폰을 키고 전화를 기다림 (소켓에서 데이터 수신 대기 --> **listen**)

4. 전화가 오면 전화를 받음 (소켓에서 데이터 수신 허용 --> **accept**)


## 소켓 인터페이스 함수
기본적으로 사용되는 소켓 인터페이스 함수의 목록은 다음과 같다.

- socket(): 소켓 생성
- bind(): 소켓에 IP, PORT를 할당
- listen(): 클라이언트의 접속 요청 대기
- connect(): 클라이언트가 서버에 접속 요청
- accept(): 클라이언트의 접속 허용
- recv(): 데이터 수신(SOCK_STREAM)
- send(): 데이터 송신(SOCK_STREAM)
- recvfrom(): 데이터 수신(SOCK_DGRAM)
- sendto(): 데이터 송신(SOCK_DGRAM)
- close(): 소켓 종료

위의 함수 중 bind(), listen(), accept()의 경우 서버측에서만 사용되며, 반면 connect 함수의 경우 클라이언트측에서만 사용된다. 이외의 socket(), recv(), send(), recvfrom(), sendto(), close() 함수는 서버와 클라이언트 모두 사용한다.


## 소켓의 종류
* 유닉스 도메인 소켓 : **AF_UNIX**
* 인터넷 소켓 : **AF_INET**

소켓의 종류는 크게 두 가지로 분류된다. 같은 호스트 내의 프로세스 간의 데이터를 주고 받기 위해서 사용하는 유닉스 도메인 소켓인 **AF_UNIX**와, 인터넷을 통해 다른 호스트와 정보를 주고 받기 위해 사용하는 인터넷 소켓인 **AF_INET**. 소켓을 표시하는 이름은 <sys/socket.h>에 정의되어 있는 주소 패밀리명을 사용한다.


## Socket 프로그래밍 코드 예제
### Server - main.py
```python
import tcpServer

andRaspTCP = tcpServer.TCPServer("127.0.0.1", 50000)
andRaspTCP.start()
```

### Server - tcpServerThread.py
```python
# -*- coding:utf-8 -*-
import socket, threading
import tcpServerThread


class TCPServer(threading.Thread):
    def __init__(self, HOST, PORT):
        threading.Thread.__init__(self)

        self.HOST = HOST
        self.PORT = PORT

        # 소켓 객체 생성
        self.serverSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        # HOST 주소와 PORT를 사용하는 인터페이스와 연결
        self.serverSocket.bind((self.HOST, self.PORT))

        # 사용자의 접속을 기다리는 단계로 넘어감
        self.serverSocket.listen(1)

        # 연결된 목록과 쓰레드를 관리하기 위한 리스트 생성
        self.connections = []
        self.tcpServerThreads = []

    def run(self):
        try:
            while True:
                print ('[+] 연결 대기...')

                #클라이언트의 접속 수락
                #누군가가 접속하여 연결되었을 때 비로소 결과 값이 return 되는 함수
                #누군가 접속하기 전까지 이 부분에서 멈춰있음. connection을 통해 앞으로 통신을 진행
                connection, clientAddress = self.serverSocket.accept()

                self.connections.append(connection)
                print ('[+] 연결 완료 : {}\n'.format(clientAddress))

                subThread = tcpServerThread.TCPServerThread(self.tcpServerThreads, self.connections, connection, clientAddress)
                subThread.start()
                self.tcpServerThreads.append(subThread)
        except:
            print("[!] ServerThread error")

    def sendAll(self, message):
        try:
            self.tcpServerThreads[0].send(message)
        except:
            pass
```

### Server - tcpServer.py
```python
# -*- coding:utf-8 -*-
import socket, threading
import os
import io


class TCPServerThread(threading.Thread):
    def __init__(self, tcpServerThreads, connections, connection, clientAddress):
        threading.Thread.__init__(self)

        self.tcpServerThreads = tcpServerThreads
        self.connections = connections
        self.connection = connection
        self.clientAddress = clientAddress


    def run(self):
        try:
            # socket 객체 생성
            sock = socket.socket()           

            # recv는 socket에 메시지가 수신될 때 까지 대기(accept와 동일) 만약 1024보다 수신할 데이터가 크다면 가져오지 못했던 것을 가져옴
            # 파일의 meta-data 수신
            filename = self.connection.recv(1024) # 파일 이름 수신
            self.connection.send(u"[+] server : filename query ok".encode()) # 파일 이름 수신 완료 메시지 전송

            filesize = self.connection.recv(1024) #파일 사이즈 수신
            self.connection.send(u"[+] server : filesize query ok".encode()) #파일 사이즈 수신 완료 메시지 전송

            print ('[+] 파일 이름 : {}'.format(filename.decode())) # 받을 파일 이름과 사이즈 출력
            print ('[+] 파일 크기 : {} bytes'.format(filesize.decode())) # 받을 파일 이름과 사이즈 출력


            # 파일의 data를 수신
            data = self.connection.recv(1024)
            if not data:
                print("[!] 파일 수신 중 오류")
                return


            # 파일 data를 저장
            with open(filename.decode(), 'wb') as f:
                data_transferred = 0
                try:
                    while data:
                        f.write(bytes(data))
                        data_transferred = data_transferred + len(data)
                        data = self.connection.recv(1024)
                    print ("[+] 수신 파일 저장 완료")

                    # 파일 수신 및 저장 완료 메시지 전송
                    self.connection.send("[+] server : finished transferring file".encode())
                except Exception as e:
                    print (e)
                    pass

                print ("[+] 수신 파일 크기 : {} byres\n".format(data_transferred))

        except Exception as e:
            print (e)
            self.connections.remove(self.connection)
            self.tcpServerThreads.remove(self)
            exit(0)
        self.connections.remove(self.connection)
        self.tcpServerThreads.remove(self)


    def send(self, message):
        print('tcp server :: ', message)
        try:
            for i in range(len(self.connections)):
                self.connections[i].sendall(message.encode())
        except:
            pass

```

### Client - Client.py
```python
#-*- coding:utf8 -*-
import socket
import argparse
import socketserver
import os
import datetime


# 파일 데이터와 크기를 반환
def get_file_data(fullpath):
	with open(fullpath, mode='rb') as f:
		data = f.read()
		size = os.path.getsize(fullpath)
	return data, size


# 메타 데이터 전송 및 수신 결과 반환
def send_metadata(sock, data):
	sock.send(data.encode('utf-8'))
	result = sock.recv(1024)
	return result
	

if __name__ == '__main__':
	# 옵션을 사용하여 보내고자 하는 서버에 지정한 파일을 전송하고자 함
	parser = argparse.ArgumentParser(description='Clinet.py -H [HOST IP] -P [HOST PORT] -F [FILE PATH]')
	parser.add_argument('-H', dest='host', help='HOST IP', required=True)
	parser.add_argument('-P', dest='port', help='HOST PORT', required=True)
	parser.add_argument('-F', dest='file', help='FILE PATH', required=True)
	args = parser.parse_args()


	# 옵션으로 받은 정보 저장
	HOST = args.host
	PORT = args.port
	PATH = args.file
	ADDR = (HOST, int(PORT))

	# 소켓 객체 생성 및 연결
	sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM) 
	sock.connect(ADDR) 
	print ('[+] Connected : {} '.format(ADDR))

	
	# 파일 데이터와 크기 확보
	data, size = get_file_data(PATH)

	# 1) 파일 이름과 크기 정보 송신 및 결과 출력
	information = [PATH, str(size)]
	for idx in information:
		result = send_metadata(sock, idx)
		print (result)


	# 2) 실제 파일 데이터 송신
	with open(PATH, 'rb') as f:
		data_transferred = 0
		try:
			data = f.read()
			while data:
				data_transferred = data_transferred + sock.send(data)
				data = f.read()
		except Exception as e:
			print (e)
			pass
	sock.close()
```

### Socket 통신 결과
![alt text](/assets/img/socket_result.png)
클라이언트 측에서는 옵션 기능을 넣어 -H 옵션을 통해 연결하고자하는 Host IP, -P 옵션을 통해 PORT를 입력하고 마지막으로 -F 옵션을 통해 전송하고자 하는 파일의 경로를 입력해줄 수 있게 하였다.
서버측의 경우 일회성으로 종료하는 것이 아닌, 계속해서 소켓 통신이 가능하도록 하였다.

### Reference
[1] <a href="https://recipes4dev.tistory.com/153">소켓 프로그래밍. (Socket Programming)</a><br>
[2] <a href="https://simsimjae.tistory.com/48">소켓(socket programming)프로그래밍이란</a><br>
[3] <a href="https://12bme.tistory.com/228">[네트워크] 소켓프로그래밍 기초</a><br>