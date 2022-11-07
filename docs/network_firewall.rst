Set Up the Network Firewall
===========================

Now that you've set up your password manager, you can move on to setting up
the Network Firewall. You should stay logged into the *Admin Workstation* to
access the Network Firewall's web interface for configuration.

Unfortunately, due to the wide variety of firewalls that may be used, we
do not provide specific instructions to cover every type or variation in
software or hardware. However, if you have the necessary expertise, we
provide `abstract firewall rules`_ that can be implemented with iptables, Cisco
IOS etc. We recommend that you use a firewall with at least four physical interfaces.

The documentation linked below describes the configuration procedure for pfSense-
and OPNSense-based firewalls. One option not covered in this guide is to build
your own network firewall and `install pfSense
<https://docs.netgate.com/pfsense/en/latest/install/download-installer-image.html>`__
or `OPNSense <https://opnsense.org/download/>`__ on it. However, for most
installations, we recommend buying a dedicated firewall appliance with
your firewall OS of choice pre-installed.

We currently recommend three firewalls in our :ref:`Hardware Guide <hardware_guide>`:

* The `Netgate SG-4100 <https://shop.netgate.com/products/4100-base-pfsense>`__,
  a pfSense-based firewall with 6 network interfaces: 2 WAN ports and 4 LAN ports.

* The `Netgate SG-6100 <https://shop.netgate.com/products/6100-base-pfsense>`__,
  a pfSense-based firewall with 8 network interfaces: 4 WAN ports and 4 LAN ports.

* The `TekLager APU4D4 <https://teklager.se/en/products/routers/apu4d4-open-source-router>`__,
  an OPNSense-based open-source hardware firewall with 4 network interfaces: WAN,
  LAN, OPT1, and OPT2.

Configuration: pfSense
----------------------
If you are using a pfSense-based firewall such as the recommended SG-4100, follow
the instructions to :ref:`Configure a pfSense firewall for use with SecureDrop <firewall_pfsense>`.

Configuration: OPNSense
-----------------------
If you are using an OPNSense-based firewall such as the recommended APu4D4, follow
the instructions to :ref:`Configure an OPNSense firewall for use with SecureDrop <firewall_opnsense>`.

Configuration: Other Firewalls
------------------------------

If you are using a firewall based on an OS not listed above, you should still set it up
use the same overall configuration and ruleset as defined for the supported models.

The *Application* and *Monitor Servers* should be set up on separate subnets configured on
separate physical NICs, with the *Admin Workstation* also on a separate subnet if possible.
Including the WAN connection, a minimum of 4 NICs must be available.

The abstract ruleset required by SecureDrop can be described as follows:

.. _abstract firewall rules:

* Disable DHCP (in case the firewall is providing a DHCP server by default)
* Disallow all traffic by default (inbound or outbound)
* Allow UDP OSSEC (port 1514) from *Application Server* to *Monitor Server*
* Allow TCP ossec agent auth (port 1515) from *Application Server* to *Monitor Server*
* Allow TCP/UDP DNS from *Application Server* and *Monitor Server* to the IPs of known name servers
* Allow UDP NTP from *Application Server* and *Monitor Server* to all
* Allow TCP any port from *Application Server* and *Monitor Server* to all (this is needed for making connections to the Tor network)
* Allow TCP 80/443 from *Admin Workstation* to all (in case there is a need to access the web interface of the firewall)
* Allow TCP SSH from *Admin Workstation* to *Application Server* and *Monitor Server*
* Allow TCP any port from *Admin Workstation* to all

This can be implemented with iptables, Cisco IOS etc. if you have the necessary
expertise.
