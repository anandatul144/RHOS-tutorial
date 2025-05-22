## Chapter 3: Intro to RHOS GUI

### Goals

* Guide the user through the RHOS Developer Console
* Understand the concepts: projects, pods, builds, and routes

### Creating a Project

1. Log into the RHOS Developer Console using your web browser and credentials.
2. Click on **Projects** from the left-hand navigation bar.
3. Select **Create Project** and name it `python-server`.
4. Optionally, add a description and display name.
5. Click **Create**.

### Deploying from a Container Image

1. With your project selected, choose **+Add** from the top menu.
2. Select **Container Image**.
3. In the image field, enter the image path you pushed in Chapter 2 (e.g., `quay.io/yourusername/python-http:latest`).
4. Set the application name (e.g., `python-app`) and ensure the port `8080` is correctly detected or manually entered.
5. Click **Create** to deploy.

### Using Topology View

1. After deployment, go to the **Topology** tab.
2. You should see your application represented as a circle.
3. Click on the circle to open detailed information, including pod status, resource usage, and logs.

### Monitoring Logs and Pods

1. Click on the **circle** representing your application in the Topology view.
2. Under the Resources tab, view pod logs.
3. Check status conditions, restart counts, and events.

You can also check pod status via the **Workloads > Pods** section.

---

### Assignment: Deploy via RHOS Developer Console

#### Objective

Use the RHOS GUI to deploy the container image created in Chapter 2.

#### Instructions

1. Log in to RHOS and create a project named `python-server`.
2. Use the **+Add > Container Image** method to deploy the container image `quay.io/yourusername/python-http:latest`.
3. Ensure the exposed port is `8080`.
4. Open the **Topology** view and inspect the running application.
5. Click the route link and verify the Python HTTP server is accessible.

#### Verification

* The project `python-server` should exist.
* The pod status should be **Running**.
* The route should return the directory listing served by the Python server.
* Logs should confirm the message: `Serving on port 8080...`

This concludes the RHOS GUI introduction. Proceed to CLI usage in the next chapter for a deeper understanding of cluster operations.
