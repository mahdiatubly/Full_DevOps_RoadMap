# Kubernetes

### The Most Common Commands

_To make your life easier while running these commands you can create an alias to the kubectl command:_

        $ alias k="kubectl"

_You can use any letter other than k to be the alias._

- To get all the nodes in the cluster:

        $ kubectl get nodes
        # For more details
        $ kubectl get nodes -o wide

- To get all the namespaces in the cluster:

        $ kubectl get namespaces

- To get all the pods in the default namespace:

        $ kubectl get pods

- To get all the pods in a specific namespace (for example the namespace is kub-system):

        $ kubectl get pods --namespace=kub-system

- To create a new pod (in the default namespace):

        # You can add --dry-run -o yaml to console log the yaml file that will create the pod. You can add > filename.yaml to save the output in the file.
        # kubectl run [pod name] --image=[image name] --dry-run -o
        $ kubectl run nginx --image=nginx

- To create a pod using YAML file:

```
        # the version of Kubernrtes api
        apiVersion: v1
        # The kind of the component that you want to create
        kind: Pod
        metadata:
                name: first-pod
                #You can add more labels
                labels:
                        app: first-app
                        type: front-end
        # The specifications of the element
        spec:
                containers:
                        -name: nginx-container
                        - image: nginx
```

Then run:

                # You can use "apply" insted of "create"
                $ kubectl create -f file-name.yml

- To get the pod's information:

        # kubectl describe pod [pod name]
        $ kubectl describe pod nginx

_Note: If you want to get all the containers in a node, you have to connect to the node using ssh protocol, then you can use the Docker commands that we learned in the previous module_

_Note: If you use Docker as a container runtime therefore in each pod there will be a default container created called Pause container. The main functionality of this container is to keep the namespace of the pod._

- To edit the pod configuration:

        # kubectl edit pod [pod name]
        $ kubectl edit pod nginx

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

- Replication Controllers is the old approch that used by kubernetes to scale up and monitor pods (in v1). ReplicaSets is new technology in kuberenetes to manage pods and scaling them up and down (valid in app/v1). One of the key differences between them that the ReplicaSets offers selection element to connect created pods to the scalling and monitoring system.

- The structure of the ReplicaSets YAML file that used to create ReplicaSets besides the commands mentioned in the next point is as following:

```
        # the version of Kubernrtes api
        apiVersion: apps/v1
        # The kind of the component that you want to create
        kind: ReplicaSet
        metadata:
                name: first-replicaset
                #You can add more labels
                labels:
                        app: first-app
                        type: frontend
        spec:
                # The template the should be used to create replicas.
                template:
                        metadata:
                                name: first-pod
                                #You can add more labels
                                labels:
                                        app: first-app
                                        type: frontend
                        # The specifications of the element
                        spec:
                                containers:
                                        - name: nginx-container
                                          image: nginx
                # The number of replicas
                replicas: 3
                # Used to define the number of the exist replicas.
                selector:
                        matchLabels:
                                type: frontend
```

Then run the following command:

                $ kubectl create -f file-name.yml

To change the number of replicas you can update the file and then run:

                $ kubectl replace -f file-name.yaml
                # Or you can use the following command without updating the file
                $ kubectl scale --replicas=6 -f file-name.yaml
                # Or you can use edit command: $ kubectl edit replicaset [replicaset name]
                $ kubectl edit replicaset first-replicaset

To get help in defining a problem in the YAML file:

                $ kubectl explain replicaset

- To scale up or down (if the exist replicas more than the assigned number in the command then the replicas will be scalled down to the assigned number and vice versa) the number of the replicas in the deployment (Notice that each one of the the new replicas has different IP address):

                # kubectl scale deployment [deployment name] --replicas=[#]
                $ kubectl scale deployment nginx-deployment --replicas=7

- To get all ReplicaSets:

                # rs = replicaset
                $ kubectl get replicaset

- On the top of replicaset there is the deployment which gives the capablility to apply rollback update and pause running changes and many other capabilities. Creating a new deployment using YAML file is exactly the same as creating replicaset except in the kind field you have to write Deployment instead of Replicast. Whenever you create a deployment a replicaset and pods will be automatically created. To get all the components that created with the deployment run the following command:

                $ kubectl get all

- To update a deployment, update the YAML file first, then, run the following command:

                # To track the cause of change you can use --record flag
                $ kubectl apply -f file-name.yaml [--record]
                # you can set a new image without updating the file using:
                $ kubectl set image deployment [deploymentName] [containerName]=[newImage]

- To get the stauts of the rollout process:

                $ kubectl rollout status [deploymmentName]
                $ kubectl rollout history [deploymmentName]

- To undo the updates on the deployment:

                $ kubectl rollout undo [deploymmentName]

- There are three types of services:

        - NodePort service: it help in mapping the port on the node (called nodePort) to a port on the pod (called targetPort) The service and pods have IP addresses, the IP address of the service called the cluster IP address of the service. Without services users can't access the pods content.
          To create a nodePort service using YAML file then follow the following structure:

          ```
          apiVersion: v1
          # The kind of the component that you want to create
          kind: Service
          metadata:
                  name: first-node-port-svc
          spec:
                  type: NodePort
                  ports:
                          - targetPort: 80
                            #The service port, it's optional, if not specified will be the same as the target port automatically.
                            port: 80
                            #NodePort is also optional and should be in the range of(30000-32767)0.If it's not assigned will be picked randomly from this range.
                            nodePort: 30008
                  selector:
                          #Pull the label attribute from the pod YAML file
                          app: firstApp
                          type: frontEnd

          ```

        Then run `create -f` command as with other type of kurbenetes components.

        - ClusterIP: provides an interface to access a group of pods like backend pods, redis pods, and frontend pods. To create a a clusterIP follwo the structure of the following file:

                ```
                apiVersion: v1
                # The kind of the component that you want to create
                kind: Service
                metadata:
                        name: first-clusterIP-svc
                spec:
                  type: CulusterIP
                  ports:
                          - targetPort: 80
                            #The service port, it's optional, if not specified will be the same as the target port automatically.
                            port: 80
                  selector:
                          #Pull the label attribute from the pod YAML file
                          app: firstApp
                          type: frontEnd
                ```

        Then run `create -f` command as with other type of kurbenetes components.

- To create a service to a deployment (will assign a cluster IP address to the deployment and it's not accessable outside the kuberenetes cluster):

        # kubectl expose deployment [deployment name] --port=[the external IP] --target=[port inside the container]
        $ kubectl expose deployment nginx-deployment --port=8080 --target-port=80

_To access a service you can use: curl IP:Port_

- To get all the services:

        # You can use delete, describe commands with services too!
        $ kubectl get services
