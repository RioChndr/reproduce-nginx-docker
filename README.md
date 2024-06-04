# Nginx docker 504 Gateway Time-out

Why this nginx do not know his neighbour ?, nginx give error 504 gateway time-out. It means nginx do not know the proxy or the server are not running yet, but it shoudl be run. My guest is `host.docker.internal`.

![Screenshot](./ss1.png)

# Fixed

Nevermind, Fixed here https://github.com/RioChndr/reproduce-nginx-docker/commit/106436c

```diff
     location / {
-      proxy_pass http://host.docker.internal:8080;
+      proxy_pass http://backend:8080;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
     }
```

It work by change the host into docker service's name. The port are required, docker-compose backend also do not have to expose his PORT into host, nginx and backend are in the same compose, they handle the hostname out-of-the-box. Nice one.