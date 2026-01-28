# Frontend Kubernetes Deployment

This directory contains the Kubernetes manifests and configuration for deploying the frontend service of the project.

## Contents
- `manifest.yaml`: Main Kubernetes manifest containing all resources for the frontend.
- `nginx.conf`: NGINX configuration (provided via ConfigMap).
- `Dockerfile`: Docker build instructions for the frontend container.
- `namespace.yaml`: Namespace definition (if needed).
- `nginx.conf`: NGINX configuration file.

## Resources Defined in manifest.yaml
- **ConfigMap**: Provides the NGINX configuration to the frontend pods.
- **Deployment**: Deploys the frontend application with 2 replicas, resource limits, and health checks.
- **Service**: Exposes the frontend pods on port 80 within the `pavan` namespace.
- **HorizontalPodAutoscaler**: Scales the frontend deployment based on CPU utilization.
- **TargetGroupBinding**: Integrates the frontend service with AWS ALB (if using AWS).

## Key Features
- **Label Consistency**: Ensures Service and Deployment selectors match for proper adoption.
- **Readiness & Liveness Probes**: Health checks for reliable rolling updates and pod health monitoring.
- **Resource Requests & Limits**: Prevents resource contention and supports autoscaling.
- **Configurable NGINX**: Easily update NGINX settings via ConfigMap.

## Usage
1. **Set Namespace** (if not already created):
   ```sh
   kubectl apply -f namespace.yaml
   ```
2. **Apply ConfigMap and Deployment**:
   ```sh
   kubectl apply -f manifest.yaml
   ```
3. **Verify Resources**:
   ```sh
   kubectl get all -n pavan
   ```
4. **Access the Frontend**:
   - If using AWS ALB, access via the ALB DNS.
   - If using NodePort/LoadBalancer, use the external IP or port.

## Notes
- Update the Docker image (`pavan095/frontend:v1`) as needed for new releases.
- Adjust resource requests/limits and autoscaler thresholds based on your environment.
- For production, review NGINX logging and error levels in the ConfigMap.

## Troubleshooting
- Ensure all labels match between Service and Deployment.
- Check pod logs for errors:
  ```sh
  kubectl logs deployment/frontend -n pavan
  ```
- Use `kubectl describe` for detailed resource status.

---
For more details, see the individual manifest sections and comments.
