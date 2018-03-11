# <img src="https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/pet_Image.jpg?raw=true" width="64">Pet House System
[![License: GPL v3](https://img.shields.io/badge/licence-GPL%20v3-yellow.svg)](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/LICENSE)
<img src="https://img.shields.io/badge/python-%3E%3D3-brightgreen.svg">
<img src="https://img.shields.io/badge/release-v1.0.2-blue.svg">
### Pet House System is a tool that enables you to manage pets through Messenger.



## Index
* [Introduction](#introduction)
* [Features](#features)
* [Requirement](#requirement)
* [Settings & Installation](#settings--installation)
  * [Settings](#settings)
  * [Installation](#installation)
* [Pet House Structure](#pet-house-structure)
* [Motor operation structure](#motor-operation-structure)
* [Client & Server Structure](#client--server-structure)
  * [Full server structure](#full-server-structure)
  * [Client & Main server structure](#client--main-server-structure)
  * [Main server & Pi-server structure](#main-server--pi-server-structure)
* [How to use](#how-to-use)
* [How to connect motor wires](#how-to-connect-motor-wires)
  * [Food Motor](#food-motor)
  * [Water Motor](#water-motor)
  * [Door Motor](#door-motor)
* [Notes](#notes)
* [LICENSE](#license)

## Introduction

Pet House systems were built for people who would be absent from home and wouldn't be able to take care of their pets.
so it can help reduce pet worries because it is easy to manage pets when you are away on long trips or on sudden appointments.
It's very simple to use because it uses a messenger. If you are in an environment that has Internet access, you can use it anywhere.


## **Features**
 - Usage through Messenger
 - Raspberry Pi with chat-bot based System
 - Using communication with flask server
 - Using 2 flask servers.(Main Server(in aws), PiServer(in raspberryPi))
 - You can check the status of your pet with PiCamera
 - You can manage meal and to give water to your pet(s).


## **Requirement**

 - Raspnerry Pi 3 module B (used in Pi Server)
 - 3 servo-motors(for meal,water,door) and PiCamera
 - Computer or Notebook(used to chat-bot API Server)
 - Smart Phone for using chat-bot(used in client)

## **Settings & Installation**

### **Settings** 

 - Using 2 static ip address.
 - Main server is in AWS.(This server manage chat-bot API.)
 - PiServer is in RaspberryPi (This server manage RaspberryPi)
 - Using python 3.x version. Because Hangul generate error with uni-code/utf8.
 
### **Installation**
 
 **1) Server side**
  - Install MySQL.
  ```
  sudo apt-get update
  sudo apt-get install mysql-server
  ```
  
  - Install python3 modules; requests, flask, pymysql 
  ```
  sudo pip3 install requests
  sudo pip3 install flask
  sudo pip3 install pymsql
  ```
   
 **2) PiServer side**
  - Install GPIO modules.
  ```
  sudo apt-get install python-dev
  sudo apt-get install python-rpi.gpio
  ```
   
  - Install flask, requests modules.
  ```
  sudo pip3 install flask
  sudo pip3 install requests
  ```

## **Pet House Structure**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/Pet_House_Structure.png?raw=true)

Three motors are operated by messenger, and manage feeds, water and door.<br>
And you can see the pet directly through the Pi Camera.


## **Motor operation structure** 

| Food | Water |
| :----: | :----: |
| ![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_food.png?raw=true) <br> Rotate and restore the motor for a short time, and feed the prey by opening and closing the entrance  | ![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_water.png?raw=true) <br> Water is given by folding or unfolding the tube with the motor |
**Door(open)** | **Door(close)**
|![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_open.png?raw=true) <br> Open the door by pulling the thread by the rotation of the motor | ![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_close.png?raw=true) <br> When the motor stops, the door is closed by the resilience of the weight. |



## **Client & Server Structure**

### **Full server structure**

![](https://github.com/kuj0210/opensourceproject/blob/master/.README/Client&Server_Structure.png?raw=true)


- Client: The client represents a user using messanger application.
- Server(Main Server): This server is main of full structure. It manage chatbot and commands for controlling PiServer.
- PiServer(RaspberryPi): This server manage to control motors, camera and push thread.


### **Client & Main server structure**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/Structure_client&mainserver.png?raw=true)


 Client will order various commands. (regist user, control to pet-home etc) And the main-server get this commands. Before main-server get this commands, messages go to API server and API server give the data to main-server.(data:json type) After main-server get this type's data, it'll parse this data and make operation list for ordering to PiServer. The main-server send operation list to PiServer and make reply-message for sending to user.<br>
 But if a user don't register to server or don't registered in PiServer's userlist, this user can't use this chatbot. Main-server use database for managing user-data and registed Pi-servers. Below inform main-server and pi-server structure.


### **Main server & Pi-server structure**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/Structure_mainserver&piserver.png?raw=true)


This structure is main-server and pi-server(in RaspberryPi using flask framework) structure. Before the Pi-server open flask server, this server send user and this device's information(registed userlist and PiKey) to main server. If this communication come into existence(communication success), the pi-server is ready to get data from main-server. The main server send operation list to pi-server by user's order. Pi-Server parse these, order to each of motor or pi-camera for implement of user's commands. And then, after implement of user's commands, pi-server send result-data to main-server. The main-server parse this data, make the appropriate reply and finally send json type data to API server. (This json data will become reply message; it is shown reply message to user.)


## **How to use**


**1) Add Official account of Messanger(IoT Pet House System)**

 - kakao-talk platform: @guineapighome (IoT 펫홈 관리 시스템)<br> 
 - naver-talktalk platform: IoT 펫홈 시스템

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_first.PNG?raw=true)


**2) Enter chat in the format “[등록]/e-mail/Product-Key”**

 Check your device's product-key, and regist your email and product-key to chatbot-server.<br>
 If you don't regist, chatbot-server don't support your command. 
 
![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_regist.PNG?raw=true)


**3) Enter chats that associated with food, water and door opening.**

If you're a registed user, you can do chatting with IoT-pet-home-system!<br>
Order to set feed, water or open pet-home door at the IoT-pet-home-system chatbot.

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_operation.PNG?raw=true)


**4) If you don't know how to use or need to remind your account, please enter the command below.**

- "[사용법]" : This command will inform how to use this chatbot.<br>
- "[정보]" : This command will inform your account that you registed at this chatbot-server.

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_etc.PNG?raw=true)


**5) If you forget to feed or set water to your pet, chatbot's push service support you!**

- If you don't set feed or water to your pet, push alarm inform to you once an hour.
- But this service only support at naver-talk-talk platform. (Kakao-talk platform don't support it.)

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_push.PNG?raw=true)
     



## **How to connect motor wires**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/raspberry-pi-pinout.png?raw=true)


### Food Motor

- Orange wired: connects to pin 35
- Red wired: connects to 3v3 or 5v pin(one of pin 1, 2, 4, 17)
- Brown wired: connects to GND pin(one of pin 6, 9, 14, 20, 25, 30, 34, 39)

### Water Motor

- Orange wired: connects to pin 32
- Red wired: connects to 3v3 or 5v pin(one of pin 1, 2, 4, 17)
- Brown wired: connects to GND pin(one of pin 6, 9, 14, 20, 25, 30, 34, 39)

### Door Motor

- Orange wired: connects to pin 12
- Red wired: connects to 3v3 or 5v pin(one of pin 1, 2, 4, 17)
- Brown wired: connects to GND pin(one of pin 6, 9, 14, 20, 25, 30, 34, 39)



 ## **Notes**
 
 - In issue#36 :: Reference PPT and video(참고용 PPT 및 동영상)<br>
   https://github.com/kuj0210/IoT-Pet-Home-System/issues/36
   
 - Installation method of Rasbian<br>
   https://www.raspberrypi.org/documentation/installation/installing-images/



 ## **LICENSE**
 
 IoT-Pet-Home-System is licensed under [the GNU GENERAL PUBLIC LICENSE v3](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/LICENSE).
 
 ```
 Copyright (C) 2017-present, kuj0210, KeonHeeLee, seok8418

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```

<br>
<br>

***

***

<br>

# <img src="https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/pet_Image.jpg?raw=true" width="64">Pet House System
[![License: GPL v3](https://img.shields.io/badge/licence-GPL%20v3-yellow.svg)](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/LICENSE)
<img src="https://img.shields.io/badge/python-%3E%3D3-brightgreen.svg">
<img src="https://img.shields.io/badge/release-v1.0.2-blue.svg">
### PetHouseSystem은 Messenger를 통해 애완동물을 관리할 수 있게 해 주는 도구입니다.


## Index
* [Introduction](#introduction-1)
* [Features](#features-1)
* [Requirement](#requirement-1)
* [Settings & Installation](#settings--installation-1)
  * [Settings](#settings-1)
  * [Installation](#installation-1)
* [Pet House Structure](#pet-house-structure-1)
* [Motor operation structure](#motor-operation-structure-1)
* [Client & Server Structure](#client--server-structure-1)
  * [Full server structure](#full-server-structure-1)
  * [Client & Main server structure](#client--main-server-structure-1)
  * [Main server & Pi-server structure](#main-server--pi-server-structure-1)
* [How to use](#how-to-use-1)
* [How to connect motor wires](#how-to-connect-motor-wires-1)
  * [Food Motor](#food-motor-1)
  * [Water Motor](#water-motor-1)
  * [Door Motor](#door-motor-1)
* [Notes](#notes-1)
* [LICENSE](#license-1)

## Introduction

Pet House systems 은 집을 비워서 그들의 애완동물을 돌봐줄 수 없는 사람들을 위해 만들어졌습니다.
Pet House System은 긴 여행을 갔을 때나 갑작스러운 약속이 생겼을 때 애완동물을 쉽게 돌볼 수 있게 해주기 때문에 애완동물에 대한 걱정을 줄여줍니다.
메신저를 이용했기 때문에 사용하기도 아주 쉽습니다. 인터넷이 연결되어있는 환경이라면 어디에서도 사용할 수 있습니다.


## **Features**
 - 메신저를 통한 사용 
 - 챗봇 시스템을 기반한 라즈베리파이
 - flask server를 통한 정보전달
 - 2개의 flask server 사용(Main Server(in aws), PiServer(in raspberryPi))
 - PiCamera를 통해서 애완동물의 상태를 확인할 수 있음
 - 애완동물에게 먹이와 물을 줄 수 있습니다.


## **Requirement**

 - Raspnerry Pi 3 module B (Pi Server에 사용)
 - 3 개의 서보모터(먹이, 물, 문)와 PiCamera
 - 컴퓨터 또는 노트북(챗봇 API Server에 사용)
 - 챗봇을 사용할 스마트폰

## **Settings & Installation**

### **Settings** 

 - 2개의 고정IP주소 사용
 - AWS에 Main server(챗봇 API관리)준비. 
 - 라즈베리파이에 PiServer(라즈베리파이 관리)준비
 - 한글이 uni-code/utf8 오류를 발생시키기 때문에 python 3.x 버전을 사용합니다.
 
### **Installation**
 
 **1) Server side**
  - Install MySQL.
  ```
  sudo apt-get update
  sudo apt-get install mysql-server
  ```
  
  - Install python3 modules; requests, flask, pymysql 
  ```
  sudo pip3 install requests
  sudo pip3 install flask
  sudo pip3 install pymsql
  ```
   
 **2) PiServer side**
  - Install GPIO modules.
  ```
  sudo apt-get install python-dev
  sudo apt-get install python-rpi.gpio
  ```
   
  - Install flask, requests modules.
  ```
  sudo pip3 install flask
  sudo pip3 install requests
  ```

## **Pet House Structure**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/Pet_House_Structure.png?raw=true)

3개의 모터가 메신저에 의해서 동작하고, 먹이와 물, 문을 관리합니다.<br>
그리고 PiCamera를 통해서 애완동물을 직접 볼 수 있습니다.


## **Motor operation structure** 

| 먹이 | 물 |
| :----: | :----: |
| ![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_food.png?raw=true) <br> 모터를 짧은 시간동안 돌리고 복구합니다. 입구가 열리고 닫히면서 먹이를 줍니다.  | ![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_water.png?raw=true) <br> 튜브가 모터에 의해 접히고 펼쳐지면서 물을 공급합니다. |
**문(열기)** | **문(닫기)**
|![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_open.png?raw=true) <br> 모터의 회전이 실을 잡아당기면서 문이 열립니다. | ![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/motor_close.png?raw=true) <br> 모터가 정지되면 모게추의 복원력에 의해 문이 닫힙니다. |



## **Client & Server Structure**

### **Full server structure**

![](https://github.com/kuj0210/opensourceproject/blob/master/.README/Client&Server_Structure.png?raw=true)


- Client: 메신저 어플을 사용하는 유저를 나타냅니다..
- Server(Main Server): 전체 구조의 중심이 되는 서버입니다. 챗봇과 PiSever 제어를 위한 명령어를 관리합니다.
- PiServer(RaspberryPi): 모터제어와 카메라, 푸시알람을 관리합니다.


### **Client & Main server structure**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/Structure_client&mainserver.png?raw=true)


클라이언트가 다양한 명령어를 명령합니다. (사용자 등록, pet-home 제어 등등) 그리고 main-server는 이 명령을 받습니다. main-server가 이 명령을 받기 전에 메시지들은 API 서버로 가고, API 서버는 데이터(json형식)를 메인서버로 보냅니다. main-server가 json형식의 데이터를 받게 되면 main-server는 이 데이터를 분석하고, PiServer에 명령하기 위한 명령리스트를 만듭니다. main-server 는 PiServer에 명령 리스트를 보내고 user에게 보낼 답장을 만듭니다.<br>
 하지만 사용자가 서버에 등록하지 않거나 PiServer의 사용자 리스트에 등록하지 않은 경우에는 챗봇을 사용할 수 없습니다. Main-server는 사용자 데이터와 등록된 Pi-servers를 관리하기 위해서 데이터베이스를 사용합니다. 아래는 main-server 및 Pi-server 구조를 나타냅니다.


### **Main server & Pi-server structure**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/Structure_mainserver&piserver.png?raw=true)

 main-server  및 pi-server(flask framework를 사용하는 RaspberryPi 내부)의 구조입니다. Pi-server는 flask server를 열기 전에 사용자와 이 장치의 정보(등록된 사용자 목록 및 PiKey)를 main server로 보냅니다. 이 통신이 존재하게 되면(통신 성공), pi-server는 main-server로부터 데이터를 얻을 준비가 된 것입니다. main server는 사용자의 명령에 의해 pi-server로 명령 리스트를 보냅니다. Pi-Server가 이것들을 분석하고, 사용자 명령의 실행을 위해 각 모터나 pi-camera에 동작을 지시합니다. 사용자 명령을 실행한 후 pi-server는 결과 데이터를 main server로 보냅니다. main server는 이 데이터를 분석하고 적절한 응답을 생성한 후, json유형 데이터로 API서버에 전송합니다. (이 json데이터가 응답 메시지가 되고, 사용자에게 응답 메시지가 표시됩니다.)

## **How to use**


**1) 메신저의 공식 계정을 추가(IoT Pet House System)**

 - kakao-talk platform: @guineapighome (IoT 펫홈 관리 시스템)<br> 
 - naver-talktalk platform: IoT 펫홈 시스템

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_first.PNG?raw=true)


**2) “[등록]/e-mail/Product-Key” 형식으로 채팅 입력**

기기의 제품 키를 확인하고 이메일과 제품 키를 챗봇 서버에 등록합니다.<br>
등록하지 않으면, 챗봇 서버가 명령을 지원하지 않습니다.
 
![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_regist.PNG?raw=true)


**3) 먹이, 물, 문 개폐와 관련된 채팅 입력.**

등록된 사용자라면 IoT-pet-home-system으로 채팅을 할 수 있습니다!<br>
IoT-pet-home-system 챗봇으로 먹이와 물을 주거나 문을 열도록 해보세요.

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_operation.PNG?raw=true)


**4) 사용 방법을 모르거나 계정확인이 필요한 경우 아래 명령을 입력하십시오.**

- "[사용법]" : 챗봇을 사용하는 방법을 알려주는 명령어입니다.<br>
- "[정보]" : 챗봇 서버에 등록된 계정을 알려주는 명령어입니다.

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_etc.PNG?raw=true)


**5) 만약 애완동물에게 먹이를 주거나 물을 주는 것을 잊었다면, 챗봇의 푸시알람 서비스가 도와줄 겁니다!**

- 애완 동물에게 먹이나 물을 주지 않으면, 한시간에 한번씩 푸시 알람이 알려줍니다.
- 이 서비스는 ‘네이버 톡톡‘에서만 지원됩니다. (’카카오톡‘은 아직 지원하지 않습니다.)

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/chatbot_push.PNG?raw=true)
     



## **How to connect motor wires**

![](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/.README/raspberry-pi-pinout.png?raw=true)


### 먹이 모터

- 주황색 선: 35번 핀에 연결
- 빨간색 선: 3v3 또는 5v 핀에 연결(1, 2, 4, 17 번 핀 중 하나)
- 갈색 선: GND 핀에 연결(6, 9, 14, 20, 25, 30, 34, 39 번 핀 중 하나)

### 물 모터

- 주황색 선: 32번 핀에 연결
- 빨간색 선: 3v3 또는 5v 핀에 연결(1, 2, 4, 17 번 핀 중 하나)
- 갈색 선: GND 핀에 연결(6, 9, 14, 20, 25, 30, 34, 39 번 핀 중 하나)

### 문 모터

- 주황색 선: 12번 핀에 연결
- 빨간색 선: 3v3 또는 5v 핀에 연결(1, 2, 4, 17 번 핀 중 하나)
- 갈색 선: GND 핀에 연결(6, 9, 14, 20, 25, 30, 34, 39 번 핀 중 하나)



 ## **Notes**
 
 - In issue#36 :: Reference PPT and video(참고용 PPT 및 동영상)<br>
   https://github.com/kuj0210/IoT-Pet-Home-System/issues/36
   
 - Rasbian 설치 방법<br>
   https://www.raspberrypi.org/documentation/installation/installing-images/



 ## **LICENSE**
 
 IoT-Pet-Home-System is licensed under [the GNU GENERAL PUBLIC LICENSE v3](https://github.com/kuj0210/IoT-Pet-Home-System/blob/master/LICENSE).
 
 ```
 Copyright (C) 2017-present, kuj0210, KeonHeeLee, seok8418

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```
