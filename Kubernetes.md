# Kubernetes

### The Most Common Commands

- In the master node & worker node components:

        Master
        - etcd cluster (strores info about everything about pods ...)
        - kube controller manager (controller is a process that continuously monitors the state of various components within the system)
        - kube-scheduler (is only responsible for deciding which pod goes on which node. It doesn’t actually place the pod on the nodes.)
        - kube-apiserver (primary management component)

        Worker
        - kublete (You must always manually install the kubelet on your worker nodes. its doesn't auto with kubeadm)
        - kube-proxy (is a process that runs on each node in the kubernetes cluster.Its job is to look for new services and every time a new service is created it creates the appropriate rules on each node to forward traffic to those services)

- Etcd is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. It gracefully handles leader elections during network partitions and can tolerate machine failure, even in the leader node.

- If you use kubeadm then etcd server will be a pod in the system namespace, otherwise you have to downloaded and set it on the cluster.

- To run etcd service (on port 2379 as a default) run the follwing aommand:

        $etcd
        # To add a value to ectd
        $etcdctl put [key1] [value1]
        # to get stored data
        $etcdctl get [key1]

- To use the lastest release of etcd api run the following command:

        ETCDCTL_API=3 ./etcd version

_To make your life easier while running these commands you can create an alias to the kubectl command:_

        $ alias k="kubectl"

_You can use any letter other than k to be the alias._

*Also namespace==ns & service==svc*

- To get all the nodes in the cluster:

        $ kubectl get nodes
        # For more details
        $ kubectl get nodes -o wide

- As a default pods and all the other components are created in the default namespace. However if you are looking to create a separate space with other authority levels and defined rnge of the resources then you have to create a new namespace. If you want to get pods or do any thing the new namespace then use the following:

        $ kubectl get pods --namespace=[name]
        # To switch to the new space
        $ kubectl config set-context $(kubectl config current-context) --namespace=[name]
        # To get pods in all namespaces
        $ kubectl get pods --all-namespaces

Also, when you try to create a component using yaml files then you have to add a namespace attribute in the metadeta field.

- To limit the access of a namespace to resources use ResourceQuota; it's created using YAML files as any other component.

- To get all the namespaces in the cluster:

        $ kubectl get namespaces

- To get all the pods in the default namespace:

        $ kubectl get pods
        $ kubectl get pods -o wide

- To get all the pods in a specific namespace (for example the namespace is kub-system):

        $ kubectl get pods --namespace=kub-system
        
- To get pods in all name spaces:

        # kubectl get pods --all-namespaces
        
- To create a new namespace:

        $ kubectl create ns [name]

- To create a new pod (in the default namespace):

        # You can add --dry-run -o yaml to console log the yaml file that will create the pod. You can add > filename.yaml to save the output in the file.
        # kubectl run [pod name] --image=[image name] --dry-run=client -o yaml > [filename]
        $ kubectl run nginx --image=nginx
        # To add a label to the pod
        $ kubectl run [pod name] --image=[image name] --labels="app=love"

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
                        - name: nginx-container
                          image: nginx
                          ports:
                                - containerPort: 5432
                          # If the pod contains a DB service.
                          env:
                                - name: POSTGRES_USER
                                  value: postgres
                                - name: POSTGRES_PASSWORD
                                  value: posgtgres


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

1- NodePort service: it help in mapping the port on the node (called nodePort) to a port on the pod (called targetPort) The service and pods have IP addresses, the IP address of the service called the cluster IP address of the service. Without services users can't access the pods content.
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

2- ClusterIP: provides an interface to access a group of pods like backend pods, redis pods, and frontend pods. To create a a clusterIP follwo the structure of the following file:

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

3- LoadBalancer: used to provide a single IP address to user where the app is deployed on multiple pods and each has different IP address. This service is not offered in all environements, it's just on some cloud services provided by some of the vendors like Google and AWS (tacks the place of nodePort in those env.'s).To create a a LoadBalancer follwo the structure of the following file:

                ```
                apiVersion: v1
                # The kind of the component that you want to create
                kind: Service
                metadata:
                        name: first-load-balancer-svc
                spec:
                  type: LoadBalancer
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
        # To create the sevice to a pod
        $ kubectl expose pod [redis] --port=6379 --name [redis-service]

_To access a service you can use: curl IP:Port_

- To get all the services:

        # You can use delete, describe commands with services too!
        $ kubectl get services

- Kubeadm is a tool used to set up multinode cluster easly.
- To get the logs of a pod:

        $kubectl logs [pod name]
        
- To restrict the pods that can run on a node thent ou have to add a Tain to the node and give a tolerance to the pods that can run on it (Notice that taint and toleration do not guarantee hat a specific pod will placed in a specific node, where it can be placed in another untainted node):

        $kubectl taint nodes [node name] [key]=[value]: [one of theses values: NoSchedual | PreferNoSchedual | NoExecute (most restrict)]
        # To untaint the node add - to the end of he previous statement
        
- To add taint to the YAML file of a pod: 

        spec:
                tolerations:
                - key: "app"
                  operator: "Equal"
                  value: "blue"
                  effect: NoSchedual

- To label a node use the following command:

        $kubectl label nodes [node name] [key]=[value]
        
- To select a specific node for a pod; add the following section to the pod definition file:

        spec:
                ...
                nodeSelector:
                        size:large
  
- The primary purpose of node affinity feature is to ensure that pods are hosted on particular nodes. Affinity provides more flexibility on the chosen node. To select a range of nodes to run the pod (Notice that the affinity does not guarantee that other nodes will not be added to the node (opposite to the taint)):

        spec:
          affinity:
            nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: disktype
                    operator: In
                    values:
                    - ssd

- To limit pod resorces:

        apiVersion: v1
        kind: Pod
        metadata:
          name: frontend
        spec:
          containers:
          - name: app
            image: images.my-company.example/app:v4
            resources:
              requests:
                memory: "64Mi"
                cpu: "250m"
              limits:
                memory: "128Mi"
                cpu: "500m"
          - name: log-aggregator
            image: images.my-company.example/log-aggregator:v6
            resources:
              requests:
                memory: "64Mi"
                cpu: "250m"
              limits:
                memory: "128Mi"
                cpu: "500m"

        - You can ceate default limit range for pods resource by creating  a limi trange file

                apiVersion: v1
                kind: LimitRange
                metadata:
                  name: mem-limit-range
                spec:
                  limits:
                  - default:
                      memory: 512Mi
                    defaultRequest:
                      memory: 256Mi
                    type: Container

- Daemon set are like replica sets, as in it helps you deploy multiple instances of pod. But it runs one copy of your pod on each node in your cluster. Whenever a new node is added to the cluster a replica of the pod is automatically added to that node, it is the perfect choice to apply a monitoring agent on nodes. Creating a DaemonSet is similar to create a replicaset, the only difference is the kind.

- Static pod: creating a pod without te precense of master node and its component, just using kubelet. In the definition of static pods you'll get the owner as node instead of replicaset. Also, you will get the name of the node added to the end of the pod name (podName-nodeName). Notice that you can not update static pods or delete them using kubectl commands instead you need to get inside the node an d delete thier definitions files. To define  he location where the yaml files of the pods to be saved you need to add the path to kubelet.service file in the ``--pod-manifest-path= [the path]` field. or add `--config=kubeconfig.yaml` and then add the path to the kubeconfig.yaml file as following: `staticPodPath: [the path]` You need to use the docker commands to list te static pods eventhough they shows with others if the master exist.

- To delete a static pod:

        1. Connect to its node using ssh
        2. Go to /var/lib/kubelete/config.yaml
        3. Take the static pod path
        4. use the path to delete the file
        5. use get po --watch to see how its die, it deserves to die!

- To replace a pod use the following command:

        $kubectl replace --force -f [file.yaml]


- To get nodes with a specific selector:

        $kubectl get pods --selector [var.]=[val.] --no-headers
        $kubectl get all --selector [var.]=[val.] --no-headers
        
- To extract the pod definition in YAML format to a file using the command
        
        # Note that you can't to edit pods without recreating htem.
        $kubectl get pod webapp -o yaml > my-new-pod.yaml
        
- The status OOMKilled indicates that it is failing because the pod ran out of memory.

- To add a command to run before starting the pod (put it the last option where all the things next to it are cosidedred as commands ): 

        $kubectl run [name] --image=[image name] --dry-run=client -o yaml --command -- sleep 1000 > [file path]l
