---
layout: post
title: Let's Encrypt Certificates
date: 2018-12-31 19:59 +0000
---
![_config.yml]({{ site.baseurl }}/images/letsEncrypt.png)

Let’s Encrypt is a free Certificate Authority (CA) that issues SSL certificates. Let’s Encrypt certificates are only valid for 90 days. You will need to install the lego client on Cloud9.

{% highlight ruby %}
# Variables
export PUBLIC_IP_ADDRESS=123.XXX.YYY.ZZ
export DOMAIN=example.com
export EMAIL_ADDRESS=dkenner@example.com
export APACHE_CONF=/opt/apache2/conf
export LEGO_PATH=/etc/lego

# Stop the Apache server

# Request a new certificate for your domain 
lego --email="$EMAIL_ADDRESS" --domains="$DOMAIN" --domains="www.$DOMAIN" --path="/etc/lego" run

# confirm they were built
ls -al $LEGO_PATH/certificates | grep $DOMAIN.crt
ls -al $LEGO_PATH/certificates | grep $DOMAIN.key

# Link the new SSL certificate and certificate key file to the correct locations, for Apache Web Server. 

# Update the file permissions to make them readable by the root user only.
mv $APACHE_CONF/server.crt $APACHE_CONF/server.crt.old
mv $APACHE_CONF/server.key $APACHE_CONF/server.key.old
mv $APACHE_CONF/server.csr $APACHE_CONF/server.csr.old
ln -s $LEGO_PATH/certificates/$DOMAIN.key $APACHE_CONF/server.key
ln -s $LEGO_PATH/certificates/$DOMAIN.crt $APACHE_CONF/server.crt
chmod 600 $APACHE_CONF/server*

# Start the Apache server
{% endhighlight %}