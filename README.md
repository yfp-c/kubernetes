## **Kubernetes**
----------
`kubectl get service` or `kubectl get svc` for kubernetes service information
`kubectl` for official documentation
`kubectl explain svc` for kind and version information

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