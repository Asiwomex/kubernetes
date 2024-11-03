# Kubernetes Deployments

This repository contains Kubernetes configurations for deploying applications with separate setups. Each deployment is organized in its respective subdirectory with detailed instructions for setup and configuration.

## Deployments

- [Django Deployment](./django_deployment)  
  This folder contains Kubernetes manifests for deploying a Django application with a PostgreSQL database. Detailed steps and configurations for setting up the Django environment are provided within.

- [Nginx Deployment](./nginx_deployment)  
  This folder contains Kubernetes manifests for deploying Nginx, configured to serve as a reverse proxy for your applications. Check this directory for configuration details.

## Getting Started

1. **Ensure Kubernetes Cluster is Running**  
   You can use a local cluster like `kind` or `minikube`, or a managed Kubernetes cluster.

2. **Choose a Deployment Directory**  
   Navigate to either deployments based on your desired setup.

3. **Follow Deployment Instructions**  
   Each subdirectory includes its own `README.md` with step-by-step instructions for deployment and configuration.

## Additional Information

For each deployment, you may need:
- **Namespace Configuration**  
  Ensure your deployments are isolated in the correct namespace as per the instructions in each deployment's `README.md`.

- **Base64 Encoding for Secrets**  
  Some configurations require encoding sensitive data, such as database passwords, using base64.

For more details, refer to the specific deployment's documentation.
