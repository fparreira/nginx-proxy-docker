# Configuring Docker Proxy Reverse Using Nginx

## The Problem 
The sevices containers are called by the ports of the host. In some situations this is bad.  
eg.  
<http://192.168.1.10:3000>  
<http://192.168.1.10:8081>

## The Solution 
A basic solution is configuring a Nginx container as Reverse Proxy.  
eg.  
<http://app1.domain.local>  
<http://storeadmin.domain.local>  

It will be create three container. The first is the nginx reverse proxy. The others will be the services.

## Reverse Proxy

First create the conf file to respond requests.  

#### reverse-proxy.conf  
```
server {
  listen 80;
  listen [::]:80;

  server_name app1.domain.local;

  location / {
      proxy_pass http://192.168.1.10:3000/;
  }
}

server {
  listen 80;
  listen [::]:80;

  server_name storeadmin.domain.local;

  location / {
      proxy_pass http://192.168.1.10:8081/;
  }
}
```

#### Dockerfile