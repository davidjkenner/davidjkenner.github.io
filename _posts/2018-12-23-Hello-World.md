---
layout: post
title: Cloud9 on AWS
---

A look at Cloud9 environment running in AWS (shown below).

![_config.yml]({{ site.baseurl }}/images/cloud9-console.png)

AWS Cloud9 provides both a GUI interface complete with hotkeys and also a command line.
AWS Cloud9 already has AWS CLI, SAM and git installed.
Terraform and Ansible will need to be installed

To install Ansible
{% highlight ruby %}
sudo yum  install ansible
ansible --version
ansible --help
ansible-playbook --version
ansible-playbook --help
ansible-galaxy  --help
{% endhighlight %}

To install Terraform
{% highlight ruby %}
sudo yum install terraform
terraform --version
terraform --help
{% endhighlight %}