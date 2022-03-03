## **Kubernetes**
----------
`kubectl get service` or `kubectl get svc` for kubernetes service information
`kubectl` for official documentation
`kubectl explain svc` for kind and version information
`kubectl get pods`
`kubectl get pvc`
`kubectl get all`
`kubectl delete pvc --all` && `kubectl delete pvc [name]`
`kubectl delete all --all`

### **What is Kubernetes?**
------------
Kubernetes is at its heart, what is known as a container orchestration tool.

It's responsible for allocating and scheduling containers, providing then with abstracted functionality like internal networking and file storage, and then monitoring the health of all of these elements and stepping in to repair or adjust them as necessary.

In short, it's all about abstracting how, when and where containers are run.

### **Benefits of Kubernetes?**
- Requires less resources
- **Self Healing** - A containerized application or component will automatically be redeployed in its intended state whenever a failure occurs.
- Automated rollouts and rollback
- Auto Scaling
- **Automatic bin packing** - Kubernetes places containers automatically based on the required resources and other restrictions without impairing availability. Automated rollouts and rollbacks: Kubernetes distributes changes to the software or the configuration using a rollout.
- **Storage orchestration** - Kubernetes allows users to automatically mount a storage system of choice, such as local storage, public cloud providers, and more. You still need to provide the underlying storage system.
- Keeps a check on the health of the nodes and containers.
- Horizontal scaling: scales application up and down quickly either manually or automatically

### **Use cases of Kubernetes**

![image](https://user-images.githubusercontent.com/98178943/156406416-05020796-465c-49e3-9275-c41afafb3e0e.png)

- **Kubernetes and Amazon EC2**: Kubernetes manage clusters for Amazon EC2, compute instances as well as run containers on these instances using processes of deployment, maintenance, and scaling.
- **Pokemon GO and Kubernetes**: They soon realized that the servers could not handle the amount of traffic they received. Then the application was run on GKE, which had the ability to orchestrate their container cluster at a manageable scale.

This enabled the team to focus on deploying live changes for their users. GKE helped the company to not only serve, by allowing Niantic to improve their user’s experience but also add newer features to the application.

### **How does Kubernetes benefit the business?**
- Achieves portability across on-prem and public clouds.
- Accelerates containerised application development, and unifis development and operational teams on a single platform.
- Deploy live changes for users
- Multi-cloud capability
- Increased developer productivity

### **Kubernetes rollback - how to use it?**
In Kubernetes, rolling updates are the default strategy to update the running version of your app.

In a nutshell, you deploy a newer version of your app and Kubernetes makes sure that the rollout happens without disrupting the live traffic.

However, even if you use techniques such as Rolling updates, there's still risk that your application doesn't work the way you expect it at the end of the deployment.

Perhaps it has a bug, or it wasn't configured correctly, and it keeps crashing.

**When you introduce a change that breaks production, you should have a plan to roll back that change.**

Kubernetes has a built-in mechanism for rollbacks.

Kubernetes and `kubectl` offer a simple mechanism to roll back changes to resources such as Deployments, StatefulSets and DaemonSets.

Kubernetes keeps a rolling revision history for pods, so you can roll back to any stored revision (the number of revisions is configurable with a default value of 10).

### **Kubernetes components**
---------------
![asffa0](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

- When Kubernetes us deployed, it creates a cluster. A cluster contains machines called nodes that host containerised applications. 

- These nodes host pods. 
- The control plane oversees nodes and pods in each cluster. 
- To break it down fully..

| Component      | Description |
| ----------- | ----------- |
| Nodes       | node is a virtual or physical machine that runs workloads. Each node contains the services necessary to run pods:         |
| Kubelet   | An agent that runs on each node in a cluster. It ensures that the containers running in a pod are in a healthy state.        |
| Pods        |A pod consists of containers and supplies shared storage, network resources and specifies how the containers will run. 
| Control Plane | The Control Plane is responsible for maintaining the desired end state of the Kubernetes cluster as defined by the developer.
|K-proxy     | A network proxy service that runs on each node in a given cluster. It maintains network rules on nodes that allow network communications to pods from within or outside the cluster.

### Creating pods
------------------
In order to test Nginx we create two yml files, one to create the pods with the pulled image and another to map our localhost to port 80 using loadbalancers.

This yml file creates a deployment for our nginx image and creates 3 pods.

```yml
---

apiVersion: apps/v1 # which api call we need to make for k8 to make a deployment for us
kind: Deployment # pod, service # replica set # ASG
metadata: # metadata to name your deployment - case insensitive
  name: nginx-deployment

spec: # labels and selectors are communication services between micro-services
  selector:
    matchLabels:
      app: nginx # look for this label to match with k8s service

  replicas: 3

# template to use it's label for k8s to launch in the browser
  template:
    metadata:
      labels:
        app: nginx

# define the container specs
    spec:
      containers:
      - name: nginx
        image: yfpc/103anginx   # ahskhan/sre_nginx_test
        ports:
        - containerPort: 80

        imagePullPolicy: Always
```

Map our localhost to port 80 using loadbalancers:

```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-deployment
  namespace: default
  
  
spec:
  ports:
  - nodePort: 30442 # range is 30000-32768
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx # this label connects this service to deployment
    # session affinity - your load balancer directs all requests from a particular end user to a specific origin server.
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
      - hostname: localhost  
```

**nodePort** - A NodePort service is the most primitive way to get external traffic directly to your service. NodePort, as the name implies, opens a specific port on all the Nodes (the VMs), and any traffic that is sent to this port is forwarded to the service.

**Load balancer** - A LoadBalancer service is the standard way to expose a service to the internet. On GKE, this will spin up a Network Load Balancer that will give you a single IP address that will forward all traffic to your service.

## Deploying our app using Kubernetes

Node deployment

```yml
---
apiVersion: apps/v1 # which api call we need to make for k8 to make a deployment for us
kind: Deployment #tool for running local Kubernetes clusters using Docker container “nodes”
metadata: # metadata to name your deployment - case insensitive
  name: node-deployment
spec: # labels and selectors are communication services between micro-services
  selector:
    matchLabels:
      app: node # look for this label to match with k8s service
  
  replicas: 3 # how many pods
  template: # template to use it's label for k8s to launch in the browser
    metadata:
      labels:
        app: node 
    spec:
      containers:
        - name: node
          image: yfpc/yacob_docker_app:v3
          ports: 
            - containerPort: 3000
          env:
            - name: DB_HOST
              value: mongodb://mongo:27017/posts
          imagePullPolicy: IfNotPresent    

---
apiVersion: v1
kind: Service
metadata:
  name: node
spec:
  selector:
    app: node
  ports:
    - port: 3000
      targetPort: 3000
  type: LoadBalancer     
```

Mongodb deployment and persistent volume claim

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  selector:
    matchLabels:
      app: mongo
  replicas: 1
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - name: mongo
          image: mongo
          ports:
            - containerPort: 27017
          volumeMounts:
            - name: storage
              mountPath: /data/db
      volumes:
        - name: storage
          persistentVolumeClaim:
            claimName: mongo-db # claims persistent volume
# when restarting pod, all saved data is gone. Persistent Volume ensures that the saved data is still there even if pod is gone. New pod will read existing data from
# that storage to get up to date data.
---
apiVersion: v1
kind: PersistentVolumeClaim # application has to claim persistent volume
metadata:
  name: mongo-db
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 256Mi
---
apiVersion: v1
kind: Service
metadata:
  name: mongo
spec:
  selector:
    app: mongo
  ports:
    - port: 27017
      targetPort: 27017
```

### **Volumes**
---------------
when restarting pod, all saved data is gone. Persistent Volume ensures that the saved data is still there even if pod is gone. New pod will read the existing data from that storage to get up to date data.