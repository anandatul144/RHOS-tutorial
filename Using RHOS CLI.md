## Chapter 4: Intro to RHOS CLI

### Goals

* Learn to use the `oc` CLI tool
* Monitor existing projects, deployments, and pods from the command line
* Create modifications, stop, and re-run pods using CLI

### Installing and Authenticating `oc`

Install the OpenShift CLI (`oc`) using your system's package manager or download it from the OpenShift console:

```bash
# Example for RHEL-based systems:
sudo dnf install -y openshift-clients

# Verify installation:
oc version
```

Authenticate using your RHOS console:

1. Log into the web console.
2. Click your username in the top right corner.
3. Select **Copy Login Command**.
4. Paste the login command in your terminal.

---

### Key `oc` Commands

The `oc` CLI tool provides comprehensive control over OpenShift resources. Below are important commands and patterns organized by task category.

* Create or switch projects:

```bash
oc new-project python-server
oc project python-server
```

* List resources:

```bash
oc get all
oc get pods
oc get deployments
```

* Get detailed information:

```bash
oc describe pod <pod-name>
oc describe deployment <deployment-name>
```

* View logs:

```bash
oc logs <pod-name>
```

* Apply a modified deployment YAML:

```bash
oc apply -f deployment.yaml
```

---

#### Viewing and Managing Resources

* List all resources in the current project:

```bash
oc get all
```

* List specific resource types:

```bash
oc get pods
oc get services
oc get deployments
oc get routes
```

* View detailed information:

```bash
oc describe pod <pod-name>
oc describe svc <service-name>
oc describe deployment <deployment-name>
```

* Get raw YAML output for any resource:

```bash
oc get deployment <deployment-name> -o yaml
```

#### Logs and Shell Access

* View logs for a pod:

```bash
oc logs <pod-name>
```

* View logs for a specific container in a multi-container pod:

```bash
oc logs <pod-name> -c <container-name>
```

* Execute commands or open a shell inside a container:

```bash
oc exec -it <pod-name> -- /bin/bash
oc rsh <pod-name>
```

#### Creating and Updating Resources

* Create resources from a YAML file:

```bash
oc apply -f resource.yaml
```

* Replace a resource completely (destructive if structure changes):

```bash
oc replace -f resource.yaml
```

* Delete resources:

```bash
oc delete pod <pod-name>
oc delete deployment <deployment-name>
```

* Create config maps and secrets:

```bash
oc create configmap my-config --from-file=config.txt
oc create secret generic my-secret --from-literal=password=admin
```

#### Scaling and Restarting

* Scale a deployment:

```bash
oc scale deployment <deployment-name> --replicas=3
```

* Trigger a redeploy:

```bash
oc rollout restart deployment <deployment-name>
```

* View rollout history:

```bash
oc rollout history deployment <deployment-name>
```

* Roll back to a previous version:

```bash
oc rollout undo deployment <deployment-name>
```

### Port Forwarding and Debugging

To access a pod’s exposed port locally:

```bash
oc port-forward pod/<pod-name> 2222:22
```

This example forwards port 22 (SSH) from the pod to your localhost port 2222.

To open a remote shell into the pod:

```bash
oc rsh <pod-name>
```

---

### Assignment: Modify Deployment via CLI

#### Objective

Use the CLI to add an SSH container into the deployment and access it locally.

#### Instructions

1. Export the current deployment to YAML:

```bash
oc get deployment python-app -o yaml > deployment.yaml
```

2. Edit `deployment.yaml` to add another container:

```yaml
      - name: ssh-container
        image: linuxserver/openssh-server
        ports:
        - containerPort: 22
```

3. Apply the changes:

```bash
oc apply -f deployment.yaml
```

4. Get the pod name:

```bash
oc get pods
```

5. Forward port 22 to your localhost:

```bash
oc port-forward pod/<pod-name> 2222:22
```

6. Test connection:

```bash
ssh user@localhost -p 2222
```

(Ensure SSH credentials are known or default login is enabled in the image.)

#### Verification

* `oc get pods` should show a running pod with two containers.
* `oc logs <pod-name> -c ssh-container` should show SSH service startup.
* `ssh` to `localhost -p 2222` should initiate a valid SSH connection.

This completes your first CLI-based deployment modification and validation.
