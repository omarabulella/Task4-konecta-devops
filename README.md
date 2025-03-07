# Task4-konecta-devops
1- create pod nginx with name my nginx direct from command don't use yaml file
explination: i created a pod using the kubectl command without the need for a YAML file. The pod will use the nginx image
Command: kubectl run my-nginx --image=nginx
output: pod/my-nginx created
2- create pod nginx with name my nginx command and use Image nginx123  direct from command don't use yaml file
explination: trying to create a new pod with the same name
Command: kubectl run my-nginx --image=nginx123
output: Error from server (AlreadyExists): pods "my-nginx" already exists
3- check the status and why it dosn't work 
explaination :Why It Doesn’t Work?
The CrashLoopBackOff status indicates that the pod is continuously crashing and restarting
command:kubectl get pods
output:my-nginx    0/1     CrashLoopBackOff   7 (4m47s ago)   10m
4- I ned to know node name - IP - Image
explination: To get the node name, IP, and image for the pod, 
Command:kubectl get pod my-nginx -o wide
output: NAME       READY   STATUS             RESTARTS        AGE   IP       NODE           NOMINATED NODE   READINESS GATES
my-nginx   0/1     CrashLoopBackOff   9 (2m12s ago)   12m   <none>   minikube-m02   <none>           <none>

5- delete the pod 
command: kubectl delete pod my-nginx
output:pod my-nginx deleted

9-Delete any one of the 5 pods and check what happen and explain 
explaination: The ReplicaSet ensures that the number of pods defined 5 replicas in this cas) is maintained.
When a pod is deleted, the ReplicaSet controller notices the discrepancy and creates a new pod to bring the total number of replicas back to 5.
command: kubectl delete pod nginx-replicaset-qsfpw

10-Scale dwon the pods agin to 2 without scale command use terminal  
command: kubectl edit replicasets nginx-replicaset
output:replicaset.apps/nginx-replicaset edited

11- find out the issue in the below Yaml (don't use AI)

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend///////////////// the selector is tier: frontend
  template:
    metadata:
      labels:
        tier: nginx //////////////  the label is tier: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
explaination: the selector was looking for tier: frontend, but the pod template was labeled as tier: nginx.

12- find out the issue in the below Yaml (don't use AI)

apiVersion: apps/v1
kind: deployment
metadata:
  name: deployment-1
  labels: //////////////////// missing
    name: busybox-pod //////// missing
spec:
  replicas: 2
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      labels:
        name: busybox-pod
    spec:
      containers:
      - name: busybox-container
        image: busybox
        command:
        - sh
        - -c ////////// no quotes here
        - "echo Hello Kubernetes! && sleep 3600" /////////////// need quotes



13- find out the issue in the below Yaml (don't use AI)

apiVersion: apps/v1 ////////// corrected apiversion 
kind: Deployment
metadata:
  name: deployment-1
  labels: //////////////////// missing
    name: busybox-pod //////// missing
spec:
  replicas: 2
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      labels:
        name: busybox-pod
    spec:
      containers:
      - name: busybox-container
        image: busybox
        command:
        - sh
        - -c /////////// no quotes needed
        - "echo Hello Kubernetes! && sleep 3600" /////////////// quotes needed

14- what's command you use to know what Image name that running the deployment?
Explanation:
kubectl describe deployment <deployment-name>: This provides a detailed output of the deployment, including the image name under the Containers section. 
command: kubectl describe deployment <deployment-name>


24 -23- Create a LoadBalancer Service:
* Create a LoadBalancer service for your frontend.
* Explain what happens when you try to apply it in an environment that does not support load balancers (e.g., Minikube).
answer: the EXTERNAL-IP will stay in the pending state.

