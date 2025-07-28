# Typeface Task Helm

This repository contains Helm charts for deploying the **Typeface** application's backend (`typeface-be`) and frontend (`typeface-fe`) services on Kubernetes. The charts provide reusable templates, helper functions, and deployment notes to streamline installation and management.

## Repository Structure

The repository is organized into the following directories:

### 1. `typeface-be/`
**Purpose:** Helm chart for the backend service of Typeface.

#### - `values.yaml`
Defines default configuration for deployments. Key options include:
- **replicaCount**: Number of backend pod replicas.
- **namespace**: Target Kubernetes namespace.
- **image**: Container image repository, tag, and pull policy.
- **imagePullSecrets**: For authenticating with private registries.
- **serviceAccount**: Options to create and configure a service account.
- **podAnnotations** / **podLabels**: Add custom metadata for pods.
- **securityContext** / **podSecurityContext**: Set security-related options for pods and containers.
- **service**:
  - **type**: Usually set to `NodePort` for local access, especially with Minikube.  
    _NodePort_ exposes the service on a port on each node, enabling convenient local development and testing.  
    With Minikube, this allows you to reach your application using `minikube service <service-name>`.
  - **port**: The port exposed by the service inside the cluster.
  - **nodeport**: The port exposed externally on the node (e.g., `30080`).
- **ingress**: Settings for exposing the application via Ingress (usually disabled by default).
- **resources**: CPU and memory requests/limits (tuned for compatibility with lightweight environments like Minikube).
- **autoscaling**: Disabled by default, but can be enabled for horizontal scaling.
- **livenessProbe/readinessProbe**: HTTP probes for health checks. (Currently disabled)

#### - `templates/_helpers.tpl`
Contains helper functions for generating resource names, labels, and service account names. These templates ensure consistent and valid Kubernetes resource naming and labeling.

#### - `templates/NOTES.txt`
Provides deployment notes, including instructions for accessing the service depending on your chosen configuration (`NodePort`, `LoadBalancer`, `ClusterIP`, or `Ingress`). Includes example `kubectl` commands for port-forwarding and service inspection.

### 2. `typeface-fe/`
**Purpose:** Helm chart for the frontend service of Typeface.

#### - `values.yaml`
Configuration options are analogous to the backend chart, with appropriate ports and image references for the frontend application.

#### - `templates/_helpers.tpl`
Helper functions for resource naming, labeling, and service accounts, mirroring the backend chart's structure.

#### - `templates/NOTES.txt`
Deployment notes and example commands for accessing the frontend service, similar to the backend.

## NodePort and Minikube Note

Both charts default the service type to `NodePort` (see `values.yaml`). This allows users running Kubernetes locally (e.g., with Minikube) to access applications via a browser or API client.  
- To access your service with Minikube, use:  
  ```bash
  minikube service <chart-name> -n namespace
  ```
- The `nodeport` field specifies the external port used (e.g., `30080` for backend, `30090` for frontend).

This is particularly helpful for development and testing scenarios.

## How to Use

1. **Clone this repository:**
   ```bash
   git clone https://github.com/aayush-meshram/typeface-task-helm.git
   cd typeface-task-helm
   ```

2. **Install the Helm chart:**
   - Navigate to either the `typeface-be` or `typeface-fe` directory.
   - Run:
     ```bash
     helm upgrade --install <release-name> -n namespace-name
     ```
   - (Optionally) Override configuration options using `--set key=value` or a custom values file.

3. **Access your application:**
   - For local development (Minikube), use `minikube service typeface-be` or `minikube service typeface-fe`.
   - For other environments, refer to the notes in `templates/NOTES.txt`.

## Folder Breakdown

| Folder           | Purpose                                      | Key Components                |
|------------------|----------------------------------------------|-------------------------------|
| `typeface-be/`   | Backend Helm chart                           | `values.yaml`, `_helpers.tpl`, `NOTES.txt`   |
| `typeface-fe/`   | Frontend Helm chart                          | `values.yaml`, `_helpers.tpl`, `NOTES.txt`   |
