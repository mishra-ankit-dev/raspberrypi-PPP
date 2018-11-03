# Peer-to-peer

First of all coneect the GSM Module with Pi(you can choose any method i.e serial, UART, USB).  To connect serially follow the below instructions.  

GSM pins   =>   Pi pins

    Tx     =>    Rx
    Rx     =>    Tx
    GND    =>    GND
    
To check if connected execute following command and search for something like ttyS0 (for serial), ttyUSB0 (for usb) or ttyAMA0 (for uart)  

    sudo ls /dev/tty*
 
Now connect the power supply to the GSM module (in my case it is SIM 900A) via suitable adapter (recommended 1 Amp). 
If all the above setup is complete run the python script to check if the connection is good and module is able to communicate with Pi. 
You can run a python script named gsm_test.py by executing following command:

For python 2 :

    sudo python gsm_test.py
    
For python 3 :

    sudo python3 gsm_test.py
    
If you get "OK" as the response then its all good else check steps again. 


APN : Acess Point Name

An Access Point Name (APN) is the name of a gateway between a GSM/GPRS network and the internet.You can get your APN by serching it on google (for example "APN for Vodafone"). 

After you know your APN we have to disable the serial interface by navigating to menu ==> Preferences ==> Raspberry Pi Configuration ==> interface ==> disable serial. 

To move further you should have working internet connection to Pi. 
Execute following command to update the operating system. 

    sudo apt-get update

Then execute given below command to install PPP softwares. 

    sudo apt-get install ppp screen elinks

Further steps needs to be executed as root so login to your root account by executing the following command followed by entering password for your root account 

    sudo -i

Now navigate to /etc/ppp/peers/ and open a file named "rnet" by executing following commands 

    cd /etc/ppp/peers/
    sudo nano rnet

Copy the content of rnet file in this open file and replace YOUR_NETWORK_APN with your APN you got from google. 
Well now everything is setup now we need to create the PPP connection by executing last command. 

    sudo pon rnet
    
In case you want to disable PPP connection execute following command. 

    sudo poff rnet

If you want to enable PPP connection on every boot then you can schedule it with crontab

    sudo crontab - e

Then select any method (recommended no.2 - nano) and add the following line to the end

    @reboot /etc/ppp/peers/
    @reboot sudo pon rnet

    
Now every time your Pi boots it will automatically establish PPP connection. You can check if PPP is established execute

    ifconfig
