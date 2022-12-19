# Raspberry Pi to GSM peer to peer communication

## Peer-to-peer Connectivity of GSM

First of all coneect the GSM Module with Pi(you can choose any method i.e serial, UART, USB).  To connect serially follow the below instructions.  

	  GSM pins    =>   RPi pins

    Tx [Pin 8]    =>    Rx
    Rx [Pin 10]   =>    Tx
    GND           =>    GND
 
Use stable recommended supply as per the GSM board to VCC Pin of GSM Module.
  
To check the port connectivity to raspberry pi, execute following command and search for something like ttyS0 (for serial), ttyUSB0 (for usb) or ttyAMA0 (for uart)  

    sudo ls /dev/tty*
 
Now connect the power supply to the GSM module (in my case it is SIM800L) via suitable adapter and voltage regulator(recommended 4.1V, 1 Amp).

## Test the Setup

If all the above setup is completes, run the python script to check if the connection is fine and module is able to communicate with RPi. 
You can run a python script named test_gsm.py by executing following command:

For python 2 :

    sudo python test_gsm.py
    
For python 3 :

    sudo python3 test_gsm.py
    
If you get "OK" as the response then its all good else check steps again. 

## Setup the PPP Connection

APN : Acess Point Name

An Access Point Name (APN) is the name of a gateway between a GSM/GPRS network and the internet. You can get your APN by serching it on google (for example "APN for Airtel"). 

After you know your APN we have to disable the serial interface by navigating to menu ==> Preferences ==> Raspberry Pi Configuration ==> interface ==> disable serial and login shell. 

To move further you should have working internet connection to Pi. 
Execute following command to update the operating system and installed packages. 

    sudo apt-get update


Then execute given below command to install PPP softwares. 

    sudo apt-get install ppp screen elinks

Further steps needs to be executed as root so login to your root account by executing the following command followed by entering password for your root account 

    sudo -i

Now navigate to /etc/ppp/peers/ and open a new file named "rnet" by executing following commands 

    cd /etc/ppp/peers/
    sudo nano rnet

Copy the content of rnet file [from this repo] in this open file and replace YOUR_NETWORK_APN with your APN you got from google. 
Well now everything is setup now we need to create the PPP connection by executing last command. 

    sudo pon rnet
    
In case you want to disable PPP connection execute following command. 

    sudo poff rnet

If you want to enable PPP connection on every boot then you can schedule it with crontab

    sudo crontab - e
	
Also you can use rc.local file to enable the PPP on every boot.

Then select any method (recommended no.2 - nano) and add the following lines to the end

    @reboot /etc/ppp/peers/
    @reboot sudo pon rnet

Now every time your Pi boots it will automatically establish PPP connection. You can check if PPP is established execute

    ifconfig

you will see the new internet port is opened with name ppp0.

**Output Console Window**

![PPP GSM Output image](https://user-images.githubusercontent.com/84657983/208369068-e2fb47b1-5925-4220-8c41-e760d27c4728.jpg)


Also you can get the LED [network and status] notification on GSM modem about the Connection [in my case status LED blinks after every 3 seconds after successful connection].

**Thank you for the reading.**

**Always open for Any suggestions and Help.**

