Before we begin, this aims to be used in lab environment only.

Playbooks stated here deploy a AWX application as well a monitoring service using ELK stack.

The lab used for this purpose was:

* VMWare
* Two VM instances of CentOS 8.3.
  * ISO used was boot with the mirror: http://mirror.centos.org/centos/8/BaseOS/x86_64/os/
  * A minimal install.
  * And the Ansible user created during install.

Upon clone, please don't forget to update de *hosts* file inside the inventory folder.
The IP address are a mere sugestion, be sure to provide your own. Don't forget to change also in the awx.yml file.
Please update de user and password in the *all* file inside group_vars.

If you wish just to deploy AWX you can skip the execution of the monitoring.yml playbook. And set the default variable *'allow_monitoring'* inside the role of awx_setup to *false*.

The meet Ansible requirements, since python wasn't installed during minimal install, you'll need to RAW and install python38 first.
So, for CentOS and RHEL distros, please run:

* ansible-playbook -i inventory/hosts requirements_centos_rhel.yml

This will install and output the current Python Version!

First things first,

Please setup the Monitoring VM using:

* ansible-playbook -i inventory/hosts monitoring.yml

After this, 

Then, since this is a Home Lab and I don't have an existing OpenShift or Kubernetes cluster, we'll use:

* minikube

So, please run and also change the defaults inside the awx_setup role:

minikube_cpu: 2
minikube_ram: 4g

* ansible-playbook -i inventory/hosts awx.yml


There are 3 major roles in this playbook:

* minimal_nginx
  * This one installs nginx and configures selinux.


* awx_docker
  * This installs and configures docker.


* awx_setup
  * This will use docker, and will configure nginx after the installation. 
  * In this role we install:
    * minikube
    * all pods needed for AWX
    * and configure the firewall

Detailed information is inside each tasks/main.yml.

Please note this is an in development version of AWX. Things may break in the future.

After completion, you may go to the IP of your AWX machine in the web browser, and login as:
- admin
- changeme

Also, monitoring is available using the ip address of your monitoring host.
If you don't want to enable monitoring in your AWX host, please comment all from the elk_metricbeat role to the bottom of the awx.yml file.
