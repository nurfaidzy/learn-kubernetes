# Kubernetes Learning Project

A comprehensive hands-on learning repository covering essential Kubernetes concepts, resources, and deployment patterns using Minikube.

## 📚 Learning Topics Covered

### 1. **Core Concepts**
- **Pods** (`templateMain/`) - Basic unit of deployment in Kubernetes
- **Labels & Annotations** (`label/`, `anotation/`) - Metadata for organizing and managing resources
- **Namespaces** (`nameSpace/`) - Virtual clusters for resource isolation

### 2. **Workload Resources**
- **ReplicationController** (`replicationController/`) - Ensures specified number of pod replicas (legacy)
- **ReplicaSet** (`replicationSet/`, `replicaMatchExpres/`) - Modern pod replication with advanced selectors
- **Deployment** (`deploy/`) - Declarative updates for Pods and ReplicaSets
- **StatefulSet** (`statefullSet/`) - Manages stateful applications with stable network identities
- **DaemonSet** (`daemonSet/`) - Ensures pods run on all (or selected) nodes
- **Job** (`job/`) - One-time task execution
- **CronJob** (`cronJob/`) - Scheduled recurring jobs

### 3. **Networking & Service Discovery**
- **Services** (`service/`) - Stable network endpoints for accessing pods
  - ClusterIP, NodePort, LoadBalancer types
- **External Services** (`externalService/`) - ExternalName for mapping to external DNS
- **Ingress** (`ingress/`) - HTTP/HTTPS routing to services
- **Expose Port** (`exposePort/`) - NodePort service configuration
- **Load Balancer** (`loadBalancer/`) - External load balancing

### 4. **Configuration & Secrets**
- **ConfigMap** (`configMap/`) - Store non-confidential configuration data
- **Secrets** (`secret/`) - Store sensitive information (passwords, tokens)
- **Environment Variables** (`environtment/`) - Passing configuration to containers
- **Downward API** (`DownwardAPI/`) - Expose pod/container metadata to containers

### 5. **Storage**
- **Volumes** (`volume/`) - Persistent storage for pods
- **PersistentVolume** (`persistentVolume/`) - Cluster-level storage resources
- **Sharing Volumes** (`sharingVolume/`) - Volume sharing between containers in a pod

### 6. **Pod Configuration**
- **Multi-Container Pods** (`multiContainerPod/`) - Running multiple containers in a single pod
- **Resource Requests & Limits** (`computationResource/`) - CPU and memory management
- **Probes** (`probe/`) - Liveness, readiness, and startup probes for health checks
- **Node Selector** (`nodeSelector/`) - Schedule pods on specific nodes

### 7. **Scaling & Autoscaling**
- **Horizontal Pod Autoscaler** (`horizontalScaling/`) - Automatic scaling based on CPU/memory metrics

## 🛠️ Technologies Used

- **Kubernetes**: Container orchestration platform
- **Minikube**: Local Kubernetes cluster for development
- **Docker**: Container runtime
- **kubectl**: Kubernetes command-line tool
- **WSL**: Windows Subsystem for Linux

## 📁 Project Structure

```
Kubernetes/
├── anotation/          # Annotations examples
├── computationResource/# Resource requests and limits
├── configMap/          # ConfigMap examples
├── cronJob/            # Scheduled jobs
├── daemonSet/          # DaemonSet examples
├── deploy/             # Deployment with environment variables
├── DownwardAPI/        # Downward API examples
├── environtment/       # Environment variable configuration
├── exposePort/         # NodePort service examples
├── externalService/    # ExternalName service
├── horizontalScaling/  # HPA configuration
├── ingress/            # Ingress routing
├── job/                # Job examples with custom Docker images
├── label/              # Labels examples
├── loadBalancer/       # LoadBalancer service type
├── multiContainerPod/  # Multi-container pod patterns
├── nameSpace/          # Namespace management (dev, test, prod)
├── nodeSelector/       # Node selection for pod scheduling
├── persistentVolume/   # PersistentVolume and PVC
├── probe/              # Health check probes
├── replicaMatchExpres/ # Advanced ReplicaSet selectors
├── replicationController/ # ReplicationController (legacy)
├── replicationSet/     # ReplicaSet examples
├── secret/             # Secrets management
├── service/            # Service types and networking
├── sharingVolume/      # Volume sharing patterns
├── statefullSet/       # StatefulSet for stateful apps
├── templateMain/       # Basic pod templates
└── volume/             # Volume examples
```

## 🚀 Getting Started

### Prerequisites
- Docker installed
- Minikube installed
- kubectl installed
- WSL (for Windows users)

### Setup Minikube
```bash
# Start Minikube
minikube start

# Enable required addons
minikube addons enable ingress

# Use Minikube's Docker daemon (for local images)
eval $(minikube docker-env)
```

### Running Examples

Each folder contains YAML configuration files and may include:
- `*.yml` - Kubernetes resource definitions
- `README.md` - Detailed explanations
- `dockerfile` - Custom Docker images
- `*.sh` - Helper scripts for deployment

To apply a configuration:
```bash
kubectl apply -f <folder>/<file>.yml
```

To check resources:
```bash
kubectl get pods
kubectl get services
kubectl get deployments
```

## 📖 Key Learnings

### Namespace Management
- Created namespaces for different environments (dev, test, prod)
- Learned how pods reference namespaces via `metadata.namespace`

### Service Networking
- Services provide stable endpoints for pods
- Services are NOT pods - they're network abstractions
- ReplicaSet manages pods; Service provides load balancing

### Image Management with Minikube
- Use `eval $(minikube docker-env)` to build images directly in Minikube
- Set `imagePullPolicy: Never` for local images
- Faster than `minikube image load`

### Port Forwarding
- NodePort range: 30000-32767
- Minikube requires tunneling: `minikube service <name> --url`
- Use `kubectl port-forward` for custom local ports

### Jobs vs CronJobs
- Jobs: One-time execution, automatic cleanup on completion
- CronJobs: Scheduled execution (e.g., `* * * * *` = every minute)

### Multi-Container Pods
- Multiple containers share the same network and storage
- Useful for sidecar patterns (logging, proxying)

### Ingress Configuration
- Requires Ingress controller addon in Minikube
- Maps domain names to services
- Needs hosts file entry or port-forwarding for local access

## 🎯 Best Practices Learned

1. **Always specify resource limits** to prevent noisy neighbor issues
2. **Use labels and selectors** for flexible resource management
3. **Separate configuration from code** using ConfigMaps and Secrets
4. **Health checks are essential** - use liveness and readiness probes
5. **Use Deployments over ReplicaSets** for better update management
6. **Keep containers stateless** when possible; use StatefulSets only when needed
7. **Test locally with Minikube** before deploying to production clusters

## 🔧 Common Commands

```bash
# Get resources
kubectl get pods
kubectl get services
kubectl get deployments

# Describe resources
kubectl describe pod <pod-name>
kubectl describe service <service-name>

# View logs
kubectl logs <pod-name>

# Execute commands in pods
kubectl exec <pod-name> -- <command>

# Port forwarding
kubectl port-forward service/<service-name> <local-port>:<service-port>

# Delete resources
kubectl delete -f <file>.yml
kubectl delete pod <pod-name>

# Minikube
minikube service <service-name> --url
minikube ip
minikube docker-env
```

## 📝 Notes

- This project uses Minikube with Docker driver on WSL
- Local images must be built in Minikube's Docker daemon
- For browser access, update Windows hosts file (not WSL hosts)
- Port tunneling required for NodePort services in Minikube

## 🎓 Learning Resources

- Official Kubernetes Documentation
- Minikube Documentation
- Hands-on practice with each concept in separate folders

---

**Author**: nurfaidzy KLIKUMKM  
**Purpose**: Learning Kubernetes fundamentals through practical examples
