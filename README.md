# RHOS-tutorial

# RHOS Tutorial Book: From Basics to Container Orchestration

This book is designed to learn Red Hat OpenShift (RHOS) step by step, using a practical and hands-on approach. Each chapter builds on the previous one, assuming no prior knowledge of cloud or container orchestration.

---

## Chapter 1: Prerequisite — Basic Linux Commands and Applications

### Goals

* Understand basic Linux shell commands
* Get comfortable with navigation, file operations, and process management

### Topics Covered

* Navigating directories: `cd`, `pwd`, `ls`
* File operations: `touch`, `mkdir`, `cp`, `mv`, `rm`, `nano`, `cat`, `less`, `head`, `tail`
* Permissions: `chmod`, `chown`, `ls -l`
* Package management: `dnf`, `yum`, `apt`
* Networking: `ping`, `curl`, `wget`, `netstat`, `ip`, `ss`
* Process monitoring: `ps`, `top`, `htop`, `kill`, `systemctl`

### Exercise

1. Create a directory called `rhos_lab`.
2. Inside it, create a file called `hello.txt` with your name inside.
3. Change permissions to read-only.

---

## Chapter 2: How to Use Docker — Docker for Pods and RHOS

### Goals

* Understand Docker basics
* Use Docker to simulate pods before deploying on RHOS

### Topics Covered

* Installing Docker/Podman
* Dockerfile syntax and structure
* `docker build`, `docker run`, `docker ps`, `docker stop`
* Volumes and networks
* Image layers and tagging
* RHOS relation: Containers vs Pods

### Exercise

1. Build a Docker image that runs a Python HTTP server.
2. Tag and push the image to Quay.io or a local registry.

---

## Chapter 3: Intro to RHOS GUI

### Goals

* Learn the RHOS Developer Console
* Understand the concepts: projects, pods, builds, and routes

### Topics Covered

* Logging in
* Creating a project
* Deploying from container image
* Using topology view
* Monitoring logs and pods

### Exercise

1. Create a new project called `python-server`.
2. Deploy your Docker image from Chapter 2.
3. Access the route and verify your server is running.

---

## Chapter 4: Intro to RHOS CLI

### Goals

* Learn to use `oc` CLI tool
* Automate common tasks

### Topics Covered

* Installing and authenticating `oc`
* `oc new-project`, `oc get`, `oc describe`, `oc logs`
* Using `oc apply` with YAML files
* Port-forwarding and debugging

### Exercise

1. Use CLI to create the same deployment as in Chapter 3.
2. Forward port to localhost and access the service.

---

## Chapter 5: Create and Deploy a Container

### Goals

* Build, run, and test containers for simple apps

### Exercise 1: Python Webserver

* Create a simple Python HTTP server (using `http.server`)
* Containerize it and push to registry
* Deploy it in RHOS

### Exercise 2: File Logger + Server

* Modify the app to write logs to a file
* Serve the logs via HTTP GET
* Validate log persistence with a volume mount

---

## Chapter 6: Create Multiple Connected Containers

### Goals

* Deploy a multi-container setup
* Use persistent volumes
* Run a Jupyter notebook in one container

### Topics Covered

* Multi-container Pods using YAML
* Service and networking between containers
* PersistentVolumeClaims (PVC)
* Exposing Jupyter Notebook via route

### Exercise

1. Create a deployment with:

   * Ubuntu container writing to a shared volume
   * Jupyter container reading the same volume
2. Access notebook and validate data sharing

---

## Suggestions for Improvement

### Questions to Consider:

* Would you benefit from a chapter on Helm or Operators?
* Should we include a CI/CD chapter for automation?
* Do you want to learn how to connect RHOS with GitLab?

### Suggested Additional Chapters:

* **Chapter 7: GitOps with ArgoCD or Tekton**
* **Chapter 8: Monitoring and Logging with Prometheus and Grafana**
* **Chapter 9: Security, RBAC, and Policies in RHOS**
* **Chapter 10: Using Webhooks and GitLab CI to Trigger Deployments**

---

Let me know if you'd like this tutorial translated into an interactive Jupyter Book or converted into a PDF for easier sharing!

Pawssibly ready for feedback! 🐾
