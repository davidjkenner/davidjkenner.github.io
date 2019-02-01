---
layout: post
title: Getting started with Ansible
date: 2019-02-01 17:30 +0000
---
Getting Started with Ansible:

Checklist:
* AWS Account
* EC2 sandbox 
*   called test-box 
*   uses a key pair (ie cloud9.pem)
*   Fedora\Redhat\Centos operating system
*   IP Address of test-box
* Download the pem file to Cloud9 (ie cloud9.pem)
* Cloud9 up and running


Installing Ansible on Cloud9 (or Fedora/Redhat/Centos)
{% highlight ruby %}
which python			# confirm that python 2.7+ is installed
yum  install ansible   	# install ansible on Cloud9
vim /etc/ansible/hosts	# update to add hosts & ip address
{% endhighlight %}

Now we create main.yml file that will run shell commands for ec2-user on a sandbox called test-box.
NOTE: yml arrangement must be perfect, yml is not forgiving
{% highlight ruby %}
---
- hosts: test-box
  remote_user: ec2-user
  tasks:
  - name: delete log file
    shell: rm -rf /tmp/ansible.out

  - name: cat hosts file
    shell: cat /etc/hosts >> /tmp/ansible.out

  - name: check disk space
    shell: df -h >> /tmp/ansible.out

  - name: update environmnet
    shell: sudo yum update -y >> /tmp/ansible.out

  - name: install apache2 
    shell: sudo yum install apache2 -y >> /tmp/ansible.out

{% endhighlight %}

Now its time to run Ansible
{% highlight ruby %}
ansible-playbook  --key-file cloud9.pem  main.yml
{% endhighlight %}

Now its time to confirm that Ansible ran some commands successfully
review the ansible.out log and that Apache2 was installed
{% highlight ruby %}
ssh -i "cloud9.pem" ec2-user@$IPADDRESS
{% endhighlight %}