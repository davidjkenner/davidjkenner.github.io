---
layout: post
title: AWS EC2 creation using Terraform
date: 2018-12-31 21:04 +0000
---
![_config.yml]({{ site.baseurl }}/images/ec2-terraform.png)

You can use Terraform to create an EC2 on AWS. The following will create a VM with Amazon Linux (Fedora) on the Free Tier of AWS. It will open both ports 22 and 80 for ssh and http.

{% highlight ruby %}
# HOW TO RUN PROGRAM
# ~/terraform -init  # to initialize plugins
# ~/terraform -plan  # to test .tf files
# ~/terraform -apply # to run .tf files

# ------------------------------------------------------------------------------

# ------------------------------------------------------------------------------

# AVAILIBILITY ZONE  https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html

# ------------------------------------------------------------------------------

provider "aws" {
shared_credentials_file = "~/.aws/credentials"
 profile  = "default"
 region = "us-west-2"
}

##################################################################
# Data sources to get VPC, subnet, security group and AMI details
##################################################################

data "aws_vpc" "default" {
 default = true

}
data "aws_subnet_ids" "all" {
 vpc_id = "${data.aws_vpc.default.id}"
}

data "aws_ami" "amazon_linux" {
 most_recent = true

 filter {
  name = "name"
  values = [
   "amzn-ami-hvm-*-x86_64-gp2",
  ]
 }

 filter {
  name = "owner-alias"
  values = [
   "amazon",
  ]
 }
}

module "security_group" {
 source   = "terraform-aws-modules/security-group/aws"
 version   = "2.7.0"
 name    = "example"
 description = "Security group for example usage with EC2 instance"
 vpc_id   = "${data.aws_vpc.default.id}"
 ingress_cidr_blocks = ["0.0.0.0/0"]
 ingress_rules    = ["http-80-tcp", "all-icmp"]
 ingress_rules    = ["ssh-tcp", "all-icmp"] # will throw error but still works
 egress_rules    = ["all-all"]
}

resource "aws_eip" "this" {
 vpc   = true
 instance = "${module.ec2.id[0]}"
}

module "ec2" {
 source = "terraform-aws-modules/ec2-instance/aws" # https://github.com/terraform-aws-modules/terraform-aws-ec2-instance

 instance_count = 1
 name            = "example-normal"
 ami             = "${data.aws_ami.amazon_linux.id}"
 instance_type        = "t2.micro"
 key_name          = "$KEY_NAME"
 subnet_id          = "${element(data.aws_subnet_ids.all.ids, 0)}"
 vpc_security_group_ids   = ["${module.security_group.this_security_group_id}"]
 associate_public_ip_address = true
}
{% endhighlight %}