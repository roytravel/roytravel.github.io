---
date: 2020-11-22
layout: post
title: 에러 해결 모음
subtitle: 개발하며 발생한 에러를 해결하는 방법 모음
description:
image: /assets/img/troubleshooting_logo.png
optimized_image:

category: tips
tags:
  - Troubleshooting
  - trouble
  - error
  
author: Roytravel
paginate: true
---

## [Python] pip
[1] pip3 install -r requirements-gpu.txt를 진행하며 에러 발생
>ERROR: Cannot uninstall 'wrapt'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.

* 해결방법 : **pip3 install wrapt --upgrade --ignore-installed**

---


[2] pip3 install -r requirements.txt를 진행하며 에러 발생
>ERROR: Could not find a version that satisfies the requirement torch>=1.6.0 (from -r requirements.txt (line 12)) (from versions: 0.1.2, 0.1.2.post1, 0.1.2.post2)

* 해결방법 : **conda install pytorch torchvision torchaudio cudatoolkit=10.1 -c pytorch**<br>(cudatoolkit은 설치한 버전에 따라 달라질 수 있음)

---

## [Ubuntu] conda
[1] anaconda를 설치 후 conda 명령이 먹히질 않음

>conda: command not found

* 해결방법 : **source ~/.bashrc**

---

## [Raspberry pi] conda
[1] 라즈베리파이에 Miniconda를 설치하려하는데 설치가 되지 않음

* 해결방법 : **wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh**<br>
라즈베리파이는 ARM 계열이므로 해당하는 파일을 다운로드 받아야함

--- 

[2] 라즈베리파이에 Miniconda를 설치하려하는데 실행이 되지 않음
pi@raspberrypi:~/Downloads $ sh Miniconda3-latest-Linux-armv7l.sh<br>

> Miniconda3-latest-Linux-armv7l.sh: 15: Miniconda3-latest-Linux-armv7l.sh: 0: not found<br>
> Miniconda3-latest-Linux-armv7l.sh: 66: Miniconda3-latest-Linux-armv7l.sh: Syntax error: word unexpected (expecting ")")

* 해결방법 : **sudo /bin/bash Miniconda3-latest-Linux-armv7l.sh**


### Reference
[1]<a href="https://www.discoverbits.in/861/error-cannot-uninstall-wrapt-during-installation-tensorflow/">[Python] pip [1]</a><br>
[2]<a href="https://github.com/pytorch/pytorch/issues/10443">[Python] pip [2]</a><br>
[3]<a href="https://jisoo-coding.tistory.com/6">[Ubuntu] conda [1]</a><br>
[4]<a href="https://m.blog.naver.com/PostView.nhn?blogId=cjstkdgml33&logNo=221517110919&proxyReferer=https:%2F%2Fwww.google.com%2F">[Raspberry pi] conda [1]</a><br>
[5]<a href="https://m.blog.naver.com/PostView.nhn?blogId=cjstkdgml33&logNo=221517110919&proxyReferer=https:%2F%2Fwww.google.com%2F">[Raspberry pi] conda [2]</a><br>