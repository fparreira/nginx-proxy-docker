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

First let's create the conf file to respond requests.  

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
This is the basic configuration. There are others specifications for each structure.  
Now, we'll create the nginx dockerfile. The two files must be at the same directory.

#### Dockerfile
```
FROM nginx:latest  
copy reverse-proxy.conf /etc/nginx/conf.d/reverse-proxy.conf
```

Creating the Nginx Reverse Proxy Image and running the nginx service container.
```
docker build -t nginx-reverse-proxy .
```

```
docker run -d --name my-reverse-proxy -p 80:80 nginx-reverse-proxy
```

## Services Containers  

Now let's create the two containers that will run example services.

```
docker run -d --name app1-service -p 3000:80 httpd
```
```
docker run -d --name storeadmin-service -p 8081:80 nginx
```  

In this moment you can call the services on browser using the name  
**app1.domain.local**  
**storeadmin.domain.local**
  
  
---
**note** Some sceneries is need configuring the DNS.  
eg. In linux local server configure the /etc/hosts adding:  

```
192.168.1.10    domain.local
192.168.1.10    app1.domain.local
192.168.1.10    storeadmin.domain.local
```