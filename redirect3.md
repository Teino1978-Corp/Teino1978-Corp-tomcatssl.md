## How to configure Tomcat to always require HTTPS
[Source](http://www.itworld.com/development/79351/how-configure-tomcat-always-require-https)

**First**, make sure that you have configured and enabled both the HTTP and HTTPS elements in your conf/server.xml file:

~~~~
    <Connector port="8080" protocol="HTTP/1.1" 
               redirectPort="443"/>
~~~~

~~~~
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
               scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS"
               keystoreFile="conf/keystore" keystorePass="s00perSeeekrit"/>
~~~~

For details on how to prepare your conf/keystore file, see http://tomcat.apache.org/tomcat-6.0-doc/ssl-howto.html

**Then**, Restart Tomcat and test both of these connectors, making sure that you can access your web application via either connector before you proceed. Next, edit your webapp's WEB-INF/web.xml file and add the following inside of your container element:

~~~~
    <!-- Require HTTPS for everything except /img (favicon) and /css. -->
    <security-constraint>
        <web-resource-collection>
            <web-resource-name>HTTPSOnly</web-resource-name>
            <url-pattern>/*</url-pattern>
        </web-resource-collection>
        <user-data-constraint>
            <transport-guarantee>CONFIDENTIAL</transport-guarantee>
        </user-data-constraint>
    </security-constraint>
    <security-constraint>
        <web-resource-collection>
            <web-resource-name>HTTPSOrHTTP</web-resource-name>
            <url-pattern>*.ico</url-pattern>
            <url-pattern>/img/*</url-pattern>
            <url-pattern>/css/*</url-pattern>
        </web-resource-collection>
        <user-data-constraint>
            <transport-guarantee>NONE</transport-guarantee>
        </user-data-constraint>
    </security-constraint>
~~~~

This configuration declares that the entire webapp is meant to be HTTPS only, and the container should intercept HTTP requests for it and redirect those to the equivalent "https://" URL. The exception is certain requests having URL patterns that match the "HTTPSOrHTTP" web resource collection, in which case the requests will be served through the protocol the request came in on, either HTTP or HTTPS.

**Lastly**, restart your webapp (or Tomcat). It should now redirect HTTP requests to HTTPS, and it should serve the webapp via HTTPS only.