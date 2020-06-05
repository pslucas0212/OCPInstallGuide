# OCP Installation Guide
OCP 4. x Installation Guide for vSphere deployment with static IPs

## Pre-req Steps
For a static IP OCP install you need:
1. DNS - ns01.example.com - Pi 4
2. HAProxy - bastion.example.com - RHEL 8 VM on VMWare
3. HTTP - bastion.example.com - RHEL 8 VM on VMWare
4. Configure firewall - bastion.example.com - RHEL 8 VM on VMWare

You can use a single VM as bastion host to support DNS and HAProxy.  For my setup, I use Raspberry PI for DNS and a time source running chronyd

PiHole uses DNSMASQ for DNS
- For the DNS configuration on the PiHole DNSMAQ server, configure/edit four files
  - hosts for dns static naming
  - 02-land-domain.dnf for reverse lookup of ocp machines
  - 20-srv-settings.conf for SRV record lookup
  - xx-wildcard.conf for wild card lookup
  - see the dnsmasq config files here -> https://github.com/pslucas0212/OCPInstallGuide/tree/master/DNSconfig
  
Optional informaion for setting chronyd with GPS to create a stratum 1 time server source.
- documentation to follow

HAProxy configuration on bastion server
- configure HAProxy config files - my haproxy.conf files are here -> https://github.com/pslucas0212/OCPInstallGuide/tree/master/haproxyconfig

HTTP
- set httpd.conf to have httpd listen on 8080 -> Listen 8080

Firewall
- open the services - services: http https ntp ssh
- open thse ports:  ports: 8080/tcp 22623/tcp 6443/tcp 9000/tcp


## OCP Prep Steps

1. Download OCP files
    - Download OCP 4.4.x client files -> https://mirror.openshift.com/pub/openshift-v4/clients/ocp/
      - openshift-install-mac-4.4.3.tar.gz
      - openshift-install-linux-4.4.3.tar.gz
    - Download OCP 4.4.x files -> https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/
      - rhcos-4.4.3-x86_64-installer.x86_64.iso
      - rhcos-4.4.3-x86_64-metal.x86_64.raw.gz
2. 

  
  


