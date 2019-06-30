Inspired by  https://github.com/Tecnativa/docker-socket-proxy

Exposes docker.sock through filtered tcp endpoint.

Usage
Run the service (--privileged flag is required)

```
$ docker container run \
    -d --privileged \
    --name dockerproxy \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -p 127.0.0.1:2375:2375 \
    softasap/dck-socket
```
Connect your local docker client to that socket:

$ export DOCKER_HOST=tcp://localhost

Explore endpoints at
https://docs.docker.com/engine/api/v1.24/


```
frontend dockerfrontend
    bind :2375
    http-request deny unless METH_GET || { env(POST) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/containers/[a-zA-Z0-9_.-]+/((stop)|(restart)|(kill)) } { env(ALLOW_RESTARTS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/auth } { env(AUTH) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/build } { env(BUILD) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/commit } { env(COMMIT) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/configs } { env(CONFIGS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/containers } { env(CONTAINERS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/distribution } { env(DISTRIBUTION) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/events } { env(EVENTS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/exec } { env(EXEC) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/images } { env(IMAGES) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/info } { env(INFO) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/networks } { env(NETWORKS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/nodes } { env(NODES) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/_ping } { env(PING) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/plugins } { env(PLUGINS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/post } { env(POST) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/secrets } { env(SECRETS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/services } { env(SERVICES) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/session } { env(SESSION) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/swarm } { env(SWARM) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/system } { env(SYSTEM) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/tasks } { env(TASKS) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/version } { env(VERSION) -m bool }
    http-request allow if { path,url_dec -m reg -i ^(/v[\d\.]+)?/volumes } { env(VOLUMES) -m bool }
    http-request deny
    default_backend dockerbackend
```

