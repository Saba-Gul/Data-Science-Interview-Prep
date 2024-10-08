## Deploying a containerized app to Google Kubernetes Engine (GKE) 
Deploying a containerized app to **Google Kubernetes Engine (GKE)** requires a series of steps to prepare the environment, containerize your application, push the container image to a registry, configure Kubernetes, and finally deploy the app to GKE. Here's a comprehensive list of steps to deploy a containerized app to GKE from your local development environment (IDE).

### **1. Prerequisites**
Before you begin, ensure you have the following:
- **Google Cloud account** with a project set up.
- **Google Cloud SDK** installed.
- **Docker** installed for building your container image.
- **kubectl** command-line tool installed (comes with Google Cloud SDK).
- Enable the following Google Cloud APIs in your project:
  - **Kubernetes Engine API**
  - **Container Registry API**

### **2. Authenticate with Google Cloud**
Make sure you are authenticated to Google Cloud and set your project:

```bash
gcloud auth login
gcloud config set project [PROJECT_ID]
```

### **3. Create a GKE Cluster**
Create a GKE cluster where your application will run. The following command creates a cluster with 3 nodes in the `us-central1-a` zone:

```bash
gcloud container clusters create [CLUSTER_NAME] \
    --zone us-central1-a \
    --num-nodes=3
```

Wait for the cluster to be created. Once it's ready, configure `kubectl` to use the new cluster by fetching its credentials:

```bash
gcloud container clusters get-credentials [CLUSTER_NAME] --zone us-central1-a
```

### **4. Containerize Your Application**
If you haven’t already containerized your application, create a `Dockerfile`. Example `Dockerfile` for a Python app:

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Set the working directory in the container
WORKDIR /app

# Install system dependencies for Python and OpenCV
RUN apt-get update && apt-get install -y \
    libgl1-mesa-glx \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1

# Install Python dependencies
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . /app

# Expose the application’s port (8080 as an example)
EXPOSE 8080

# Run the application
CMD ["python", "app.py"]
```

### **5. Build the Container Image**
Now, build your Docker container image and tag it for Google Container Registry (GCR). Replace `[PROJECT_ID]` with your project ID:

```bash
docker build -t gcr.io/[PROJECT_ID]/[IMAGE_NAME]:latest .
```

### **6. Push the Container Image to GCR**
Push the container image to Google Container Registry so that it can be pulled by GKE:

```bash
docker push gcr.io/[PROJECT_ID]/[IMAGE_NAME]:latest
```

### **7. Create a Kubernetes Deployment YAML**
Write a `deployment.yaml` file that describes the Kubernetes resources for your application:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: [APP_NAME]
spec:
  replicas: 3
  selector:
    matchLabels:
      app: [APP_NAME]
  template:
    metadata:
      labels:
        app: [APP_NAME]
    spec:
      containers:
      - name: [APP_NAME]
        image: gcr.io/[PROJECT_ID]/[IMAGE_NAME]:latest
        ports:
        - containerPort: 8080
        resources:
          limits:
            memory: "512Mi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: [APP_NAME]-service
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 8080
  selector:
    app: [APP_NAME]
```

### **8. Deploy the Application to GKE**
Use `kubectl` to apply the deployment configuration to GKE:

```bash
kubectl apply -f deployment.yaml
```

### **9. Verify the Deployment**
Check the status of your deployment to ensure that the pods are running:

```bash
kubectl get pods
```

You should see your pods running.

### **10. Expose the Application**
If your service is of type `LoadBalancer`, Kubernetes will provision a public IP address. To get the external IP of the service:

```bash
kubectl get services
```

Wait for the `EXTERNAL-IP` field to be populated. This IP will allow you to access your application.

### **11. Scale Your Deployment (Optional)**
If you want to scale your application based on demand, use the following command to increase or decrease the number of pods:

```bash
kubectl scale deployment [APP_NAME] --replicas=5
```

### **12. Clean Up Resources (When Finished)**
To avoid incurring costs after you're done testing or running the app, you can clean up the resources:

- **Delete the cluster**:

```bash
gcloud container clusters delete [CLUSTER_NAME] --zone us-central1-a
```

- **Delete the container image**:

```bash
gcloud container images delete gcr.io/[PROJECT_ID]/[IMAGE_NAME]:latest --force-delete-tags
```

### **Summary of Steps**
1. **Authenticate** to Google Cloud.
2. **Create a GKE Cluster**.
3. **Build and Push the Container Image** to Google Container Registry (GCR).
4. **Write the Kubernetes Deployment YAML**.
5. **Deploy the Application** to GKE using `kubectl`.
6. **Verify and Expose the Service**.
7. Optionally **scale the application** as needed.
8. **Clean up resources** when no longer needed.

These steps cover the entire process from your local development environment to deploying a containerized app on GKE. 
