## How to secure Tomcat 7 with SSL / TLS ##
[Source](http://tkurek.blogspot.fi/2013/07/how-to-secure-tomcat-7-with-ssl-tls.html)

### Intro

The following article shows how to secure Tomcat 7 servlet container with SSL / TLS. Although there might me numerous different solutions (e.g. proxying from Apache server) the one that I present bases on Tomcat only and utilizes its default configuration files. 

The following assumptions have been made for the rest of the article:

* **OS:** Ubuntu 12.04 Server x64
* **Tomcat:** tomcat7 installed from official Ubuntu repositories (apt-get install tomcat7)
* **Tomcat** user: tomcat7
* **Tomcat** home directory: /etc/tomcat7
* **SSL / TLS port:** TCP port 8443
* **Keystore location:** /etc/tomcat7/keystore.jks
* **Keystore password:** keystore

The following section describes a detailed steps required for securing Tomcat 7 with SSL / TLS. 
All the commands have been run as a tomcat7 user.

### Configuration

**1)** Generate the keystore file that will store the certificates trusted by the Tomcat server. Depending on your needs this step may require invoking different commands. A general HowTo regarding the keytool tool usage can be found here.

In my case the keystore file was already delivered to me by my CA when requesting the certificate. However having both the CA and the CA-signed certificate you can easily create the keystore file by running the following commands:

~~~~
keytool -import -trustcacerts -alias root
-file [CA cert path] -keystore /etc/tomcat7/keystore.jks
~~~~

~~~~
keytool -import -trustcacerts -alias tomcat
-file [CA-signed cert path] -keystore /etc/tomcat7/keystore.jks
~~~~

Or to generate the self-signed certificate:

~~~~
keytool -genkey -keyalg RSA -alias tomcat
-keystore /etc/tomcat7/keystore.jks -storepass keystore
-validity 360 -keysize 2048
~~~~

**2)** Update /etc/tomcat7/server.xml configuration file and change the following part of its configuration:

~~~~
<!--
<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
           maxThreads="150" scheme="https" secure="true"
           clientAuth="false" sslProtocol="TLS" />
 -->
~~~~

to what's shown below:

~~~~
<Connector protocol="org.apache.coyote.http11.Http11NioProtocol"
           port="8443" SSLEnabled="true" maxThreads="200"
           scheme="https" secure="true"
           keystoreFile="/etc/tomcat7/keystore.jks"
           keystorePass="keystore" clientAuth="false"
           sslProtocol="TLS" />
~~~~

**3)** As a root user restart Tomcat by running the following command:

~~~~
/etc/init.d/tomcat7 restart
~~~~

You're done! The Tomcat is now secured with SSL / TLS on port 8443.

### Related topics:

- http://tkurek.blogspot.com/2013/07/tomcat-7-http-to-https-redirect.html