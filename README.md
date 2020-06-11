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
- note: when setting up HAProxy if there is a binding issue to 0.0.0.0:xxxx, you may need to set the Secure Linux with this command: setsebool -P haproxy_connect_any=1

HTTP
- set httpd.conf to have httpd listen on 8080 -> Listen 8080

Firewall
- open the services - services: http https ntp ssh
- open thse ports:  ports: 8080/tcp 22623/tcp 6443/tcp 9000/tcp


## OCP Prep Steps

1. Download OCP files
    - Download OCP 4.4.x Installer -> https://mirror.openshift.com/pub/openshift-v4/clients/ocp/
      - openshift-install-mac-4.4.3.tar.gz
      - openshift-install-linux-4.4.3.tar.gz
    - Download OCP 4.4.x files -> https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/
      - rhcos-4.4.3-x86_64-installer.x86_64.iso
      - rhcos-4.4.3-x86_64-metal.x86_64.raw.gz
    - Download OCP 4.4.x client files -> https://mirror.openshift.com/pub/openshift-v4/clients/ocp/
      - openshift-client-linux-4.4.3.tar.gz	
      - openshift-client-mac-4.4.3.tar.gz
    - upload rhcos-4.4.3-x86_64-metal.x86_64.raw.gz file to the http server.  -> /var/www/html/image
    - extract rhcos-4.4.3-x86_64-installer.x86_64.iso to a temporary location
      - sudo mount rhcos-410.8.20190425.1-installer.iso /mnt/ 
      - mkdir /tmp/rhcos 
      - rsync -a /mnt/* /tmp/rhcos/ 
      - cd /tmp/rhcos 
      - create each OCP member ISO (bootstrap, master01, master02, master03, worker01, worker02, worker03 by editing the        isolinux.cfgf file
        - vi isolinux/isolinux.cfg. See this file for the edit and command to create each OCP Member ISO  ->              https://github.com/pslucas0212/OCPInstallGuide/blob/master/OCPConfigFiles/ISOConfig.ocp4
      - Upload each file to the VMWare cluster
2. Generate SSH key
    - ssh-keygen -t rsa -b 4096 -N '' -f ~/.ssh/vsphere-ocp43
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/vsphere-ocp43
3. Copy or download the pull secret from cloud.redhat.com
    - Go to cloud.redhat.com
    - Login with your credential (create an account if you don’t have one)
    - Click “Create Cluster”
    - Click OpenShift Container Platform
    - Scroll down and click “VMware vSphere”
    - Click on “Download Pull Secret” to download the secret
4. Create an installation directory
    - mdkir ~/ocp4
    - create install-config.yaml and place it in the install directory.  My install-config.yaml is here -> https://github.com/pslucas0212/OCPInstallGuide/blob/master/OCPConfigFiles/install-config.yaml.ocp4
5. Generate Kubernetes manifests for the cluster
    - ./openshift-install create manifests --dir=./ocp4
6. edit the  <installation directory>/manifests/cluster-scheduler-02-config.yml file
    - change the mastersSchedulable to false
7. Obtain Ignition files:
    - ./openshift-install create ignition-configs --dir=./ocp4
8. Move the ignition files to the http server
    - /var/www/html/ignition
    - append-bootstrap.ign (that created it), bootstrap.ign, master.ign, worker.ign


  
  


