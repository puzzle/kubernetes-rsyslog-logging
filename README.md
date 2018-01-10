# kubernetes-rsyslog-logging
Centralized container logs with rsyslog server and Linux logger.

## purpose
Provide kubernetes configuration to establish a centralized logging solution. Tested with minikube and kubernetes 1.8

* rsyslog server that takes logs and writes them to a log file for each host that sends logs.
* example for a Docker container with overwritten command that sends it's log to the rsyslog server.

## setup
Start [minikube](https://github.com/kubernetes/minikube)
```
minikube start
```

### server
Deploy rsyslog server with service.
```
kubectl create -f rsyslog-server-deployment.yaml
kubectl create -f rsyslog-server-service.yaml
```

Log files will be written to */var/log*

Rsyslog container is pulled from [jumanjiman/rsyslog](https://hub.docker.com/r/jumanjiman/rsyslog/).
Which is build on this base: https://github.com/jumanjihouse/docker-rsyslog

#### logging configuration
Rsyslog configuration is overwritten by a config map.
See the server deployment: [rsyslog-server-deployment.yaml](./rsyslog-server-deployment.yaml)

### example application
Random logging container.

Configuration shows how to extend the run command. Original command from the Dockerfile is ```/entrypoint.sh```

Command with logging: ```/entrypoint.sh  2>&1 | logger -s 2>&1```

* Collect *stdout* and *stderr* from entrypoint command and pipe them to the logger.
* Logger writes to syslog and also to *stdout* becaus of the *-s* flag.
* Additional, the rsyslog server has to be configured.
    * See the container deployment for the final solution: [rsyslog-example-app.yaml](./rsyslog-example-app.yaml)

Important: Create example application after the service.

```
kubectl create -f rsyslog-example-app.yaml
```
