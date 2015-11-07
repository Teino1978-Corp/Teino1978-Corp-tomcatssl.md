## Tomcat 6 under secured Nginx?

In Big Blue Button Tomcat 6 is used under Nginx.

Tomcat documentation for ["SSL - How to"](http://tomcat.apache.org/tomcat-6.0-doc/ssl-howto.html) 
in "SSL and Tomcat" section, clames that:    
"It is important to note that configuring Tomcat to take advantage of secure sockets is usually only necessary when running it as a stand-alone web server. When running Tomcat primarily as a Servlet/JSP container behind another web server, such as Apache or Microsoft IIS, it is usually necessary to configure the primary web server to handle the SSL connections from users. Typically, this server will negotiate all SSL-related functionality, then pass on any requests destined for the Tomcat container only after decrypting those requests. Likewise, Tomcat will return cleartext responses, that will be encrypted before being returned to the user's browser. In this environment, Tomcat knows that communications between the primary web server and the client are taking place over a secure connection (because your application needs to be able to ask about this), but it does not participate in the encryption or decryption itself."

If Tomcat "knows" that communications are secure, how come "request.isSecure()" returns "false"?