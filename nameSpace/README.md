# Kubernetes Namespace and Pod Example

This folder contains YAML files for creating a custom Kubernetes namespace and deploying an Nginx pod into it.

## Files

- `nameSpace.yml`: Defines the namespace `nginx-namespace` with labels and annotations for organizational purposes.
- `nginx-pod.yml`: Deploys an Nginx pod inside the `nginx-namespace` namespace.

## Usage

1. **Create the namespace:**
   ```sh
   kubectl apply -f nameSpace.yml
   ```
2. **Deploy the pod into the namespace:**
   ```sh
   kubectl apply -f nginx-pod.yml
   ```
3. **Check the namespace:**
   ```sh
   kubectl get namespaces
   kubectl describe namespace nginx-namespace
   ```
4. **Check the pod in the namespace:**
   ```sh
   kubectl get pods -n nginx-namespace
   kubectl describe pod nginx-pod -n nginx-namespace
   ```

## How it works
- The namespace is created first, providing a logical space for your resources.
- The pod YAML specifies `namespace: nginx-namespace` so the pod is placed in that namespace.
- The connection between the pod and namespace is made by matching the namespace name in the pod YAML to the namespace created by the namespace YAML.

## Notes
- You can organize your YAML files in any way; the important part is the `namespace` field in your pod YAML matches the namespace you created.
- You can add more resources (pods, services, deployments) to the same namespace by specifying `namespace: nginx-namespace` in their YAML files.
