# Kubernetes ExternalName Service

This folder demonstrates how to use Kubernetes ExternalName Service to access external services (outside the cluster) using internal DNS names.

## What is ExternalName Service?

An ExternalName Service maps a Kubernetes service name to an external DNS name. When pods query the service, Kubernetes returns a CNAME record pointing to the external domain.

**Key characteristics:**
- No pods or endpoints are created
- No load balancing or proxying by Kubernetes
- Works purely at the DNS level (CNAME record)
- Useful for accessing external databases, APIs, or websites

## How It Works

1. You create an ExternalName Service pointing to an external domain (e.g., `example.com`)
2. Pods in your cluster can access it using the service name (`external-website-service`)
3. Kubernetes DNS resolves the service name to the external domain
4. The pod's request goes directly to the external domain

## Architecture

```
Pod (curl) → DNS lookup for "external-website-service"
           ↓
Kubernetes DNS → Returns CNAME: example.com
           ↓
Pod makes request directly to example.com (outside cluster)
```

## Files

- `serviceDomain.yml`: Defines an ExternalName Service pointing to `example.com` and a curl pod for testing

## Usage

1. **Apply the configuration:**
   ```sh
   kubectl apply -f serviceDomain.yml
   ```

2. **Check the service:**
   ```sh
   kubectl get service external-website-service
   ```
   You'll see `TYPE: ExternalName` and `EXTERNAL-NAME: example.com`

3. **Test from curl pod:**
   ```sh
   kubectl exec curl -- curl -s external-website-service
   ```
   This will fetch the content from `example.com`

4. **Verify DNS resolution:**
   ```sh
   kubectl exec curl -- nslookup external-website-service
   ```
   You'll see it resolves to `example.com`

## Use Cases

- **External databases**: Access cloud databases (RDS, Cloud SQL) using a friendly name
- **Third-party APIs**: Reference external APIs with consistent internal names
- **Migration**: Gradually move services into the cluster while maintaining the same service names
- **Multi-cluster**: Access services in other clusters via their external endpoints

## Example Use Cases

### External Database
```yaml
apiVersion: v1
kind: Service
metadata:
  name: production-db
spec:
  type: ExternalName
  externalName: mydb.abc123.us-east-1.rds.amazonaws.com
```

Pods can now connect to `production-db:5432` instead of the full AWS RDS endpoint.

### External API
```yaml
apiVersion: v1
kind: Service
metadata:
  name: payment-api
spec:
  type: ExternalName
  externalName: api.stripe.com
```

Your application can call `payment-api` instead of hardcoding `api.stripe.com`.

## Important Notes

- **No port mapping**: ExternalName services don't support port configuration
- **DNS only**: No load balancing or proxying by Kubernetes
- **Requires DNS**: The external name must be resolvable
- **No selectors**: ExternalName services don't select pods (because they point outside the cluster)
- **HTTPS/TLS**: When accessing HTTPS endpoints, use the external domain name for certificate validation

## Comparison with Other Service Types

| Type | Purpose | Creates Endpoints | Load Balancing |
|------|---------|-------------------|----------------|
| ClusterIP | Internal cluster access | Yes | Yes |
| NodePort | External access via node ports | Yes | Yes |
| LoadBalancer | External access via cloud LB | Yes | Yes |
| **ExternalName** | **Map to external DNS** | **No** | **No** |

## Clean Up

```sh
kubectl delete -f serviceDomain.yml
```
