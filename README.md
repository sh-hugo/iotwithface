Abstract
IoT  (Internet  of  Things)  is  the  network  of  physical  objects devices, vehicles,   buildings   and   other   items   embedded   with   electronics,   software, sensors,  and  network  connectivity that  enables  these  objects  to  collect  and exchange data. The internet of things allows objects to be sensed and controlled remotely  across  existing  network  infrastructure.  According  to  the  Gartner, 260  million  objects  will  be  connected  by  year  2020.  Several  companies  and governments  have  tried  to  make  references  with  IoT  in  initial  times,  but nowadays   in   manufacturing,   retail   and   SOC   (Social   Overhead   Capital) industries,   successful   best   practices   are   built   recently.   In   this   paper   I summarized  tangible  IoT  based  service  models  which  are  helpful  to  academic and industrial world to understand IoT business. 

Introduction
In This paper I discussed on prototype on IoT to use sensor data to upload to server for computing process. I have used ip camera as sensor (old android mobile), Raspberry Pi 3 model B, Ubuntu Server 16.04, and Android mobile where all computed data is stored which is send by Ubuntu Server.
In this system I use shell script from raspberry pi to take image from ip-camera and send to Ubuntu server for computing and after that it is send back to raspberry pi. After that raspberry pi again send that computed data to my android mobile device


Method
Configuring Ubuntu Server
Requirements
Python 3.3+ or Python 2.7
macOS or Linux
make sure you have dlib already installed with Python bindings:
Make sure you are on root user
And Installed virtualenv
Open terminal and type this 
 
$virtualenv -p python3 py3env  
Enabling it by Typing
 $source /root/py3env/bin/activate 
 
Downloading dlib source from Github
Dlib is a modern C++ toolkit containing machine learning algorithms and tools for creating complex software in C++ to solve real world problems.
 
$git clone https://github.com/davisking/dlib.git
 
Build The main dlib
 
$cd dlib
$mkdir build
$cd build
$cmake .. -DDLIB_USE_CUDA=0 -DUSE_AVX_INSTRUCTIONS=1
$cmake --build .
 
 
 
 
 
Build and install the python extensions
 
$cd ..
$python3 setup.py install --yes USE_AVX_INSTRUCTIONS --no DLIB_USE_CUDA
Then, install this module from pypi using pip3 
$pip3 install face_recognition   
Configuring Raspberry pi
Intro to Raspberry pi
The Raspberry Pi is a low cost, credit-card sized computer that plugs into a computer monitor or TV, and uses a standard keyboard and mouse. It is a capable little device that enables people of all ages to explore computing, and to learn how to program in languages like Scratch and Python. It’s capable of doing everything you’d expect a desktop computer to do, from browsing the internet and playing high-definition video, to making spreadsheets, word-processing, and playing games.
I am using Raspberry pi to take snaps from ip-camera (android mobile)
First we need to install any operating system in raspberry pi
I prefer Linux based OS
Raspbian is our official operating system for all models of the Raspberry Pi.
I am using Raspbian OS.
Step 1: Installing vsftpd.
It is help to make ftp server on Raspberry pi.
$sudo apt update
$sudo apt-get install vsftpd
$sudo service vsftpd restart
 
 
Configuring ip-camera (Android mobile)
It has pre camera installed with wifi connectivity. 
Installing IP Webcam from Google play store.
IP Webcam turns your phone into a network camera with multiple viewing options.
Start camera Server on android >>
You have Given some ipv4 and ipv6 addresses.
Like 192.168.43.111:8080


 
I need to make my Ubuntu server login Password less from Raspberry pi
So 
First login to Raspberry pi whose IP address 192.168.43.187
$ssh pi@192.168.43.187
With user pi we generate a pair public key
  [pi@raspberry~]$ ssh-keygen -t rsa 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/pi/.ssh/id_rsa): [Press enter key]
Created directory '/home/pi/.ssh'.
Enter passphrase (empty for no passphrase): [Press enter key]
Enter same passphrase again: [Press enter key]
Your identification has been saved in /home/pi/.ssh/id_rsa.
Your public key has been saved in /home/pi/.ssh/id_rsa.pub.
The key fingerprint is:
5f:ad:40:00:8a:d1:9b:99:b3:b0:f8:08:99:c3:ed:d3 pi@raspberry.com
The key's randomart image is:
+--[ RSA 2048]----+
|        ..oooE.++|
|         o. o.o  |
|          ..   . |
|         o  . . o|
|        S .  . + |
|       . .    . o|
|      . o o    ..|
|       + +       |
|        +.       |
+-----------------+
 
Create .ssh Directory on Ubuntu server which IP address 192.168.43.1
  [pi@raspberry~]$ ssh 192.168.43.1 mkdir -p .sshThe authenticity of host '192.168.43.1 (192.168.0.11)' can't be established.RSA key fingerprint is 45:0e:28:11:d6:81:62:16:04:3f:db:38:02:la:22:4e.Are you sure you want to continue connecting (yes/no)? yesWarning: Permanently added '192.168.43.1’ (ECDSA) to the list of known hosts.192.168.0.11's password: [Enter Your Password Here] 
 
Upload generated public key to Ubuntu server ie 192.168.43.1
[pi@raspberry~]$ cat .ssh/id_rsa.pub | ssh 192.168.43.1'cat >> .ssh/authorized_keys'192.168.43.1's password: [Enter Your Password Here]  
Set permission on Ubuntu server ie 192.168.43.1
  [pi@raspberry~]$ ssh 192.168.43.1 "chmod 700 .ssh; chmod 640 .ssh/authorized_keys"192.168.43.1's password: [Enter Your Password Here] 
 
 
Execute shell script to capture image from IP cam and send it to Ubuntu server
//                                                         
///                                                       
//                                                         
//                                                         
//                                                         
                                    
                        
                                                            
                                                
                                    
 
                                    
                                                
 
 
On Ubuntu Server
I’m using Ubuntu to do computing like recognition of knows people and unknown peoples at given image and marking known one and unknown one from that.
Reference https://github.com/ageitgey/face_recognition/blob/master/examples/face_recognition_knn.py
$mkdir /root/system
$cd /root/system
$mkdir knn_examples
$cd knn_examples
$mkdir test train
$cd ..
$wget https://github.com/ageitgey/face_recognition/blob /master/examples/face_recognition_knn.py
To generate trained data
You need to make go to train directory
$cd knn_examples/train
And download your images to that directory like this
At last run below command to generate trained data
$python3 face_recognition_knn.py
trained_knn_model.clf is my trained data
From raspberry pi image it uploaded to my “test” directory. 
Here’s script to compute data and sending back to raspberry pi
//
//
//
On Raspberry pi
The above script used to compute image which uploaded by raspberry pi and after that processed image is send back to raspberry pi And raspberry pi send processed image to Client System ie. my mobile phone via “ftp server” Android app which is installed in android device.
This script used to send processed image to android device using ftp
//
///
//
 


 
Future work
I will be developing something like if person is unknown then it searches that image in google and find that person’s name from social network and gather info about that person.
This can be used for security purpose like in road traffic situation as camera of traffic crossing. Whenever someone is passing in red light then camera can send that snap to high GPU server to computer and facial recognition and then automatically it generates data about that persons and send it to authorities or even it cut challans for it and also email it to person.
This can also be used in security purpose like automatic door opener, whenever someone known try to open door camera in front of door take snap and send it to server to compute image and open door automatically.
This can also use for blind people. Camera can be attached to blind person and if blind person wants to know that the person is known or unknown if known then who’s that. It can send voice to its ear about that persons. 
