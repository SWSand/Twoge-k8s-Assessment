
# 🐦 Twoge - Twitter Alternative Deployment with Kubernetes and AWS EKS 🚀

Welcome to the deployment guide for **Twoge**, a Python Flask-based Twitter alternative! In this guide, you'll learn how to deploy Twoge on **Minikube** for local development and scale it to **AWS EKS** for production.

---

## 🚀 Project Overview

Twoge is a simple Twitter alternative built with Python Flask. The goal of this project is to demonstrate how to deploy containerized applications using Kubernetes on Minikube and AWS EKS.

**Key Features:**
- 📦 Dockerized Python Flask app
- 🐘 PostgreSQL database with persistent storage
- 🔐 Configured with ConfigMaps and Secrets
- 🌍 Exposed via LoadBalancer for AWS EKS
- ⚙️ Optional CI/CD pipeline with GitHub Actions

---

## 📋 Assessment Objectives
1. **Dockerize the Application:** Create a Docker image for the Twoge application.
2. **Deploy on Minikube:** Set up Kubernetes deployments and services for local development.
3. **Configure Database:** Use ConfigMaps and Secrets to configure PostgreSQL.
4. **Namespace:** Organize resources in a dedicated namespace.
5. **Probes:** Implement liveness or startup probes for lifecycle management.
6. **Persistent Storage:** Attach a Persistent Volume Claim (PVC) for database persistence.
7. **Deploy on AWS EKS:** Migrate deployment to AWS EKS with LoadBalancer.
8. **CI/CD Pipeline:** (Optional) Implement CI/CD with GitHub Actions.

---

## 🗂️ Project Structure

```bash
├── Dockerfile              # Dockerfile for Twoge
├── configmap.yaml          # ConfigMap for environment variables
├── secrets.yaml            # Secrets for database credentials
├── namespace.yaml          # Namespace definition
├── twoge-deployment.yaml   # Twoge app deployment
├── twoge-service.yaml      # Twoge app service (LoadBalancer for AWS)
├── db-deployment.yaml      # PostgreSQL deployment
├── db-service.yaml         # PostgreSQL service (ClusterIP)
├── storageclass.yaml       # Storage class for persistent volumes
├── pvc.yaml                # Persistent Volume Claim (PVC)
```

---

## 🚧 Steps to Deploy

<details>
  <summary>🛠️ 1. Dockerize the Twoge Application</summary>
  
- Create a `Dockerfile` for the Twoge Flask application.
- Build the Docker image using:
  ```bash
  docker build -t twoge-app .
  ```

</details>

<details>
  <summary>🏗️ 2. Deploy Twoge on Minikube</summary>

- Write deployment and service YAML files for the application and database.
- Apply the configuration:
  ```bash
  kubectl apply -f namespace.yaml
  kubectl apply -f configmap.yaml
  kubectl apply -f secrets.yaml
  kubectl apply -f db-deployment.yaml
  kubectl apply -f db-service.yaml
  kubectl apply -f twoge-deployment.yaml
  kubectl apply -f twoge-service.yaml
  ```
  
</details>

<details>
  <summary>🛡️ 3. Configure PostgreSQL Database</summary>

- Store PostgreSQL credentials in Kubernetes Secrets:
  ```yaml
  # secrets.yaml
  apiVersion: v1
  kind: Secret
  metadata:
	name: twoge-secrets
	namespace: jose
  type: Opaque
  data:
	POSTGRES_PASSWORD: cGFzc3dvcmQ=   # base64-encoded "password"
	POSTGRES_USER: dHdvZ2U=           # base64-encoded "twoge"
	POSTGRES_DB: dHdvZ2U=             # base64-encoded "twoge"
  ```

</details>

<details>
  <summary>🧑‍💻 4. Organize with Namespace</summary>

- Use `namespace.yaml` to define a namespace for Twoge:
  ```yaml
  # namespace.yaml
  apiVersion: v1
  kind: Namespace
  metadata:
	name: jose
  ```

</details>

<details>
  <summary>👨‍⚕️ 5. Implement Probes (Liveness/Readiness)</summary>

- Implement a probe in your Twoge deployment YAML file:
  ```yaml
  livenessProbe:
	httpGet:
	  path: /
	  port: 80
	initialDelaySeconds: 5
	periodSeconds: 5
	timeoutSeconds: 10
  ```

</details>

<details>
  <summary>💾 6. Configure Persistent Storage</summary>

- Use Persistent Volume Claims (PVC) to persist PostgreSQL data:
  ```yaml
  # pvc.yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
	name: twoge-pvc
	namespace: jose
  spec:
	accessModes:
	  - ReadWriteOnce
	resources:
	  requests:
		storage: 1Gi
  ```

</details>

<details>
  <summary>🌐 7. Deploy Twoge on AWS EKS</summary>

- After configuring AWS EKS, migrate the deployment to the cloud.
- Change the service type to `LoadBalancer` for public access:
  ```yaml
  # twoge-service.yaml
  apiVersion: v1
  kind: Service
  metadata:
	name: twoge-service
	namespace: jose
  spec:
	selector:
	  app: twoge
	ports:
	  - protocol: TCP
		port: 80
		targetPort: 80
	type: LoadBalancer
  ```

</details>

<details>
  <summary>📦 8. Optional: CI/CD Pipeline with GitHub Actions</summary>

- Automate your deployments using GitHub Actions for CI/CD.
  
</details>

---

## 📑 Key Files

| File                  | Description |
|-----------------------|-------------|
| `Dockerfile`           | Defines the Docker image for Twoge |
| `namespace.yaml`       | Namespace definition |
| `twoge-deployment.yaml`| Twoge app deployment configuration |
| `twoge-service.yaml`   | Twoge app service configuration |
| `configmap.yaml`       | Database config (non-sensitive) |
| `secrets.yaml`         | Database credentials (sensitive) |
| `db-deployment.yaml`   | PostgreSQL deployment configuration |
| `db-service.yaml`      | PostgreSQL service configuration |
| `storageclass.yaml`    | Storage class for persistent volume |
| `pvc.yaml`             | Persistent Volume Claim (PVC) |

---

## 🎯 Architecture Diagram

![Twoge Kubernetes Deployment Architecture](diagram-placeholder.png)

### Description:
- **Minikube Deployment**: Twoge app and PostgreSQL deployed on Minikube.
- **AWS EKS Deployment**: Twoge app deployed on AWS with LoadBalancer for external access.
- **Database**: Managed using Kubernetes StatefulSet for persistence, with PVC.

---