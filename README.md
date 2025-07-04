# 📦 ChatApp - Kubernetes on Minikube

This project demonstrates deploying a full-stack **Chat Application** on a local **Kubernetes (Minikube)** cluster using **Docker**, **MongoDB**, and **Ingress**.

---

## ✅ Prerequisites

Ensure the following are installed on your system:

- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Git](https://git-scm.com/)
- [Docker Hub account](https://hub.docker.com/) (for pushing images)

---

## 🚀 Setup Instructions

### 1. Install Docker and Minikube

#### Docker:
Follow the official guide: [Install Docker](https://docs.docker.com/get-docker/)

#### Minikube:
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Start Minikube:
```bash
minikube start
```

---

### 2. Clone the Repository

```bash
git clone git@github.com:amitrana-ar/chatapp-kubernetes.git
cd chatapp-kubernetes
```

---

### 3. Build and Push Docker Images

#### Backend

```bash
cd backend
docker build -t <your-dockerhub-username>/chatapp-backend:latest .
docker push <your-dockerhub-username>/chatapp-backend:latest
```

#### Frontend

```bash
cd ../frontend
docker build -t <your-dockerhub-username>/chatapp-frontend:latest .
docker push <your-dockerhub-username>/chatapp-frontend:latest
```

> 💡 Replace `<your-dockerhub-username>` with your actual Docker Hub username.

---

### 4. Create `.env` File

In the root of the project (or inside the backend directory if it uses dotenv):

```env
MONGODB_URI=mongodb://mongo:27017/chatapp
JWT_SECRET=your_jwt_secret_key
PORT=5001
```

> ⚠️ Replace `your_jwt_secret_key` with a strong secret key.

---

### 5. Apply Kubernetes Manifests

Make sure you're inside the project root where the Kubernetes YAML files exist.

```bash
kubectl apply -f namespace.yaml
kubectl apply -f secret.yaml
kubectl apply -f mongodb-pv.yaml
kubectl apply -f mongodb-pvc.yaml
kubectl apply -f mongodb-deployment.yaml
kubectl apply -f mongodb-service.yaml
kubectl apply -f deployment-backend.yaml
kubectl apply -f backend-service.yaml
kubectl apply -f deployment-frontend.yaml
kubectl apply -f frontend-service.yaml
kubectl apply -f ingress.yaml
```

> ✅ All resources will be created inside the specified namespace.

---

### 6. Enable Ingress on Minikube

```bash
minikube addons enable ingress
```

Verify:
```bash
kubectl get pods -n ingress-nginx
```

---

### 7. Access the Application

Get the Minikube IP:

```bash
minikube ip
```

Edit your `/etc/hosts` (Linux/macOS) or `C:\Windows\System32\drivers\etc\hosts` (Windows) file and map a domain to the Minikube IP:

```
<minikube-ip> chatapp.local
```

Now open:

```
http://chatapp.local/
```

> 🧠 Make sure the domain in `ingress.yaml` matches `chatapp.local` or whatever you choose.

---

## 📁 Kubernetes Manifest Files

- `namespace.yaml` – Define project namespace
- `secret.yaml` – Store sensitive data like JWT secret
- `mongodb-pv.yaml` – Persistent Volume for MongoDB
- `mongodb-pvc.yaml` – Persistent Volume Claim for MongoDB
- `mongodb-deployment.yaml` – MongoDB Deployment
- `mongodb-service.yaml` – MongoDB Service
- `deployment-backend.yaml` – Backend Deployment using Docker image
- `backend-service.yaml` – Backend ClusterIP/NodePort service
- `deployment-frontend.yaml` – Frontend Deployment using Docker image
- `frontend-service.yaml` – Frontend service
- `ingress.yaml` – Ingress routing rules

---

## ✅ To-Do / Improvements

- [ ] Add Horizontal Pod Autoscaler (HPA)
- [ ] Add TLS for Ingress (via cert-manager)
- [ ] Add ConfigMaps for environment variables
- [ ] Add Helm chart support
- [ ] CI/CD integration (GitHub Actions)

---

## 🙏 Acknowledgements

- [Kubernetes](https://kubernetes.io/)
- [Minikube](https://minikube.sigs.k8s.io/)
- [Docker Hub](https://hub.docker.com/)
- [MongoDB](https://www.mongodb.com/)

---

## 📬 Contact

For any issues, feel free to open an [Issue](https://github.com/amitrana-ar/chatapp-kubernetes/issues) or reach out.
