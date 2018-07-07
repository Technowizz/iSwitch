# iSwitch
Intelligent switch that can have a track on your electricity usage and can be controlled from anywhere in the world through Android Application. 


#Components Required:
    Raspberry pi
    wifi module
    Relay 
    Switch Board
    
# In Raspberry pi 

//installing requirments


cd /home
sudo apt-get update
sudo apt-get install python-mosquitto
sudo apt-get install mosquitto
sudo apt-get install python-pip
sudo pip install paho-mqtt
sudo apt-get install build-essential python quilt devscripts python-setuptools python3
sudo apt-get install libssl-dev
sudo apt-get install cmake
sudo apt-get install libc-ares-dev
sudo apt-get install uuid-dev
sudo apt-get install daemon
sudo apt-get install libwebsockets-dev
sudo apt-get install apache2

# Installing  Mosquitto Web Socket




Compile MQTT Mosquitto for web sockets
The message broker we are using is called Mosquito and in order to be able to send messages from a web browser we need to re-compile Mosquito to include Websockets. This is done as follows:

cd /home/pi   (or whatever directory you want to work from

At the time of writing this tutorial 1.4.2 was the latest version of Mosquitto. You can check for the latest version by going to the following URL:

http://mosquitto.org/files/source/

Now download the source as follows:

wget http://mosquitto.org/files/source/mosquitto-1.4.2.tar.gz

tar zxvf mosquitto-1.4.2.tar.gz

cd mosquitto-1.4.2

sudo nano config.mk

Page down a few pages until you see WITH_WEBSOCKETS os shown in Figure 2


Figure 2 : Configure the make file to include Websockets
Change the line to say:

WITH_WEBSOCKETS:=yes

Then press ctrl-x followed by "y" and ENTER to save.
Now compile the code as follows:

sudo make

sudo make install

sudo cp mosquitto.conf /etc/mosquitto

sudo nano /etc/mosquitto/mosquitto.conf

Configure the port and Websockets listener by entering these three lines at the top of the file as shown in Figure 3.

port 1883

listener 9001

protocol websockets




Figure 3 - Configure Websocket listener
Now you are ready to run mosquitto:

./src/mosquitto -c /etc/mosquitto/mosquitto.conf -d


If all went well then you should see the following lines shown in Figure 4:



Our newly compiled Mosquitto server is now running as a background service. You can see it by typing:

ps ax
mosquittosocket.txt
Displaying requirements.txt.


# Installing Web Server



Installing web server

Earlier you stalled the Apache web server as part of all the software you loaded at the beginning of this tutorial. Now you will copy the HTML files into the web server home directory.

Download the software from our website as follows:

cd /home/pi
mkdir onoff
cd onoff
sudo wget www.privateeyepi.com/downloads/onoff.zip
sudo unzip onoff.zip -d /var/www/html
sudo mv /var/www/html/onoff.py ./

Now configure the website to point towards your Raspberry Pi IP Address. If you do not know the IP Address of your Raspberry PI type:

ifconfig

Near the bottom you will see the ip address as shown below (192.168.2.19 in my case):

wlan0     Link encap:Ethernet  HWaddr b8:27:eb:d7:82:72

inet addr:192.168.2.19  Bcast:192.168.2.255  Mask:255.255.255.0

Now edit the website config file with this address:

sudo nano /var/www/html/config.js

Change the host ip address to your address (I've configured my address of 192.168.2.19):

 host = '192.168.2.19'; // hostname or IP address

Then press ctrl-x followed by "y" and ENTER to save.

Edit onoff.py to configure the GPIO pin you are using. It is set to pin number 7 by default, but if you are pin number 12 then change the 7's to 12 as shown here:

sudo nano onoff.py

....
def on():
        GPIO.setup(12, GPIO.OUT)
        GPIO.output(12, 1)

def off():
        GPIO.setup(12, GPIO.IN)
....

Press ctrl-X followed by y to save and exit.

Now run the Python application that will switch the GPIO on/Off:

sudo nohup python /home/pi/onoff/onoff.py &


The application will run in the background on your Raspberry Pi so you can log off and it continues to run.

You can also run onoff.py from the command line and not as a background service. This way you can see on/off messages being printed to the screen like this:

pi@raspberrypi:~/onoff $ python /home/pi/onoff/onoff.py
1511783114: New connection from 127.0.0.1 on port 1883.
1511783114: New client connected from 127.0.0.1 as 84959f71-37b1-4749-877d-7616a0d36a40 (c1, k60).
Off
On
On
On
Off
Off
Off
On
Off
On
Off
On
Off


That's all. Now you are ready to test it. Open up a browser on any computer or device on your network and type in the IP Address of your raspberry Pi. This will bring up the ON/OFF web page. When you click on ON the relay will switch on and when you click OFF the relay will switch off.
install webserver.txt
Displaying requirements.txt.
