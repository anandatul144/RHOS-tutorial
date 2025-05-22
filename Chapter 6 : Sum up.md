## Final Hands-On Exercise: Integrated Container Deployment and Management

### Objective

Design and deploy a multi-container application that includes logging, data sharing, service exposure, and CLI interaction. This exercise will test your ability to apply concepts from Chapters 1 to 6.

### Requirements

1. Create a directory structure using Linux commands.
2. Build two container images:

   * A Python logger server
   * A Jupyter notebook reader
3. Push both images to a registry (Quay.io).
4. Define a multi-container Deployment using YAML.
5. Create a PersistentVolumeClaim to share logs.
6. Use `oc` CLI to deploy and monitor.
7. Port forward and validate services.

### Instructions

#### Step 1: Linux Setup (Chapter 1)

```bash
mkdir ~/rhos_final_lab
cd ~/rhos_final_lab
touch logger.py reader.ipynb
```

#### Step 2: Create the Python Logger (Chapters 2, 5)

* Edit `logger.py` to append messages to `/data/log.txt`.
* Create `Dockerfile.logger`:

```Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY logger.py .
RUN mkdir /data
VOLUME ["/data"]
CMD ["python", "logger.py"]
```

* Build and push:

```bash
podman build -f Dockerfile.logger -t quay.io/yourname/logger:latest .
podman push quay.io/yourname/logger:latest
```

#### Step 3: Create Jupyter Notebook Image (Chapter 6)

Use the default `jupyter/base-notebook:latest` image or create a custom one if preferred.

#### Step 4: Define YAML Deployment (Chapters 3–6)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: final-lab-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: final-lab
  template:
    metadata:
      labels:
        app: final-lab
    spec:
      containers:
      - name: python-logger
        image: quay.io/yourname/logger:latest
        volumeMounts:
        - name: shared-logs
          mountPath: /data
      - name: jupyter-reader
        image: jupyter/base-notebook:latest
        ports:
        - containerPort: 8888
        volumeMounts:
        - name: shared-logs
          mountPath: /home/jovyan/data
      volumes:
      - name: shared-logs
        persistentVolumeClaim:
          claimName: shared-data
```

#### Step 5: Create PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: shared-data
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
```

Apply:

```bash
oc apply -f pvc.yaml
oc apply -f deployment.yaml
```

#### Step 6: Monitor and Debug (Chapter 4)

```bash
oc get pods
oc describe pod <pod-name>
oc logs <pod-name> -c python-logger
```

#### Step 7: Port Forwarding (Chapter 4)

```bash
oc port-forward pod/<pod-name> 8888:8888
```

Access the Jupyter UI at `http://localhost:8888`

### Verification Checklist

* Pod is running with two containers.
* Logs are being written to `/data/log.txt`.
* Jupyter notebook can read from `/home/jovyan/data/log.txt`.
* Port forwarding or route exposes notebook UI.


