# Burlington Amateur Radio Club Infrastructure Setup

Note: In the interest of sharing technology with other Amateur Radio clubs and
operators, this repository is publicly readable, but there are no useful keys or
credentials stored here!

## Background

Burlington Amateur Radio Club operates a repeater system that has grown to
include significant computer infrastructure.  There is an APRS digipeater and
iGate, a D-Star repeater and gateway, and an IRLP gateway attached to the
analog repeater.

In addition, the internet service at the repeater site is not routable from the
outside, so in order to allow inbound connections, we need an external gateway.
For reliability, we actually have more than one gateway; two are located in
members' homes, and one is in the Amazon EC2 cloud.

All this computing technology makes for a management challenge; who know how
it's setup and how to configure it?  We've all been in situations where
somebody is up at the repeater site and needs to edit a file to make things work.
Do they always document it?  Do we always do knowledge transfer so we aren't
dependent on one person, who might be out of town sometimes?  And what happens
if there's an equipment failure?  We use Raspberry Pis at the repeater site
because they're cheap, have I/O and are easy to run off 12V. But they're not
really industrial controllers; in particular they tend to burn out SD cards
after a while and the software needs to be rebuilt.

These problems aren't unique to Amateur Radio.  In fact, they have been
addressed by enterprise computing and cloud computing.  So, why not take some
of the tools used by enterprise infrastructure people and automate the
radio club infrastructure?

This repository is the set of Ansible playbooks and data that is used to
setup and maintain the Burlington Amateur Radio Club computing/communications
infrastructure.  [Ansible] (https://www.ansible.com/)
is a "configuration management system" that sets up remote computer systems
according to a 'playbook' that states the final configuration of the system.

By making sure that all the setup is in the Ansible playbook rather than editing
files or doing manual setups, the setup is documented and is repeatable.  It
can be duplicated easily on new hardware, and reconfiguration can be accomplished
simply by changing the playbook files.  All members who administer the systems
are granted permissions on the GitHub repository so that their changes can be
posted to a common area.

## Remote access

We have an internet connection through a cellular modem at the repeater site.
Like many residential- or consumer-grade internet connections, it is primarily
designed to allow the 'user' side to connect to internet servers.  It isn't
reachable from the internet.  So, in order to allow for remote connections into
the repeater infrastructure, the devices on the cellular modem reach out to
a 'gateway' server that _is_ available on the internet, and establish reverse
port forwarding from the internet-facing server to the remote devices.

## D-Star Gateway

We operate a D-Star repeater that is run by a Raspberry Pi computer attached to
a Yaesu Fusion repeater.  We naturally want our members to be able to use
D-Star's callsign routing functionality and inbound repeater linking.  In order
to make this work, we need a somewhat complicated architecture.

The D-Star gateway uses the [DL5DI OpenDV Software](https://github.com/dl5di/OpenDV).
This system is designed as two separate pieces; there is the D-Star Repeater
component, which actually communicates with the repeater, decoding GMSK into bits,
then implementing the D-Star network protocol, and there is the ircddbgateway
component, which routes audio and control signals from the repeater to other
repeaters on the internet.

## Virtual Private Network.

For the VPN setup mentioned above, we use openvpn.  

openvpn uses X.509 certificates to authenticate its clients.  So, we need to setup
a certifying authority to create the certificates for all the openvpn components.

The CA can be hosted anywhere; in our case we're hosting it at the gateway machine,
so we don't have to move actual certs around if we need a new one, and there's no
chance of the CA cert's private key being known, since it will never leave the
gateway machine.  The 'setup-ca.yml' playbook sets up the certifying authority.
