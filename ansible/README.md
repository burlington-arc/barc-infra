
# Setting up a new Node

The following instructions describe the steps to setting up a new Raspberry Pi
node for the digipeater.  This can be done on an SD card image, which can then
be installed at the repeater shack.

1. Download and burn an image of
[Raspbian Jessie Lite] (https://www.raspberrypi.org/downloads/raspbian/)
according to the instructions on that page.

2. Boot the pi using the new image.

3. Use 'sudo raspi-config' to set the following items:  
  ssh: on  
  hostname: as required  

4. Reboot the pi, so the new hostname is activated.  Make sure the pi is on the
local network.

5. Use 'ifconfig' to find out the pi's IP address.
