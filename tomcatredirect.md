## Tomcat 7 HTTP to HTTPS redirect
[Source](http://tkurek.blogspot.fi/2013/07/tomcat-7-http-to-https-redirect.html)

### Intro

The following article shows how to easily redirect HTTP to HTTP in Tomcat 7 servlet container that it always requires secure connection. It was assumed that the following TCP ports are used for that purpose:    
- 8080: for HTTP    
- 8443: for HTTPS

Please, follow the exact steps as described below to get it done.

### Configuration

**1)** Update server.xml configuration file in Tomcat home directory and change the following part of its configuration:

~~~~
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           URIEncoding="UTF-8"
           redirectPort="8443" />
~~~~

to what's shown below:

~~~~
<Connector port="8080" enableLookups="false"
           redirectPort="8443" />
~~~~

**2)** Update web.xml configuration file in Tomcat home directory and add the following content into the end before the closing `</web-app>` markup:

~~~~
<security-constraint>
<web-resource-collection>
<web-resource-name>Protected Context</web-resource-name>
<url-pattern>/*</url-pattern>
</web-resource-collection>
<!-- auth-constraint goes here if you requre authentication -->
<user-data-constraint>
<transport-guarantee>CONFIDENTIAL</transport-guarantee>
</user-data-constraint>
</security-constraint>
~~~~

**3)** Restart Tomcat servlet container.

You're done! The Tomcat always requires secure connection now.

### Related topics:

* http://tkurek.blogspot.com/2013/07/how-to-secure-tomcat-7-with-ssl-tls.html