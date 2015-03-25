# ngx_upstream_servers
Sometimes,command "server" is not easy to manage cluster,this is an useful patch.

CN:

在日常运维时，后端经常需要下线某台服务器，而一台机器上部署的是多个服务，由端口区分，多台机器

镜像，由upstream来负载均衡。这样需要在配置的多个地方进行删除或者修改。这个补丁就是为了简化这种

运维工作的。


---------------------------------------------------------------------------------
examlpe:


upstream app1_cluster{

    ip_hash;
    
    server  192.168.20.20:2000;
    
    server  192.168.20.21:2000;
    
    server  192.168.20.22:2000;
    
    ...
    
}

upstream app2_cluster{

    ip_hash;
    
    server  192.168.20.20:2001;
    
    server  192.168.20.21:2001;
    
    server  192.168.20.22:2001;
    
    ...
    
}

...

new config:

upstream app_cluster{

  server  192.168.20.20;
  
  server  192.168.20.21;
  
  server  192.168.20.22;
  
  ...
  
}

upstream app1_cluster{

    ip_hash;
    
    port 2001;
    
    servers app_cluster;
    
}

upstream app2_cluster{

    ip_hash;
    
    port 2002;
    
    servers app_cluster;
    
}

...

------------------------------------------------------------------------------




