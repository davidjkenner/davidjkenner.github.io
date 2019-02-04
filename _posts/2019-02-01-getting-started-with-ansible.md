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
  - name: update Cloud9 / Fedora / Redhat / CentOS
    shell: sudo yum update -y >> /tmp/os.out

  - name: confirm python is install
    shell: which python >> /tmp/os.out

  - name: check disk space
    shell: df -h >> /tmp/os.out

  - name: check uname
    shell: uname -a >> /tmp/os.out

  - name: memory information
    shell: cat /proc/meminfo >> /tmp/os.out

  - name: cpu info
    shell:  cat /proc/cpuinfo >> /tmp/os.out

  - name: linux release info
    shell:  cat /etc/*lease >> /tmp/os.out

  - name: ulimit
    shell: ulimit -a >> /tmp/os.out

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