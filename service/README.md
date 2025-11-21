# Kubernetes Service Example

This folder demonstrates how to create a Kubernetes Service to expose multiple pods and load balance traffic between them.

## Architecture

### Pods (4 total)
- **3 nginx pods**: Created by the ReplicaSet, running nginx web server on port 80
- **1 curl pod**: A test pod with curl installed to test the service internally

### Service (1)
- **nginx-service**: A network resource that provides:
  - Load balancing across the 3 nginx pods
  - Stable endpoint (`nginx-service:8080`) to access nginx pods
  - External access via NodePort on port 30080

### ReplicaSet (1)
- **nginx**: Manages the 3 nginx pods
  - Ensures 3 replicas are always running
  - Automatically restarts pods if they fail

## How It Works

1. **ReplicaSet** creates and manages 3 nginx pods with label `name: nginx`
2. **Service** uses a selector (`name: nginx`) to find and route traffic to those 3 pods
3. **Load Balancing**: When you access `nginx-service:8080`, the Service distributes requests across the 3 nginx pods
4. **curl pod** can test the Service internally by calling `nginx-service:8080`
5. **External Access**: The Service is exposed externally via NodePort on port 30080

## Key Concepts

- **Service is NOT a pod**: It's a network abstraction that routes traffic to pods
- **Pods share the same cluster network**: All pods can communicate with each other
- **Service provides stable DNS**: Even if nginx pods restart and get new IPs, the Service endpoint remains the same
- **ReplicaSet manages pods**: The Service only provides networking/load balancing

## Files

- `service.yml`: Defines the ReplicaSet, Service, and curl pod
- `dockerfile`: Creates a custom nginx image with curl installed
- `run-service.sh`: Script to build the image and deploy all resources

## Usage

1. **Build and deploy:**
   ```sh
   cd service
   bash run-service.sh
   ```

2. **Check pod status:**
   ```sh
   kubectl get pods
   ```
   You should see 3 nginx pods and 1 curl pod running.

3. **Check service:**
   ```sh
   kubectl get service nginx-service
   ```

4. **Test internally from curl pod:**
   ```sh
   kubectl exec curl -- curl -s nginx-service:8080
   ```

5. **Access externally:**
   ```sh
   minikube service nginx-service --url
   ```
   Open the URL in your browser to see nginx.

## Networking Flow

```
External Request → NodePort (30080) → Service (8080) → One of 3 nginx pods (80)
                                                      ↓
curl pod → nginx-service:8080 → Service → One of 3 nginx pods (80)
```

## Clean Up

```sh
kubectl delete -f service.yml
```
