# Kubernetes

### The Most Common Commands

_To make your life easier while running these commands you can create an alias to the kubectl command:_

        $ alias k="kubectl"

_You can use any letter other than k to be the alias._

- To get all the nodes in the cluster:

        $ kubectl get nodes

- To get all the namespaces in the cluster:

        $ kubectl get namespaces

- To get all the pods in the default namespace:

        $ kubectl get pods

- To get all the pods in a specific namespace (for example the namespace is kub-system):

        $ kubectl get pods --namespace=kub-system

- To create a new pod (in the default namespace):

        # kubectl run [pod name] --image=[image name]
        $ kubectl run nginx --image=nginx

- To get the pod's information:

        # kubectl describe pod [pod name]
        $ kubectl describe pod nginx

_Note: If you want to get all the containers in a node, you have to connect to the node using ssh protocol, then you can use the Docker commands that we learned in the previous module_

_Note: If you use Docker as a container runtime therefore in each pod there will be a default container created called Pause container. The main functionality of this container is to keep the namespace of the pod._

- To delete a pod:

        # kubectl delete pod [pod name]
        $ kubectl delete pod nginx

- To create a deployment:

        # kubectl create deployment [deployment name] --image=[image name]
        $ kubectl create deployment ngnix-deployment --image=nginx

- To get all deployments:

        $ kubectl get deployments

- To get the deployment's information:

        # kubectl describe deployment [deployment name]
        $ kubectl describe deployment nginx-deployment

- To scale up or down (if the exist replicas more than the assigned number in the command then the replicas will be scalled down to the assigned number and vice versa) the number of the replicas in the deployment (Notice that each one of the the new replicas has different IP address):

        # kubectl scale deployment [deployment name] --replicas=[#]
        $ kubectl scale deployment nginx-deployment --replicas=7

- To create a service to a deployment (will assign a cluster IP address to the deployment and it's not accessable outside the kuberenetes cluster):

        # kubectl expose deployment [deployment name] --port=[the external IP] --target=[port inside the container]
        $ kubectl expose deployment nginx-deployment --port=8080 --target-port=80

_To access a service you can use: curl IP:Port_

- To get all the services:

        # You can use delete, describe commands with services too!
        $ kubectl get services
