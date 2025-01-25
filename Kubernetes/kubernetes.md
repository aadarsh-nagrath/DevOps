Deployments are abstraction on pods and pods are abstraction on containers.

Components:

### **Pods**
- **Pod**: A pod in Kubernetes is the smallest deployable unit. It encapsulates one or more containers that share the same network namespace and storage. For your full-stack app, each component (frontend, backend, and database) will run in a separate pod.
- A **pod** typically contains one container, but it can host multiple containers that are tightly coupled, such as a backend and a cache system.

### **Deployments**
- **Deployment**: A deployment defines how your app’s pods should be created, updated, and scaled. Each part of your application (frontend, backend, and database) will have its own deployment to ensure you can control the scaling, updates, and management of each part independently.
- Deployments ensure that the required number of pod replicas are always running and will automatically handle pod creation, updates, and rollbacks if there are failures.

### **Services**
- **Service**: A service in Kubernetes abstracts the communication between pods, providing a stable network identity. It ensures that even if the pods behind it change (due to scaling or failure), the service remains constant.
- **ClusterIP** is the default service type, which exposes the service only within the cluster, and it helps backend services (e.g., the frontend connecting to the backend, or the backend connecting to the database).
- **LoadBalancer** and **NodePort** are other types of services used for exposing apps to external traffic. However, in an internal setup, the `ClusterIP` service type is typically sufficient for communication between components.

### **Networking**
- The pods and services interact within the Kubernetes cluster using an internal network. Each pod gets a unique IP, and services act as a stable entry point for pod communication.
- Kubernetes allows **service discovery**, meaning that services can refer to each other by name (e.g., the backend can refer to the database service by using the service name like `db-service`).

### **ConfigMaps and Secrets**
- **ConfigMap**: Stores non-sensitive data like configuration files or environment variables that can be shared among multiple pods. For instance, your backend can use a ConfigMap to access environment settings like the API key.
- **Secret**: Used to store sensitive information such as passwords, tokens, or database credentials. These are securely managed and not exposed in plain text within the Kubernetes environment.

### **Persistent Storage**
- **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)** are used for storage that persists beyond the life of individual pods. For example, the database might need persistent storage to retain data, even if the pod is deleted and recreated.
- Kubernetes manages storage separately from pods to ensure that the data remains intact.

### **Ingress (Optional for External Access)**
- **Ingress** provides HTTP and HTTPS routing to services in the cluster, making it possible to expose your frontend application (and potentially backend) to external traffic. It acts as a reverse proxy, allowing you to define rules for how external requests should be routed to the appropriate services.

### **Scaling**
- Kubernetes allows you to scale each part of the application (frontend, backend, and database) independently. You can increase the number of replicas for the backend if you need more processing power or scale down the frontend when demand is low.

### **Kubelet and Its Role**
The **kubelet** is a Kubernetes node agent responsible for ensuring the containers in a pod are running as expected. It acts as the communication bridge between the Kubernetes **Control Plane** (e.g., the API server) and the node where the pods are deployed.

#### Key Functions:
1. **Pod Management**:
   - Ensures the pods defined in the cluster's desired state are running on the node.
   - Communicates with the container runtime (e.g., Docker, containerd) to start, stop, and manage containers.

2. **Node Health Reporting**:
   - Reports the node's health and resource usage (CPU, memory, etc.) to the Kubernetes **Control Plane**.
   - Ensures nodes are available and ready to run workloads.

3. **Pod Lifecycle**:
   - Monitors the health of the containers in a pod.
   - Restarts containers if they crash or stop unexpectedly.

4. **Volume and Secrets Management**:
   - Mounts volumes and injects secrets into pods as specified in the configuration.

---

### **Kubectl**
The **kubectl** CLI tool is the main way administrators and developers interact with a Kubernetes cluster. It allows you to manage and inspect resources, deploy applications, and view cluster status.

#### Key Functions:
1. **Resource Management**:
   - Create, update, delete, and inspect Kubernetes resources (e.g., pods, deployments, services).

2. **Cluster Interaction**:
   - Send commands to the Kubernetes API server, which then instructs other components (like kubelet and kube-proxy) to perform actions.

3. **Debugging and Monitoring**:
   - Fetch logs (`kubectl logs <pod-name>`).
   - Debug issues using commands like `kubectl describe` and `kubectl get`.

4. **Declarative and Imperative**:
   - Supports both **imperative** (direct commands) and **declarative** (applying YAML files) approaches to manage resources.

---

### **How Service Works as a Load Balancer**
A Kubernetes **Service** is an abstraction that exposes a group of pods as a single network endpoint. It enables communication between pods and external users or other pods inside the cluster.

#### Example of Service Acting as a Load Balancer:
1. **ClusterIP**:
   - Default service type.
   - Distributes traffic between all the pods in a deployment within the cluster.
   - Example: A backend deployment with 3 replicas. A **ClusterIP service** routes incoming requests evenly to all backend pods.

2. **LoadBalancer**:
   - Provides an external IP address to expose a service outside the cluster.
   - Typically integrated with cloud provider load balancers (e.g., AWS ELB, GCP Load Balancer).
   - Example: A web app’s frontend service exposed to the internet.

3. **NodePort**:
   - Exposes the service on a static port on all nodes in the cluster.
   - Example: NodePort service at port `30001` allows access to backend pods through `<node-ip>:30001`.

**How Load Balancing Works**:
- Kubernetes uses a **Service** (backed by **kube-proxy**) to distribute traffic evenly across all the pods.
- For example, a `frontend-service` has 3 backend pods. A round-robin or random distribution method ensures all pods share the incoming traffic.

---

### **Kube-proxy and Its Role**
The **kube-proxy** is a network component that enables communication between services and pods. It runs on each node in the cluster and manages the networking rules.

#### Key Functions:
1. **Service Discovery**:
   - Routes traffic to the appropriate pod based on the service name.

2. **Load Balancing**:
   - Acts as the load balancer for the pods behind a service.
   - Maintains **iptables** or **IPVS** rules for routing traffic.

3. **Cluster Networking**:
   - Handles forwarding requests from services to pods, ensuring communication between different cluster components.

---

### **How to Schedule a Pod**
Scheduling in Kubernetes refers to the process of deciding which node should run a pod.

#### Steps to Schedule a Pod:
1. **Node Selection**:
   - Kubernetes checks which nodes meet the pod's requirements (e.g., CPU, memory, labels).
   - The **kube-scheduler** selects the most suitable node.

2. **Affinity/Anti-affinity Rules**:
   - Use `nodeSelector` or `nodeAffinity` to schedule pods on specific nodes.

3. **Taints and Tolerations**:
   - Nodes can have **taints**, and pods can have **tolerations** to control scheduling.

4. **Resource Constraints**:
   - Pods are scheduled on nodes with sufficient resources (`requests` and `limits` for CPU/memory).

#### Example:
If you want to schedule a pod on a specific node, you can use a **nodeSelector** in the pod's YAML file:

```yaml
nodeSelector:
  kubernetes.io/hostname: "node-1"
```

---

### **How to Monitor and Restart a Pod**
1. **Monitoring Pods**:
   - Use `kubectl get pods` to list pod statuses.
   - Use `kubectl describe pod <pod-name>` to get detailed information, including events.
   - Check logs using `kubectl logs <pod-name>`.

2. **Restarting Pods**:
   - Delete the pod: Kubernetes will recreate it automatically if it is part of a **ReplicaSet** or **Deployment**.
   - Example:
     ```bash
     kubectl delete pod <pod-name>
     ```
   - If the pod is standalone (not part of a ReplicaSet or Deployment), you must recreate it manually.

---

In summary:
- **Kubelet** ensures that pods run correctly on a node.
- **Kubectl** allows users to interact with the cluster and manage resources.
- **Service** enables communication and acts as a load balancer for pods.
- **Kube-proxy** manages networking rules and handles traffic routing.
- Pods are scheduled using the kube-scheduler based on constraints and node availability.
- Pods can be monitored via `kubectl logs` and restarted by deleting them if managed by a higher-level controller like a Deployment.
---

### **API Server in Kubernetes**

The **Kubernetes API Server** is the central management component of Kubernetes. It serves as the gateway for all communication within the cluster, acting as the interface between users, tools (like `kubectl`), and the cluster components.

#### **Role and Working:**
1. **Cluster Entry Point**:
   - The API Server provides a RESTful API that clients (users, automation tools, and other Kubernetes components) use to communicate with the cluster.
   - All `kubectl` commands or API requests go through the API Server.

2. **Authentication and Authorization**:
   - Ensures that incoming requests are authenticated (e.g., via tokens or certificates).
   - Verifies if the request is authorized based on RBAC (Role-Based Access Control).

3. **Validation and Admission**:
   - Validates the request against the cluster's schema.
   - Applies admission controllers to enforce policies like resource limits or pod security.

4. **Request Processing**:
   - Processes requests (e.g., to create, update, or delete resources like pods, deployments, or services).
   - Writes the desired state of the cluster to **etcd**.

5. **Load Balancer Role**:
   - The API Server itself can act as a load balancer for incoming requests.
   - It manages multiple controllers and worker nodes, ensuring efficient distribution of tasks and resources.

---

### **Scheduler in Kubernetes**

The **Kube-scheduler** is responsible for determining which node will run a newly created pod.

#### **Role and Working:**
1. **Node Selection**:
   - Evaluates all nodes in the cluster to find the best node for the pod based on:
     - Resource availability (CPU, memory, etc.).
     - Constraints like `nodeSelector` or `nodeAffinity`.
     - Taints and tolerations.

2. **Prioritization**:
   - Scores nodes based on the pod's requirements (e.g., pods might prefer nodes with lower CPU usage or specific hardware).

3. **Binding**:
   - After selecting a node, the scheduler binds the pod to the chosen node by updating the pod's status in the **API Server**.

---

### **Controller Manager**

The **Kubernetes Controller Manager** ensures that the desired state of the cluster (as specified in manifests) matches the actual state.

#### **Role and Working:**
1. **Controllers**:
   - Contains various controllers, each managing a specific aspect of the cluster:
     - **Node Controller**: Monitors node health and availability.
     - **Replication Controller**: Ensures the correct number of pod replicas are running.
     - **Endpoint Controller**: Updates the endpoints in services.
     - **Job Controller**: Manages batch jobs and ensures they complete.

2. **Continuous Reconciliation**:
   - Controllers continuously watch the cluster's state and reconcile it with the desired state by communicating with the **API Server**.

3. **Automation**:
   - Automates tasks like restarting failed pods, scaling deployments, and updating services.

---

### **etcd**

**etcd** is a distributed key-value store that acts as the Kubernetes cluster's **source of truth** for all cluster data.

#### **Role and Working:**
1. **Cluster State Storage**:
   - Stores all cluster configuration data, including nodes, pods, services, and secrets.

2. **Consistency**:
   - As a distributed system, etcd ensures strong consistency across multiple nodes.
   - Any changes to the cluster state (via the API Server) are stored in etcd.

3. **Data Persistence**:
   - Ensures that the cluster state is preserved in case of API Server or control plane failure.

4. **High Availability**:
   - Can be configured with multiple etcd instances for fault tolerance and redundancy.

---

### **kubectl**

`kubectl` is a command-line tool that interacts with the **API Server** to manage Kubernetes resources.

#### **Role and Working:**
1. **User Interface**:
   - Allows users to create, update, delete, and inspect Kubernetes resources.

2. **Cluster Interaction**:
   - Sends RESTful API requests to the Kubernetes API Server.

3. **Debugging**:
   - Fetch logs (`kubectl logs`) and describe resources (`kubectl describe`) for troubleshooting.

4. **Declarative and Imperative Approaches**:
   - Users can either:
     - Apply YAML manifests (**declarative**).
     - Execute direct commands (**imperative**).

---

### **How the API Server Acts as a Load Balancer**

The API Server is designed to handle a large number of requests efficiently and distribute workloads across the cluster. Here's how:

1. **Request Distribution**:
   - The API Server directs requests (e.g., pod creation) to the appropriate controllers or nodes.

2. **Scaling Components**:
   - When there are multiple replicas of a controller or component, the API Server balances requests between them.

3. **HA Setup**:
   - In a high-availability (HA) Kubernetes cluster, there are multiple instances of the API Server, and external load balancers distribute incoming requests across these instances.

---

### **Summary**

- The **API Server** is the central communication hub, validating, authenticating, and processing all requests.
- The **Scheduler** decides which node runs a pod based on resource availability and constraints.
- The **Controller Manager** ensures the actual state matches the desired state by automating tasks like replication and health checks.
- **etcd** is the storage backend for all cluster data, ensuring consistency and persistence.
- **kubectl** serves as the user's primary tool for interacting with the cluster, enabling resource management and debugging.

### **Minikube**
Minikube is a lightweight Kubernetes implementation used to run a Kubernetes cluster locally on your machine. It's ideal for learning, development, and testing purposes.

#### **Key Features**:
1. **Local Cluster**:
   - Spins up a single-node Kubernetes cluster on your local machine (Linux, macOS, or Windows).

2. **Multi-Node Support**:
   - Allows creation of multi-node clusters for testing distributed applications.

3. **Add-ons**:
   - Includes tools like `dashboard`, `ingress`, and `metrics-server` for extended functionality.

4. **Use Case**:
   - Simplifies Kubernetes experimentation without needing a full cloud setup.

---

### **ReplicaSet**
A **ReplicaSet** ensures a specified number of pod replicas are running at any time. It manages the replication of pods and replaces any failed pods.

#### **Is it an abstraction between Pod and Deployment?**
- Yes, a **ReplicaSet** acts as an intermediary between Pods and Deployments.
  - **Pods**: The smallest deployable unit.
  - **ReplicaSet**: Ensures a specific number of replicas of a pod are running.
  - **Deployment**: Manages ReplicaSets for declarative updates and rollbacks.

---

### **Basic `kubectl` Commands**

1. **View Resources**:
   - `kubectl get pods`: List all pods in the current namespace.
   - `kubectl get services`: List all services.
   - `kubectl get deployments`: List deployments.

2. **Inspect Resources**:
   - `kubectl describe pod <pod_name>`: Detailed info about a pod.
   - `kubectl logs <pod_name>`: View logs for a pod.

3. **Manage Resources**:
   - `kubectl apply -f <file.yaml>`: Apply a configuration file.
   - `kubectl delete pod <pod_name>`: Delete a pod.

4. **Interactive Shell**:
   - `kubectl exec -it <pod_name> -- /bin/bash`: Open an interactive shell inside a running pod.

5. **Namespace Commands**:
   - `kubectl get namespaces`: List all namespaces.
   - `kubectl config set-context --current --namespace=<namespace>`: Set the default namespace.

---

### **Configuration Files in Kubernetes**

Kubernetes uses YAML (or JSON) files to define the desired state of cluster resources.

#### **Key Sections**:
1. **Metadata**:
   - Provides information about the resource.
   - Example:
     ```yaml
     metadata:
       name: my-pod
       labels:
         app: my-app
     ```

2. **Spec**:
   - Describes the desired state.
   - Example:
     ```yaml
     spec:
       containers:
         - name: my-container
           image: nginx
           ports:
             - containerPort: 80
     ```

3. **Status**:
   - Reflects the actual state of the resource.
   - Automatically updated by Kubernetes (e.g., `Running`, `Pending`, etc.).

#### **Labels and Selectors**:
- **Labels**: Key-value pairs assigned to objects (e.g., pods).
  - Example:
    ```yaml
    labels:
      app: web
    ```
- **Selectors**: Used by controllers (e.g., ReplicaSets) to target specific resources.
  - Example:
    ```yaml
    selector:
      matchLabels:
        app: web
    ```

---

### **Ports in Kubernetes**

- **Container Port**:
  - The port inside the container.
  - Example:
    ```yaml
    containerPort: 80
    ```

- **Target Port**:
  - The port where traffic is routed inside the container.
  - Example:
    ```yaml
    targetPort: 80
    ```

- **Node Port**:
  - Exposes a service to the external world via a specific port on a node.

---

### **Kubernetes Namespace**

Namespaces are virtual clusters within a Kubernetes cluster, used to separate and organize resources.

#### **Default Namespaces**:
1. **default**:
   - The default namespace for user-created resources.

2. **kube-system**:
   - Contains resources for the cluster's core components (e.g., `kube-dns`, `kube-proxy`).

3. **kube-public**:
   - A namespace for publicly accessible data (e.g., cluster info).

4. **kube-node-lease**:
   - Used for heartbeats of nodes to determine node availability.

---

### **Summary**

- **Minikube**: A local Kubernetes environment for testing and development.
- **ReplicaSet**: Ensures a specific number of pod replicas; bridges Pods and Deployments.
- **kubectl**: CLI for managing resources (`apply`, `exec`, `logs`).
- **Config Files**: YAML files with metadata, spec, status, labels, and selectors.
- **Namespaces**: Logical separation of resources; four default namespaces organize core and user resources.


### **ConfigMap in Kubernetes**

A **ConfigMap** in Kubernetes is a key-value store that separates application configuration from the application code. It allows you to externalize your configuration so that you can change it without rebuilding your application images or modifying your pods.

#### **Purpose of ConfigMap**:
- Manage configuration data for applications.
- Decouple configuration from containerized applications.
- Share configuration across multiple pods or applications.

---

#### **Why Should Each Namespace Have Its Own ConfigMap?**
1. **Isolation**:
   - Namespaces provide a boundary for resources. Having separate ConfigMaps ensures that configurations for one namespace are not accidentally accessed or modified by applications in another namespace.

2. **Resource Management**:
   - Each namespace might host different teams or applications with unique configurations.

3. **Avoid Conflicts**:
   - ConfigMaps in separate namespaces can have the same name without conflicts (e.g., `app-config` in `namespace-A` and `namespace-B`).

---

#### **How to Define a ConfigMap?**

ConfigMaps can be created using YAML or directly with the `kubectl` command.

**Example of ConfigMap YAML**:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: my-namespace
data:
  database_url: "jdbc:mysql://db-service:3306/mydb"
  log_level: "debug"
  feature_flag: "enabled"
```

**Create it using `kubectl`**:
```bash
kubectl apply -f configmap.yaml
```

**Use ConfigMap in a Pod**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
  namespace: my-namespace
spec:
  containers:
    - name: app-container
      image: nginx
      env:
        - name: DATABASE_URL
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: database_url
```

---

### **What Does `kubans` Do?**
`kubans` is a CLI tool used to quickly switch between namespaces in Kubernetes. It simplifies namespace management and avoids the repetitive need to set the namespace in every command.

- **Usage**:
  ```bash
  kubens <namespace>
  ```
- After switching, all `kubectl` commands operate within the specified namespace until you switch again.

---

### **What Does `kubectl api-resources --namespaced=false` Do?**

This command lists **non-namespaced resources** in the Kubernetes API. Non-namespaced resources are global resources shared across the cluster.

- **Non-Namespaced Resources**:
  - `nodes`: Represents the nodes in the cluster.
  - `clusterroles`: Cluster-wide RBAC roles.
  - `persistentvolumes`: Storage volumes that are not namespace-specific.

**Example**:
```bash
kubectl api-resources --namespaced=false
```

**Output Example**:
```
NAME                  SHORTNAMES   APIGROUP    NAMESPACED   KIND
nodes                 no                        false        Node
persistentvolumes     pv                        false        PersistentVolume
clusterroles                      rbac.authorization.k8s.io  false  ClusterRole
```

---
