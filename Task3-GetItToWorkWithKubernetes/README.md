## Task 3 - Get it to work with Kubernetes

Next step is completely separate from step 2. Go back to the application you built in Stage 1 and get it to work with Kubernetes.

Separate out the two containers into separate pods, communicate between the two containers, add a load balancer (or equivalent), expose the final App over port 80 to the final user (and any other tertiary tasks you might need to do)

Add all the deployments, services and volume (if any) yaml files in the repo.

The only hard-requirement is to get the app to work with `minikube`

---

## Task 3 - Documentation

K8s Files directory

```
resources
│   
└─── common
│   │  namespace.yaml
│   │  ingress.yaml
└─── api
│   │   deployment.yaml
│   │   service.yaml
└─── sys-stats
│   │   deployment.yaml
│   │   service.yaml
```


Order of deployment and description

1. Namespace 
    - `kubectl apply -f resources/common/namespace.yaml`
    - Defines a new namespace to be used for this project in the cluster.

2. API
    - `kubectl apply -f resources/api/.`
    - Defines a deployment and service for the API python application.
        - The deployment is set for a replica of 1 and with a rolling update strategy for orchastration while deploying new versions and in case the deployment is scaled for more then 1 replica.
        - The container has been set with cpu and memory resorces and limits for best practice in order to have better control on the requirements of the nodes on which the cluster is hosted.
livelinessProbe and readiness prob have been set on the port exposed by the application.

        - A service configuration is deployed to expose the container to the cluster.
Internal routing can be done from one application to the other using the following name convention `<service name>.<namespace>.svc.cluster.local`

3. Sys-Stats
    - `kubectl apply -f resources/api/.`
    - Same as API

4. Ingress controller 
    - `kubectl apply -f resources/api/.`
    - An ingress controller is defined for routing and exposing services accordingly. This is done by pathprefix selection.

---

### Using minikube

- `minikube start --vm=true --driver=hyperkit` (had issue with default docker driver on mac os)

- `minikube addons enable ingress` (to install and enalbe nginx ingress controller in minikube). Reference: https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/

In order to get image from local machine for deployments the following command is needed to set the minikube docker-env
- `eval $(minikube docker-env)`
- The deployments need to have `imagePullPolicy: Never` defined. Reference: https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-with-minikube

Then build each image accordingly from their respective directory dockerfile
- `docker build . -t api-image`
- `docker build . -t sys-stats-image` ( The App.js had to be ammended to fetch stats from http://local-link.info/stats)

Using the command `minikube ip` the ip assigned to minkikube service is noted and this is added to the hostfile (/etc/hosts) for the local machine to be able to resolve the internal URL (http://local-link.info/)

The url defined for this project is http://local-link.info/