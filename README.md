# OCP Installation Guide
OCP 4. x Installation Guide for vSphere deployment with static IPs

## Pre-req Steps
For a static IP install you need:
1. DNS
2. HAProxy

You can use a single VM as bastion host to support DNS and HAProxy.  For my setup, I use Raspberry PI for DNS and a time source running chronyd

PiHole uses DNSMASQ for DNS
- documentation to follow
<instruction and config file DNS configuration with PiHole link go here>
  
Optional informaion for setting chronyd with GPS to create a stratum 1 time server source.
- documentation to follow

HAProxy configuration on bastion server
- documentation to follow

## OCP Prep Steps

1. Download OCP files
    - Download OCP 4.4.x client files -> https://mirror.openshift.com/pub/openshift-v4/clients/ocp/
      - openshift-install-mac-4.4.3.tar.gz
      - openshift-install-linux-4.4.3.tar.gz
    - Download OCP 4.4.x files -> https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/
      - rhcos-4.4.3-x86_64-installer.x86_64.iso
      - rhcos-4.4.3-x86_64-metal.x86_64.raw.gz
2. 

  
  


