# cmd setting

- set alias
  - alias k= kubectl

# Pods
- kubectl get pods/pod
- kubectl describe pod <pod-name>
`commands to create pod`
    - kubectl run redis --image=redis
    - kubectl create -f <pod_yaml_file_name>
- kubectl delete pod <pod-name>
- kubectl delete pod --all  (deletes all the pods)
- kubectl delete -f <pod_file-name>.yaml
-`command to update existing pod`
    - kubectl apply -f <updated_pod_definition_file>  (apply will create resource if it does not exit already)
    - kubectl edit pod <pod_name> 
    - `to replace an existing pod with new updates`
    - kubectl replace --force -f <yaml file name>
  - `command to fetch existing pod into a yaml file`
    - kubectl get pod <pod_name> -o yaml> pod.yaml
  `command to check user of a pod`
  - kubectl exec ubuntu-sleeper -- whoami
  
## logging pods
- `command to view pod logs`
- kubectl logs <pod-name> -n <pod-namespace>
- kubectl -n <namespace> exec -it app -- cat /log/<pod-name.log>
-`to stream logs live use -f`
- kubectl logs -f <pod-name>
-`if more than 1 container in pod then need to specify container name for log command`
- kubectl logs -f <pod-name> <container-name>

## Monitoring Pods

- using Metrics server, an in-memory solution, does not store on disk
- each cluster can have on metrics server
- k8 runs `kublet` agent on each nodes, it has a sub-component called`c-advisor`, it exposes metrics through kublet api to metrics server
`command to get CPU utilization of each node`
- kubectl top node
`for pod metrics`
- kubectl top pod

## Pod restart

- by default restart policy is set to always, so even if a pod exits after completeing its task, say a computation, k8 tries to restart it.
- we can change the restartPolicy value to Never or OnFailure to prevent this from happening
- `inject in pod.yaml`
spec:
    container:
    restartPolicy: Never| OnFailure

# Replica Sets
- kubectl get rs/replicaset
- kubectl describe rs <rs_name>
- kubectl create -f <rs_yaml_file>
- kubectl delete rs/replicaset <rs_name>
- kubectl delete rs --all
- kubectl delete -f <rs_yaml_file-name>
-`command to update existing rs`
    - kubectl edit rs <rs_name>
    - kubectl apply -f <updated_rs_definition_file>
    - kubectl scale rs <rs_name> --replicas=5

# Jobs

- for batch processing tasks
- to make sure pods complete there task and then exits
- like replicaSet but for tasks that are meant to be finished
- refer yaml file job.yaml for its syntax
- `commands`:
  - kubectl create -f <job-yaml-file>
  - kubectl get jobs
  - kubectl describe job
  - kubectl delete jon <job-name>
  `imperative command`:
  - kubectl create job <job-name> --image=<image-name> --dry-run=client -o yaml  > job.yaml
  
# Cron Job

- to run cronjobs on a scheduled time using cron expression
- kubectl create -f <cronjob-yaml-file>
- kubectl get cronjobs
- kubectl describe cronjob
- kubectl delete jon <cronjob-name>


# Deployments
- kubectl get deployment/deploy
- kubectl describe deployments <deploy_name>
- kubectl create -f <deploy_yaml_file>
- kubectl delete deploy/deployment <deploy_name>
- kubectl delete deploy --all
- kubectl delete -f <deploy_yaml_file>
-`command to update existing rs`
    - kubectl edit deploy <deploy_name>
    - kubectl apply -f <updated_deploy_definition_file>

## update and rollback deployments

- kubectl rollout status deployment/<deployment-name>
- kubectl rollout history deployment/<deployment-name>
- `Deployment Strategies`:
  - *recreate*: all nodes do down, then new are created; app downtime
  - *rolling update*: default; serves go down one by one; no downtime
  - *blue-green*: direct option not available, can be done by changing label selector of a service to redirect traffic from blue to green application
  - *canary*: direct option not available, traffic can be sent to both versions by create a common label: less control over traffic split, service split traffic based on number of pods, so for canary by reducing number of pods, we can reduce incoming traffic. For more control we need to use service mesh with *istio*
`commands to update deployment`
- kubectl apply -f <updated-yaml-file>
- kubectl set image deployment/<deploy-name> <image-name>=nginx:1.9.1( updated-image-name)
`revision flag`: to get history of specific revision use this flag
- kubectl rollout history deployment <deploy-name> --revision=<revision-version>
`record flag`: while update deployments use this flag to record the command used to update for future tracking 
- kubectl set image deployment <deploy-name> nginx=nginx:1.17 --record
`rollback command`
- kubectl rollout undo deployment/<deploy-name> 
`--to-revision flag`: rollback to a specific version of deployment
- kubectl rollout undo deployment <deploy-name> --to-revision=<revision-version>
   

# Namespaces
- kubectl create -f <namespace.yaml>
- kubectl create namespace <namespace_name>
`command to get resource from a specific namespace`
- kubectl get <resource> --namespace=<namespace_name>
- kubectl get pods --namespace=dev
- kubectl get pods -n=dev
`switch to namespace`
-kubectl config set-context $(kubectl config current-context) --namespace=dev
`to get resources from all namespace`
- kubectl get pods --all-namespaces
- kubectl get pods -A
`to get namespaces`
- kubectl get namespace
`to get exact number of namespaces`
- kubectl get ns --no-headers | wc -l
`to create resource in a namespace`
- kubectl run redis --image=redis -n=dev
- kubectl create -f <yaml_file_name> -n=dev


# Config Map

`create`
-`imperative`
- kubectl create configmap <config_name> --from-literal=<key>=<value>
- kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MOD=prod
- kubectl create configmap <config_name> --from-file=<path_to_file>
- kubectl create configmap app-config --from-file=appconfig.properties
-`declerative`
- kubectl create -f <configmap_filename.yaml>
`get and describe`
- kubectl get configmaps/cm
- kubectl describe configmaps/cm 
`injecting configmap to pod defination file`
add below in container section
envFrom:
    -configMapRef:
        name: app-config
or 
containers:
  - env:
    - name: APP_COLOR
      valueFrom:
         configMapKeyRef:
            name: webapp-config-map
            key: APP_COLOR


# Secrets

*secrets are not encrypted, only encoded* - do not push secrets file to github

`create`
-`imperative`
- kubectl create secret generic <secret_name> --from-literal=<key>=<value>
- kubectl create secret generic app-secret --from-literal=DB_HOST=Mysql --from-literal=DB_PASSWORD=@*bc
- kubectl create secret generic <secret_name> --from-file=<path_to_file>
- kubectl create secret generic app-secret --from-file=app_secret.properties
-`declerative`
- kubectl create -f <secret_filename.yaml>
`get and describe`
- kubectl get secrets
- kubectl describe secrets
`to see secretkeys with their values`
- kubectl get secret app-secret -o yaml
`injecting secrets to pod defination file`
add below in containers section
`injection using env`
envFrom:
    -secretRef:
        name: app-secrets
`injection as single env`
containers:
  - env:
    - name: DB_PASSWORD
      valueFrom:
         secretKeyRef:
            name: app-secrets
            key: DB_PASSWORD
`injecting as volumes`: in this case, a new file with keyname is created for each secret inside the app_secret.yaml file with their value as secret value
volumes:
    - name: app-secret-volume
      secret:
        secretName: app-secret


# Service account

- kubectl create serviceaccount <sa-name>
-  kubectl get sa
- kubectl describe sa <sa-name>
`command to update sa in a pod`
- kubectl set serviceaccount <deploy/web-dashboard dashboard-sa>

# resource limits

add resource section to the container part of pods to set requests and limits

resources:
    requests:
        memory: "4Gi"
        cpu: 2
    limits:
        memory: "6Gi"
        cpu: 4

0.1 cpu = 100 m (milli)
minimum cpu value : 1 milli

1 Cpu in kubernetes = 1vcpu in aws= 1 GCP core= 1 azure core= 1 hyperthread

CPU throttle when cpu exceeded

for memory:
1k = 1 Kilo byte = 1000 bytes
1Ki = 1 kibibyte = 1024 bytes
can use more memory than its limit, but tries to exceed limit continously , givess error OOM, out of memory

best allocation strategy: set request and not limit, so that if needed containers can use free CPU/memory available on a pod

## Limit Ranges: ensures each pod has some limit set

This limits are enforced when a pod is created
it will affect new pods after limit rabge is created or updated

## resource Quotas: to set hard limits for all the pods

# Taint nodes and Tolerant pods

- taints and tolerations does not decide the placing factor of tolerant pods, they can even be placed o nodes that has no taints on it.
taint-effect= NoSchedule|PreferNoSchedule|NoExecute
`addt taint`
- kubectl taint nodes <node-name> key=value:<taint-effect>
`remove taint`: add minus at end
- kubectl taint nodes <node-name> key=value:<taint-effect>-
- kubectl describe node kubemaster | grep Taint
-`adding toleration to pod`
spec:
    containers:
    tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"


# Node selectors

- used to place pods on a certain type of node
- cannot use not /OR/AND operator type conditions
`label a node`
- kubectl label nodes <node-name> <label-key>=<label-value>
- kubectl label nodes node01 size=Large
`inject node selector in pod`
spec:
    containers:
    nodeSelector:
        size: Large

# Node Affinity

- 3 types of affinity
  - `requiredDuringSchedulingIgnoredDuringExecution`: affinity checked during scheduling a new pod to node, not checked for running pods 
  - `preferredDuringSchedulingIgnoredDuringExecution`: affinity checked during new pod scheduling, but can be overlooked if no other node has space
  - `requiredDuringSchedulingIgnoredDuringExecution` (development in progress): checks affinity during creation , as well if any changes in affinity of node/pod, removes that pod from the node

`inject affinity to pod`
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In|NotIn|Exists
            values: #a list
            - ssd            
  containers:
  - name: nginx
    image: nginx


# labels and selectors

- set labels in metadata property of yaml files and selectors in spec
- can use these labels to filter later using selectors
- `selector command`
    <kubectl get pods --selector <label-key>=<label-value>>

# annotations
- to record other details for info purpose like version, mobileno, email, etc
  `inject annotation in yaml`:
metadata:
    labels:
        name:app
    annotations:
        buildversion: 1.2
        owner: yashi


# Services

- service spans across all the pods and nodes in a cluster and acts as load balancers automatically
- don't have to do anything addition to run service across multiple nodes
- creates a bridge b/w external traffic and for resources to communicate with each other
- 3 types:
  - node port: services acts as a VM and forwards traffic from node to pod
  - cluster IP: for internal communication b/w different nodes and clusters
  - Load Balancer
- `commands`:
- kubectl get svc/services
- kubectl describe svc <service name>



# ingress

- ingress controllers are nginx server with intelligence build in them 
- refer ingress.yaml for deployment
- Within the image, the NGINX program is stored at location NGINX Ingress controller, so you must pass that as the command to start the NGINX controller service.
- to enable an ingress deployment we require for things: ingress-controller, a service , a service account and a configMap
- An Ingress resource is a set of rules and configurations applied on the Ingress controller. You can configure rules to, say, simply forward all incoming traffic to a single application or route traffic to different applications based on the URL.
- `Commands`
  - kubectl get ingress
  <!-- - to get ingress from all namespaces -->
  - kubectl get ingress -A 
  - kubectl edit ingress <ingress-name>
  - kubectl create -f <ingress-resource-file-name>
  - kubectl describe ingress <ingress-name>
- `imperative`
  - kubectl  create ingress <ingress-resource-name> -n <nampespace> --rule="/<path>=<service-name>:<service-Port>"
  - kubectl  create ingress app-service -n ingress-nginx --rule="/wear=wear-service:8080"


# Networking Policies

- used to enforce network rules b/w pods,  to block one pod from accessing others
- Ingress i when a server received traffic from one server, egress is when a server send traffic to another server
- Must add ingress, egress or both in policyTypes field of yaml field to handle traffic in network policy
- if we allow an incoming traffic in network policy, the response to that is allowed automatically, we need not add an egress rule for that. 
- but for new request out of the pod an egress rule will be needed
- by default traffic from all namespaces is allowed, to allow traffic from only selected namespace, add the namespaceSelector property in network policy.
- if you want to allow access from m server outside k8 cluster, you can use the `ipBlock` field 
- each dash`-` under the the `from` field specifies a new rule, which are treated as OR, if 2 rules are mention within one dash, then they are treated a s an AND condition
- `commands`:
  - kubectl get networkpolicy/netpol