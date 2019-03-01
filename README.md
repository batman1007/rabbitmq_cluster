# RabbitMQ Cluster Challenge
Vagrant Ansible RabbitMQ Cluster Challenge

This repository is for a demo of installing and configuring a very basic RabbitMQ Cluster.

It uses Vagrant to create virtual machines and Ansible to configure RabbitMQ.

# Setup
Windows 10 was the base operating system, with following pre requisites installed:
* Git Bash or some other tool for cloning Git repositories
* VirtualBox (v6.0 Used)
* Vagrant (v2.2.4 Used)
  * vagrant-hosts plugin installed
  
It assumes that you are not behind a proxy and you have full internet access without restrictions.

# HowTo
* Install above software
* Clone this repo (Please make sure that your git program does not reset line endings on files as this will create problems when running - they must be set to Unix EOL)
* Run 'vagrant up' in the cloned repo folder
* Browse to http://10.1.172.21:15672 to check the setup of RabbitMQ
