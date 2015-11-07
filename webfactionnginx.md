WebFaction SSL
--------------

Nginx is used for SSL termination so the content served to GlassFish
is not encrypted. While this is correct and transparent to
applications it seems GlassFish expects to always be the first server
in a stack.

However there may be a solution to this.  According to this blog post    
(http://stupidfredtricks.blogspot.co.uk/2012/10/glassfish-31x-behind-ssl-terminating.html)    
GlassFish can take advantage of headers set by nginx to determine the
"real" protocol.

We set the following headers in nginx when a request is sent over SSL:

~~~~
HTTPS: on
X-Forwarded-Proto: https
X-Forwarded-SSL: on
~~~~

If you can configure GlassFish to be aware of any of these you should
be able to get it working.

> Is there any way to skip Nginx front-end server
> altogether for a couple of ports managed by Glassfish?

This is possible, but isn't great. You'd need to switch your site to a
different IP address **and** redirect your site to your application's
port, like so:

https://admin.yourdomain.com:22715
