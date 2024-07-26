# Cluster Set-Up Using K3D
---

### Step 1: Install Docker

K3d runs K3s clusters in Docker containers, so you need Docker installed on your machine.

**For MacOS and Windows:**

Download and install Docker Desktop from [Docker's official website](https://www.docker.com/products/docker-desktop).

**For Linux:**

Install Docker using the package manager of your choice. For example, on Ubuntu:

```bash
sudo apt-get update
sudo apt-get install -y docker.io
```

### Step 2: Install K3d

K3d is a CLI tool, and you can install it using `curl` or `wget`.

**Using curl:**

```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

**Using wget:**

```bash
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

### Step 3: Create a K3d Cluster

Create a K3d cluster using the `k3d cluster create` command. This will create a single-node K3s cluster.

```bash
k3d cluster create mycluster
```

This command will:

1. Pull the necessary Docker images.
2. Create a Docker network for the cluster.
3. Start a container running the K3s server.

### Step 4: Verify the Cluster

After creating the cluster, verify that it is running:

```bash
k3d cluster list
```

You should see your newly created cluster listed.

### Step 5: Use kubectl to Interact with the Cluster

K3d configures your `kubectl` to use the new cluster. Verify `kubectl` is configured correctly:

```bash
kubectl get nodes
```

You should see the node(s) in your cluster listed.

### Step 6: Deploy an Application

Deploy a simple application to test the cluster. For example, deploy an Nginx web server:

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

### Step 7: Access the Application

K3d maps the LoadBalancer service to a port on your localhost. Find the port with:

```bash
kubectl get service nginx
```

Access the application in your browser at `http://localhost:<PORT>`.

### Step 8: Delete the Cluster

When you're done with the cluster, delete it to free up resources:

```bash
k3d cluster delete mycluster
```

### Summary of Commands

1. Install Docker:
    - **MacOS/Windows:** Install Docker Desktop.
    - **Linux:**
      ```bash
      sudo apt-get update
      sudo apt-get install -y docker.io
      ```

2. Install K3d:
    - Using curl:
      ```bash
      curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
      ```
    - Using wget:
      ```bash
      wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
      ```

3. Create a K3d cluster:
    ```bash
    k3d cluster create mycluster
    ```

4. Verify the cluster:
    ```bash
    k3d cluster list
    ```

5. Interact with the cluster using kubectl:
    ```bash
    kubectl get nodes
    ```

6. Deploy an application:
    ```bash
    kubectl create deployment nginx --image=nginx
    kubectl expose deployment nginx --port=80 --type=LoadBalancer
    ```

7. Access the application:
    - Use `kubectl get service nginx` to find the port and access `http://localhost:<PORT>`.

8. Delete the cluster:
    ```bash
    k3d cluster delete mycluster
    ```

## Some More Operations
---
### Cluster Management With K3D

1. **Create a Cluster**
   ```bash
   k3d cluster create mycluster
   ```
   Creates a new K3d cluster named `mycluster`.

2. **List Clusters**
   ```bash
   k3d cluster list
   ```
   Lists all existing K3d clusters.

3. **Delete a Cluster**
   ```bash
   k3d cluster delete mycluster
   ```
   Deletes the K3d cluster named `mycluster`.

4. **Stop a Cluster**
   ```bash
   k3d cluster stop mycluster
   ```
   Stops the K3d cluster named `mycluster`.

5. **Start a Cluster**
   ```bash
   k3d cluster start mycluster
   ```
   Starts the K3d cluster named `mycluster`.

### Node Management

1. **List Nodes**
   ```bash
   k3d node list
   ```
   Lists all nodes across all clusters.

2. **Add a Node to a Cluster**
   ```bash
   k3d node create mynode --cluster mycluster --role agent
   ```
   Adds a new node named `mynode` as an agent (worker) to the cluster named `mycluster`.

3. **Delete a Node**
   ```bash
   k3d node delete mynode
   ```
   Deletes the node named `mynode`.

### Cluster Configuration and Info

1. **Get Cluster Info**
   ```bash
   k3d cluster info mycluster
   ```
   Displays information about the cluster named `mycluster`.

2. **Edit Cluster Configuration**
   ```bash
   k3d cluster edit mycluster --k3s-arg "--disable=traefik@server:0"
   ```
   Edits the configuration of the cluster named `mycluster` to disable Traefik on the server node.

### Networking

1. **Expose Cluster Service**
   ```bash
   k3d cluster create mycluster --port 8080:80@loadbalancer
   ```
   Exposes port 80 of the cluster load balancer on port 8080 of the localhost.

### Volume Management

1. **Mount Volumes**
   ```bash
   k3d cluster create mycluster --volume /path/to/local/dir:/path/in/container
   ```
   Mounts a local directory into the containers.

### Environment Variables

1. **Set Environment Variables**
   ```bash
   k3d cluster create mycluster --env "KEY=VALUE@server:0"
   ```
   Sets environment variables for the specified nodes (in this case, server node 0).

### Logs and Monitoring

1. **View Logs**
   ```bash
   k3d node logs mynode
   ```
   Displays logs for the node named `mynode`.

### Examples

1. **Create a Cluster with One Master and Two Workers**
   ```bash
   k3d cluster create mycluster --agents 2 --servers 1
   ```

2. **Deploy an Application**
   ```bash
   kubectl create deployment nginx --image=nginx
   kubectl expose deployment nginx --port=80 --type=LoadBalancer
   ```

3. **Access a Deployed Application**
   ```bash
   kubectl get service nginx
   ```
   Use the output to determine the port mapped on localhost and access the application.

### Miscellaneous

1. **Version Check**
   ```bash
   k3d version
   ```
   Displays the version of K3d.

2. **Help Command**
   ```bash
   k3d help
   ```
   Displays help information for K3d commands.

These commands should cover most common operations you would need to manage your K3d clusters effectively.