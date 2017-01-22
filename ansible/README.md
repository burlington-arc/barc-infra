
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
  hostname: as required (for the rest of this doc, we'll call it 'barcpi003')

4. Reboot the pi, so the new hostname is activated.  Make sure the pi is on the
local network.  

5. Use 'ifconfig' to find out the pi's IP address.

6. Make an entry into the local '/etc/hosts' for the hostname and ip address.

7. Test the ssh connection.

8. ssh into the pi and copy your public key to its 'authorized_keys' file under
the 'pi' user, as follows:

    cat ~/.ssh/id_rsa.pub | ssh pi@barcpi003 "cat >>~/.ssh/authorized_keys"  

8. Add the new host to the 'hosts' file and also add a 'host_vars' file for the
new host.

9. Setup the remote access and 'phone-home' service:

    ansible-playbook -i hosts -l barcpi003 remote-access.yml

10. If the new node needs aprx, run the 'install-aprx' playbook.

    ansible-playbook -i hosts -l barcpi003 install-aprx.yml

11. Run the site playbook to configure the new node for the rest of the infrastructure.

    ansible-playbook -i hosts -l barcpi003 site.yml
