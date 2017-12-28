# RaspberryUsbStick
Connect a RDS&amp;RCS usb 3G stick to raspberry pi

Install the required packages

    sudo apt-get install wvdial usb-modeswitch
Run the command   
    lsusb 
For the 3G dial-up to work, a device link file /dev/gsmmodem is required. 
Check with ls -al /dev/gsm* if the file is present – it most probably is not!

  ls -al /dev/gsm*
  
Now unplug the modem from the USB-hub and plug the modem back.

Run the command   to check that the modem is visible
    ls -al /dev/gsm*
    
Now configure wvdial, file:

sudo nano /etc/wvdial.conf

[Dialer Defaults]
Init1 = ATZ
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
Modem Type = Analog Modem
ISDN = 0
#Baud = 460800
Baud = 9600
New PPPD = yes
Modem = /dev/ttyUSB0

[Dialer pin]
Init3 = AT+CPIN=1234

[Dialer rds]
Phone = *99***1#
#Init3 = AT+CGDCONT=1,"IP","internet"
Username = internet
Password = internet
Stupid mode = 1


Now configure the ppp/wvdial, file:

sudo nano /etc/ppp/peers/wvdial

noauth
name wvdial
usepeerdns
#noccp

Final step is to run the dialer:

sudo wvdial pin

sudo wvdial rds

