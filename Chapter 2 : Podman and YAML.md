
## Chapter 2: How to Use Docker and Podman — Docker for Pods and RHOS and YAML

### Goals

* Understand Podman basics
* Understand how YAML works in Podman and RHOS
* Use Podman to simulate pods before deploying on RHOS

### Installing Docker or Podman

Depending on your system, install either Podman or Docker.

* **Fedora / RHEL / CentOS (recommended: Podman):**

```bash
sudo dnf install -y podman
```

* **Ubuntu (for Docker):**

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
```

Verify installation:

```bash
podman --version
# or
docker --version
```

---

### Dockerfile Syntax and Structure

A `Dockerfile` is a plain text file that contains a series of instructions to build a container image. Each line in a Dockerfile represents a command that is executed in sequence to create the final image. Below is an example Dockerfile with explanations for each line.

Create a file called `Dockerfile` with the following contents:

```Dockerfile
FROM python:3.11-slim       # Base image containing Python 3.11 with minimal dependencies
WORKDIR /app                # Set the working directory inside the container to /app
COPY server.py .            # Copy the local server.py file into the working directory inside the container
CMD ["python", "server.py"]  # Set the default command to run the Python script when the container starts
```

#### Common Dockerfile Instructions

* `FROM`: Specifies the base image. It must be the first instruction.
* `WORKDIR`: Sets the working directory for any subsequent instructions.
* `COPY`: Copies files from the host filesystem into the container image.
* `ADD`: Similar to COPY, but supports URL sources and automatic unpacking of compressed files.
* `RUN`: Executes a command during the image build process, often used to install packages.
* `CMD`: Sets the default command to run when the container starts. Only the last CMD is used.
* `ENTRYPOINT`: Sets the command that will always run when the container starts.
* `ENV`: Sets environment variables.
* `EXPOSE`: Documents which ports the application will use (optional metadata only).
* `LABEL`: Adds metadata to the image in key=value format.

#### Example with Additional Instructions

```Dockerfile
FROM python:3.11-slim
LABEL maintainer="you@example.com"
ENV PYTHONUNBUFFERED=1
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8080
CMD ["python", "server.py"]
```

In this extended version:

* `LABEL` provides metadata.
* `ENV` sets an environment variable.
* `RUN` installs Python packages.
* `COPY . .` copies all local files into the container.
* `EXPOSE` documents the application’s port.
* `CMD` launches the application.
  This builds a container that runs a Python server script.

Create `server.py`:

```python
from http.server import SimpleHTTPRequestHandler, HTTPServer
server = HTTPServer(("0.0.0.0", 8080), SimpleHTTPRequestHandler)
print("Serving on port 8080...")
server.serve_forever()
```

---

### Build, Run, Inspect, and Stop

Build your container image using Podman or Docker:

```bash
podman build -t python-http .
# or
docker build -t python-http .
```

Run it:

```bash
podman run -d -p 8080:8080 python-http
# or
docker run -d -p 8080:8080 python-http
```

List running containers:

```bash
podman ps
# or
docker ps
```

Stop the container:

```bash
podman stop <container_id>
# or
docker stop <container_id>
```

---

### Working with Volumes and Networks

To persist data or connect multiple containers, use volumes and networks.

Example: Attach a volume to the container

```bash
podman run -d -p 8080:8080 -v ./data:/data python-http
```

Create a network:

```bash
podman network create mynet
```

---

### Image Layers and Tagging

Tag an image:

```bash
podman tag python-http quay.io/yourusername/python-http:latest
```

Push it to Quay.io:

```bash
podman login quay.io
podman push quay.io/yourusername/python-http:latest
```

---

### RHOS Relation: Pods and Deployments

In RHOS, you do not run individual containers directly. Instead, you define them in YAML as pods or deployments. Each deployment may manage one or more pods, and each pod can contain one or more containers.

Basic deployment YAML example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-server
spec:
  replicas: 1
  selector:
    matchLabels:
      app: python-server
  template:
    metadata:
      labels:
        app: python-server
    spec:
      containers:
      - name: python-http
        image: quay.io/yourusername/python-http:latest
        ports:
        - containerPort: 8080
```

---

### Assignment: Build and Deploy

#### Objective

Practice building and running a simple container, tagging it, and preparing for deployment to RHOS.

#### Instructions

1. Create a working directory and enter it:

   ```bash
   mkdir python_http_lab
   cd python_http_lab
   ```

2. Create `Dockerfile` and `server.py` as described above.

3. Build the image:

   ```bash
   podman build -t python-http .
   ```

4. Run the container:

   ```bash
   podman run -d -p 8080:8080 python-http
   ```

5. Test the server in your browser or with curl:

   ```bash
   curl http://localhost:8080
   ```

6. Tag the image:

   ```bash
   podman tag python-http quay.io/yourusername/python-http:latest
   ```

7. Push the image (ensure you have a Quay.io account):

   ```bash
   podman login quay.io
   podman push quay.io/yourusername/python-http:latest
   ```

#### Verification

* Accessing `http://localhost:8080` should return an HTML directory listing.
* The image should appear under your Quay.io account.

Complete these steps before proceeding to RHOS GUI and CLI deployment in the next chapters.
