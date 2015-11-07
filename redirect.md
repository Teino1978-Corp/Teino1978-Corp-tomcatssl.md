## Redirect HTTP to HTTPS on Tomcat ##

[Source](http://www.coolestguidesontheplanet.com/redirecting-http-https-tomcat/)

You’ve bought your SSL secure certificate and successfully installed on Tomcat with the keytool but how do your redirect the entire site to go HTTPS and redirect any HTTP connection straight over to HTTPS.

You need to edit the 2 Tomcat configuration files; server.xml and web.xml and then when edited restart the tomcat service.

Open **server.xml** typically found in tomcat/conf and change:

~~~~
<Connector port="80"
           enableLookups="false"
           redirectPort="8443" />
~~~~
 
to

~~~~
<Connector port="80"
           enableLookups="false"
           redirectPort="443"
~~~~
 
Then open **web.xml** (same directory) and add this snippet before the closing tag of /web-app:

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
 
Restart Tomcat and all pages should redirect to https.