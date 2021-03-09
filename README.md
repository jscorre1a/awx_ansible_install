This project was created in a Home Lab using:

* VMWare
* CentOS 8.3 boot iso.
  * With the mirror: http://mirror.centos.org/centos/8/BaseOS/x86_64/os/
  * A minimal install.
  * And the Ansible user created during install.

The meet Ansible requirements, since python wasn't installed during minimal install, you'll need to RAW and install python38 first.
So, for CentOS and RHEL distros, please run:

* ansible-playbook -i inventory/hosts requirements_centos_rhel.yml

This will install and output the current Python Version!

Then, since this is a Home Lab and I don't have an existing OpenShift or Kubernetes cluster, we'll use:

* minikube

After this, you'll need to run the main.yml

Note that, there are 3 major roles stated here:

* awx_nginx
  * This one installs and configures nginx and selinux.


* awx_docker
  * This installs and configures docker.


* awx_setup
  * This will use docker, and will configure nginx after the installation and configuration of:
    * minikube
    * all pods needed for AWX
    * configuration of the firewall

Detailed information inside each tasks main.yml.

Please note this is a development version of AWX. So... Things may break in the future.

After completion, you may go to the IP of your machine in the web browser, and login as:
- admin
- changeme