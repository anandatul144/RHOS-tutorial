## Chapter 5: Create and Deploy a Container

### Goals

* Build, run, and test containers for simple apps

### Exercise 1: Python Webserver

This exercise reinforces previous concepts by guiding you through creating a lightweight Python web server and deploying it in RHOS.

#### Application Code

Create `server.py`:

```python
from http.server import SimpleHTTPRequestHandler, HTTPServer
server = HTTPServer(("0.0.0.0", 8080), SimpleHTTPRequestHandler)
print("Serving on port 8080...")
server.serve_forever()
```

#### Dockerfile

```Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY server.py .
CMD ["python", "server.py"]
```

#### Build and Push

```bash
podman build -t python-webserver .
podman tag python-webserver quay.io/yourusername/python-webserver:latest
podman push quay.io/yourusername/python-webserver:latest
```

#### Deploy in RHOS

Use either the RHOS GUI or `oc apply` with a deployment YAML.

---

### Exercise 2: File Logger + Server

This builds on the webserver example by adding logging capabilities and persistent storage.

#### Updated Application Code

```python
from http.server import BaseHTTPRequestHandler, HTTPServer
import logging

class RequestHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        logging.info("Received GET request")
        self.send_response(200)
        self.end_headers()
        with open("/data/log.txt", "r") as f:
            self.wfile.write(f.read().encode())

logging.basicConfig(filename="/data/log.txt", level=logging.INFO)
server = HTTPServer(("0.0.0.0", 8080), RequestHandler)
server.serve_forever()
```

#### Dockerfile

```Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY server.py .
RUN mkdir /data
VOLUME ["/data"]
CMD ["python", "server.py"]
```

#### Deploy with PVC

Create a PersistentVolumeClaim:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: log-storage
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Mount it in the deployment:

```yaml
        volumeMounts:
        - name: log-volume
          mountPath: /data
      volumes:
      - name: log-volume
        persistentVolumeClaim:
          claimName: log-storage
```

---


### Exercise: Ubuntu Logger and Jupyter Notebook

#### Deployment Overview

* **Container 1**: Ubuntu-based script logs messages into a shared volume
* **Container 2**: Jupyter Notebook reads from the same volume

#### Shared PVC Definition

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

#### Combined Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dual-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dual-app
  template:
    metadata:
      labels:
        app: dual-app
    spec:
      containers:
      - name: ubuntu-writer
        image: ubuntu:latest
        command: ["/bin/bash", "-c", "while true; do echo $(date) >> /data/log.txt; sleep 10; done"]
        volumeMounts:
        - name: shared-volume
          mountPath: /data
      - name: jupyter-reader
        image: jupyter/base-notebook:latest
        ports:
        - containerPort: 8888
        volumeMounts:
        - name: shared-volume
          mountPath: /home/jovyan/data
      volumes:
      - name: shared-volume
        persistentVolumeClaim:
          claimName: shared-data
```

### Route and Access

Expose the Jupyter service by creating a service and route for port 8888.

```bash
oc expose deployment dual-app --port=8888 --name=jupyter-service
oc expose svc jupyter-service
```

Access the notebook via the provided route and navigate to the `/home/jovyan/data` directory to view the shared logs.

\[Content placeholder. Refer to Git history or archived copy for full text.]
