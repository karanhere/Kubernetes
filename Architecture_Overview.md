
The entire architecture can be divided into two parts viz.
The Master Node (Manage,Plan, schedule,monitor nodes)
The Worker Node (Host applications as containers)

The Master node consists of components that enable management of the worker nodes
The entire information about the containers being created, deleted, updated is stored in the ETCD database in the form of key-value pairs.
There are different control plane managers like replication controller, node controller for each object.
The Kube-scheduler decides the node where the container should be placed based on its requirements
The entire orchestration of the Kubernetes cluster and handling requests from external user is done by the kube api server.
The Worker node consists of components needed to communicate with the master node and also with containers
The kubelet is an agent installed on each node that sends information about health and monitoring logs to the master via the kube api server.
The services in containers communicate with each other with the help of kube proxy.
As all applications are containerised, thus a Container runtime engine is installed on both master and worker nodes, example:
Docker, orkt.

The ETCD Database:
It is a simple,reliable, distributed key-value pair datastore for faster access of data.
As compared to relational databases, key-value datastores consist of two columns viz key and values respectively.
There cannot be duplicate keys in any such datastore.
It listens on the IP of the server and port 2379.
The “kubectl get…” command fetches information from the etcd database and displays the output.
In HA environment, multiple etcd db’s are installed and differentiated based on the 

            The Kube-API server :
The “kubectl” command line does a POST(http call) request to the kube-api server to perform any functions.
The kube api is responsible for the orchestration of the entire k8s cluster.
The kube api server receives a request, then validates it and responds back about the status of the task.

The Kube scheduler:
The kube scheduler constantly monitors the status of the kube api server and if it finds any operation to be performed as deploying pods then it checks and finds the appropriate node and then proceeds with the deployment of the pod to that node.
Note that the kube scheduler only decides which pod goes on which node and then transfers this information to the kube api server.
This information is then passed onto the kubelet on the worker node, which then creates  a pod on that node.

Lets understand a basic operation of pod creation :

The kube api server authenticates users, validates requests and then updates the etcd datastore with the information for pod creation and sends an acknowledgement back to the user that the pod has been created. 
Note that the pod created has no node being assigned yet.
The kube scheduler constantly monitors the kube api server and then it finds a pod created with no node assigned yet, so it finds the appropriate node and then informs the kube api server about that node.
This updated information on the kube api server is then updated in the etcd datastore and then this information is passed onto the kubelets in the worker nodes which then create pods.
These kubelets then install a container runtime engine on the pods for the respective application.
Infact kube api server is the only component that interacts directly with the etcd datastore.

The Kube-controller manager:
The kubernetes cluster consists of different controller managers based on the operations they perform. The main reason for having these controllers is that they constantly monitor the status 
