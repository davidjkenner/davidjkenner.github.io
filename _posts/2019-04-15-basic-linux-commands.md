---
layout: post
title: Basic Linux commands
date: 2019-04-15 22:24 +0000
---
Some useful commands to get you started with AWS Cloud9 terminal.  

{% highlight ruby %}
pwd     				# present working directory
ls -al       			# list all in directory
ls -altr 				# list in reverse order, last item you worked on the last line
whoami      			# your current user
sudo su -     			# switch to root
aws help     			# help for the aws client
sam --help     			# help for serverless application manager
cd      				# change directory
cd ~                    # takes you back to your users home directory
cp				        # to copy a file or directory(s) can be used to make backups
cat /etc/*lease      	# version of Linux you are using
wget				    #  will get a file and download it
unzip				    # unzip a file
mv				        # move a file
mkdir				    # make a directory
find . -name $FILE_NAME	# find command
type -t                 # To see if a command is aliased, a file, or a built-in command
which       			# tells if it is a command and it’s location, ie which mysqldump
chown       			# change owner
chmod				    # change permissions of a file, ie chmod 600 LightsailDefaultKey-us-west-2.pem 
export      			# export a variable, ie export PUBLIC_IP=54.84.221.54
echo        			# confirm variable is correctly set
ssh     				# securely remote into another Linux server, ie 
				        # ssh -i LightsailDefaultKey-us-west-2.pem bitnami@$PUBLIC_IP
netstat -ano | grep LISTEN	# show what ports are being listened on
date				    # date on the server
rm –rf				    # forced removal of file or reclusive directories
history                 # shows commands you ran
view .bash_history    	# view a file
clear                   # clears your screen
shutdown -P            		#  Power-off the machine
{% endhighlight %}