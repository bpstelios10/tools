# Tools

## Deploy locally (docker)
### Prometheus
First you need to build the image, by running this command in the folder that the Dockerfile is:
```bash
docker build .
```
If successful, the message is `Successfully built {image-id}`.
Next step is to spin up prometheus and publish it to some port, like 9090:
```bash
docker run --add-host=host.docker.internal:host-gateway -p 9090:9090 {image-id} --config.file=/etc/prometheus/prometheus-docker.yml
```

### Grafana
First you need to build the image, by running this command in the folder that the Dockerfile is:
```bash
docker build .
```
If successful, the message is `Successfully built {image-id}`.
Next step is to spin up grafana and publish it to some port, like 3000:
```bash
docker run --add-host=host.docker.internal:host-gateway -p 3000:3000 {image-id}
```

## K8s deployments
For k8s deployments, helm is being used. 
For local tests use minikube: `minikube start -p {some-context-name} --memory 8000 --alsologtostderr --vm-driver=virtualbox`

### K8s Infrastructure
In helm, first we need to create the namespaces etc.
For the namespaces to be created we need to install the service in k8s: `helm install monitoring-infrastructure helm/monitoring-infrastructure`
And every time a change is applied, we need to upgrade the `version` in Chart.yaml and run: `helm upgrade monitoring-infrastructure helm/monitoring-infrastructure`

### Prometheus deployment
Create and upgrade prometheus and service by using gradle tasks (not all modules are relevant to every env): 
```bash
  ./gradlew {module}:deployToDev
```
(The version in chart.yaml and values-*.yaml need to be updated)

### Grafana deployment
Create and upgrade grafana and service by using gradle tasks (not all modules are relevant to every env): 
```bash
  ./gradlew {module}:deployToDev
```
(The version in chart.yaml and values-*.yaml need to be updated)
