## Set up Tomcat to redirect HTTP requests to HTTPS ##
[Source](http://krangsquared.blogspot.fi/2007/09/set-up-tomcat-to-redirect-http-requests.html)

We had JIRA installed and using the built-in Tomcat app server,
accepting both HTTP and HTTPS requests. We wanted to restrict it so
that all JIRA access was only via SSL. The config changes were pretty
simple.

**First** - Change Tomcat's server.xml.
Edit the non-SSL <Connector> entry listening on port 80 and add or
edit the redirectPort atribute to point to the port on which the SSL
<Connector> is listening. By default, the redirectPort was pointing
to port 443.

Was:

~~~~
<Connector port="80"
           enableLookups="false" redirectPort="8443"
           maxThreads="100" minSpareThreads="100" maxSpareThreads="100"/>
~~~~

Changed to:

~~~~
<Connector port="80"
           enableLookups="false" redirectPort="443"
           maxThreads="100" minSpareThreads="100" maxSpareThreads="100"/>
~~~~

**Second** - In the Tomcat web.xml file the following <security-constraint> has
to be added within the <web-app> element. This new element must be
added after the <servlet-mapping> element:

~~~~
<!-- SSL settings. only allow HTTPS access to JIRA -->
<security-constraint>
<web-resource-collection>
<web-resource-name>Entire Application</web-resource-name>
<url-pattern>/*</url-pattern>
</web-resource-collection>
<user-data-constraint>
<transport-guarantee>CONFIDENTIAL</transport-guarantee>
</user-data-constraint>
</security-constraint>
~~~~