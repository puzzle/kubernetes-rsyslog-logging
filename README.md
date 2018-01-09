# kubernetes-rsyslog-logging
Centralized container logs with rsyslog server and Linux logger.

## purpose
Provide kubernetes configuration to establish a centralized logging solution. Tested on minikube an kubernetes 1.8

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
kubectl create -f rsyslog/rsyslog-server-deployment.yaml
kubectl create -f rsyslog/rsyslog-server-service.yaml
```

### example application
Create example application after the service.

```
kubectl create -f rsyslog/rsyslog-example-app.yaml
```
