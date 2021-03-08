# Generator to add APIs capabilities to a Boot application on K8s

This generator uses standard ```kubectl``` resources and services to optimize a Spring Boot application deployed with Tanzu API grid components   

## Generator Commands

Use these commands in the ```commands``` section of the Generators UI and/or in the `acc generator` CLI.

`api-boot-k8s core` creates a ```kubernetes/core``` library with the following artifacts

- Spring Boot deployment and service
- Tanzu Observability configuration (via wavefront K8s proxy)  
- Optional: Spring Boot Observer Sidecar (needed Spring Boot Observer server to run separately)

`api-boot-k8s api` creates a ```kubernetes/api``` library with the following artifacts

- Tanzu Spring Cloud Gateway deployment 
- Tanzu API Hub deployment and service 

## Building and deploying the application

This generator relies on using the new `buildpack` support added in Spring Boot version 2.3 to create a container image.

If you are using Minikube you should configure your terminal to use the same Docker environment:

```bash
eval $(minikube docker-env)
```

**Step 1:** Build the project and create the container image:

```
./mvnw -DskipTests clean package spring-boot:build-image
```

**Step 2:** Deploy the app to Kubernetes

```
kubectl apply -f kubernetes/app
```

Use `kubectl get all` to verify that the resources created.

Accessing the application's endpoint varies based on the type of Kubernetes cluster you are using.  For example, if minikube is being used, look for the endpoint to access using the command `minikube service list`. For othetr Kubernetes clusters, you can use port-forwarding to access the service. Run `kubectl port-forward service/<name-of-service> 8080:80` to forward the service port to `localhost:8080`.

The README.md file from the code repository for the appliation should include a few `curl` commands to exercise the application.

# Generator Information

This generator creates the Kubernetes `service` and `deploymnet` resources files to deploy a Spring Cloud Gateway, API Hub and Spring Boot using `kubectl`

To use the generator you will need to install the following command line tool:

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)


## Generator installation

```
tss generator install --go-getter-url=github.com/dektlong/generator-boot-api-k8s
```

To use the install command you need to install [go-getter](https://github.com/hashicorp/go-getter#installation-and-usage) CLI.
