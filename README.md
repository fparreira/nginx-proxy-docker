# Configuring Docker Proxy Reverse Using Nginx

## The Problem 
The sevices containers are called by the ports of the host. In some situations this is bad.  
eg.  
<http://192.168.1.10:3000>  
<http://192.168.1.10:9000>  
<http://192.168.1.10:8081>

## The Solution 
A basic solution is configuring a Nginx container as Reverse Proxy.  
eg.  
<http://app1.domain.local>  
<http://app2.domain.local>  
<http://storeadmin.domain.local>  