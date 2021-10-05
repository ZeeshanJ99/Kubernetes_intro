# Kubernetes_intro

![image](https://user-images.githubusercontent.com/88186084/136014968-df58e29a-4ae1-4075-9296-486d000fcf77.png)

--------------------------------------------

## What is Kubernetes (K8s)

![image](https://user-images.githubusercontent.com/88186084/135876559-8bee85a2-e97e-4469-b6b2-04a6ee80cbcd.png)

Kubernetes is an open-source container-orchestration system for automating computer application deployment, scaling, and management. It was originally designed by Google and is now maintained by the Cloud Native Computing Foundation.

Kubernetes is a open-source container orchestrator. It clusters together groups of hosts running containers, and helps you easily and efficiently manage those clusters.

--------------------------------------------

## Benefits of Kubernetes

- Self Healing
- Load Balancing and Service Dicov
- Automated rollouts and rollback
- Auto Scaling
- Automatic bin packing
- Storage orchestration
- Market leader - Kubernetes adoption within enterprise IT environments is rising and no longer just a developer community project. In a recent survey, 59% of respondents cited they were running Kubernetes in production.

--------------------------------------

## Kubernetes Architecture

![image](https://user-images.githubusercontent.com/88186084/135998784-faee5566-e5d0-4cf6-8de3-846a49345c49.png)


--------------------------------------------

## How to set it up

![image](https://user-images.githubusercontent.com/88186084/135873929-0abf28c6-e9b3-461b-af7a-7a28f5309cb0.png)

---------------------------------------------

## Docker architecture with nodeapp

![image](https://user-images.githubusercontent.com/88186084/136014552-a3073f36-b6b3-42fb-b031-afd51dd9d80d.png)


-------------------------------------------------

`kubectl` - brings up commands we can use in kubernetes
everything starts with kubectl

## Basic Commands (Beginner):

    create        Create a resource from a file or from stdin.
    expose        Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
    run           Run a particular image on the cluster
    set           Set specific features on objects
    
--------------------------------------------

## Basic Commands (Intermediate):
  
    explain       Documentation of resources
    get           Display one or many resources
    edit          Edit a resource on the server
    delete        Delete resources by filenames, stdin, resources and names, or by resources and label selector

--------------------------------------------

## Deploy Commands:
  
    rollout       Manage the rollout of a resource
    scale         Set a new size for a Deployment, ReplicaSet or Replication Controller
    autoscale     Auto-scale a Deployment, ReplicaSet, StatefulSet, or ReplicationController

--------------------------------------------

## Cluster Management Commands:
  
    certificate   Modify certificate resources.
    cluster-info  Display cluster info
    top           Display Resource (CPU/Memory) usage.
    cordon        Mark node as unschedulable
    uncordon      Mark node as schedulable
    drain         Drain node in preparation for maintenance
    taint         Update the taints on one or more nodes

--------------------------------------------

## Troubleshooting and Debugging Commands:

    describe      Show details of a specific resource or group of resources
    logs          Print the logs for a container in a pod
    attach        Attach to a running container
    exec          Execute a command in a container
    port-forward  Forward one or more local ports to a pod
    proxy         Run a proxy to the Kubernetes API server
    cp            Copy files and directories to and from containers.
    auth          Inspect authorization
    debug         Create debugging sessions for troubleshooting workloads and nodes

--------------------------------------------

## Advanced Commands:

    diff          Diff live version against would-be applied version
    apply         Apply a configuration to a resource by filename or stdin
    patch         Update field(s) of a resource
    replace       Replace a resource by filename or stdin
    wait          Experimental: Wait for a specific condition on one or many resources.
    kustomize     Build a kustomization target from a directory or URL.

--------------------------------------------

## Settings Commands:

    label         Update the labels on a resource
    annotate      Update the annotations on a resource
    completion    Output shell completion code for the specified shell (bash or zsh)

--------------------------------------------

## Other Commands:

    api-resources Print the supported API resources on the server
    api-versions  Print the supported API versions on the server, in the form of "group/version"
    config        Modify kubeconfig files
    plugin        Provides utilities for interacting with plugins.
    version       Print the client and server version information

--------------------------------------------



`kubectl version` 

`kubectl get service` - see cluster is created, cluster IP etc.

![image](https://user-images.githubusercontent.com/88186084/135875921-50f66c81-d4d6-4553-90f4-e358dfdffa69.png)

-----------------------------------------------------

## Simple cluster interaction

## Deployment

### nginx-deploy.yml 

    # KB works with API versions to declare the resources
    # We have to declare the apiVersion and the kind of service/component

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment # naming the deployment


    spec:
      selector:
        matchLabels:
          app: nginx # look for this label to match with k8 service

      # Lets create a replica set of this with 2 instance/pod
      replicas: 2

      # template to use its labal for K8 service to launch in browser
      template:
        metadata:
          labels:
            app: nginx # This label connects to the service or any other

        # Lets define the container spec
        spec:
          containers:
          - name: nginx
            image: zeeshanj/sre_customised_nginx:latest # name of image pulls from dockerhub
            ports:
            - containerPort: 80

--------------------------------

`kubectl create -f nginx-deploy.yml`

--------------------------------

### Describe
`kubectl get pods` - shows pods available

`kubectl describe pod nginx-deployment-5d9cb6447-fjhb7` - use the 

--------------------------------

### Editing the file

- `kubectl edit deploy nginx-deployment`
- Opens up the notepad where you can edit the code
- Lets edit the number of pods available

![image](https://user-images.githubusercontent.com/88186084/135882541-603d3998-6e04-44b7-a3fb-f3fb339bd71d.png)

----------------------------------------------

![image](https://user-images.githubusercontent.com/88186084/135882624-60d56971-b81d-40fe-80df-cff003387af2.png)

- Make sure to save on Notepad before exiting

-------------------------------------------------

- Now we have 3 pods running
![image](https://user-images.githubusercontent.com/88186084/135882741-bfbf572f-c81f-4d91-94ee-a05c59326fbe.png)

-----------------------------------------------------

## nginx-service.yml

Now we will create this file in the same folder and will make the nginx page globally available



    ---
    # Select the type of API version and the type of service/object
    apiVersion: v1
    kind: Service
    # Metadata for name
    metadata:
      name: nginx-deployment
      namespace: default

      # Specification to include ports Selector to connector
    spec:
      ports:
      - nodePort: 30442
        port: 80
        protocol: TCP
        targetPort: 80

    # Lets define the selector and label to connect to nginx
      selector:
        app: nginx # this label connects this service to deployment

      # Creating LoadBalancer type of deployment
      type: LoadBalancer

-------------------------

Creates the service.yml file - `kubectl create -f nginx-service.yml`

-------------------------

`kubectl get service` - gets information about the 

![image](https://user-images.githubusercontent.com/88186084/135884448-faf07104-35a2-4fc5-9eee-b9661d5fa1c5.png)

-------------------------

## Localhost

![image](https://user-images.githubusercontent.com/88186084/135885034-393a6b58-d0f9-4ccc-a4a6-bcb458de46fa.png)

-------------------------

- SELF HEALS - when delete pod makes new one. localhost always running. Like an ASG

![image](https://user-images.githubusercontent.com/88186084/135884979-e806df8c-a8c7-473a-a8d7-a74b8bbc4a95.png)






