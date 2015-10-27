# docker-nginx-http2
Docker image for nginx 1.9.6 with openssl 1.0.2 and http2 module support

## What's inside
 - Nginx 1.9.6 build from source (works exactly like official nginx image)
 - Openssl 1.0.2 build from source and pre-installed
 - Nginx http_v2_module
 - Nginx http_stub_status_module
 - Nginx http_realip_module

 Configure arguments shows below:
```
./configure \
    --prefix=/usr/local/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --pid-path=/var/run/nginx.pid \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --with-http_ssl_module \
    --with-http_v2_module \
    --with-openssl=/usr \
    --with-http_realip_module \
    --with-http_stub_status_module \
    --with-threads \
    --with-ipv6 \
```

## Default Path
 - Nginx conf: `/etc/nginx/nginx.conf`
 - Pid file: `/var/run/nginx.pid`
 - Bin path: `/usr/sbin/nginx`
 - Access log: `/var/log/nginx/access.log`
 - Error log: `/var/log/nginx/error.log`

## How to use this image
`$ docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d ehekatl/nginx-http2`
Alternatively, a simple Dockerfile can be used to generate a new image that includes the necessary content (which is a much cleaner solution than the bind mount above):
```
FROM ehekatl/nginx-http2
COPY static-html-directory /usr/share/nginx/html
```
Place this file in the same directory as your directory of content ("static-html-directory"), run `docker build -t some-content-nginx` ., then start your container:

`$ docker run --name some-nginx -d some-content-nginx`
exposing the port
`$ docker run --name some-nginx -d -p 8080:80 some-content-nginx`
Then you can hit http://localhost:8080 or http://host-ip:8080 in your browser.

## Complex configuration
`$ docker run --name some-nginx -v /some/nginx.conf:/etc/nginx/nginx.conf:ro -d ehekatl/nginx-http2`
For information on the syntax of the Nginx configuration files, see the official documentation (specifically the Beginner's Guide).

Be sure to include daemon off; in your custom configuration to ensure that Nginx stays in the foreground so that Docker can track the process properly (otherwise your container will stop immediately after starting)!

If you wish to adapt the default configuration, use something like the following to copy it from a running Nginx container:

`$ docker cp some-nginx:/etc/nginx/nginx.conf /some/nginx.conf`
As above, this can also be accomplished more cleanly using a simple Dockerfile:
```
FROM ehekatl/nginx-http2
COPY nginx.conf /etc/nginx/nginx.conf
```
Then, build with docker build -t some-custom-nginx . and run:

`$ docker run --name some-nginx -d some-custom-nginx`
