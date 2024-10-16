for this pipeline we need 3 secrets saved first 

KUBECONFIG
DOCKER_USERNAME
DOCKER_PASSWORD

we also need ubuntu user access to kubernetes and docker commands

to make docker available for ubuntu user 

```bash
sudo usermod -aG docker ubuntu
sudo systemctl restart docker 
```

Set Up `kubectl` for the Non-Root User (`ubuntu` User)**

When you initialize Kubernetes using `kubeadm`, it generates a `kubeconfig` file located at `/etc/kubernetes/admin.conf` that contains the cluster’s configuration and authentication information. By default, only the root user can access it.

You can copy this file and set appropriate permissions to allow the `ubuntu` user to access it.

Steps:

1. Switch to the Root User:

   If you're not already the `root` user, switch to the `root` user or use `sudo`:

   ```bash
   sudo -i
   ```

2. Create a `.kube` Directory for the `ubuntu` User:

   Create the Kubernetes config directory in the home directory of the `ubuntu` user:

   ```bash
   mkdir -p /home/ubuntu/.kube
   ```

3. Copy the `admin.conf` File to the Non-Root User's Directory:

   Copy the `admin.conf` file (which contains the Kubernetes cluster configuration) to the `.kube` directory of the `ubuntu` user:

   ```bash
   sudo cp /etc/kubernetes/admin.conf /home/ubuntu/.kube/config
   ```

4. Change Ownership of the Config File:

   Change the ownership of the `.kube/config` file so that the `ubuntu` user can read it:

   ```bash
   sudo chown ubuntu:ubuntu /home/ubuntu/.kube/config
   ```

5. **Set Permissions for the Config File:**

   Ensure the permissions are correct for the `.kube/config` file:

   ```bash
   sudo chmod 600 /home/ubuntu/.kube/config
   ```

6. Switch Back to the `ubuntu` User:

   Now switch back to the `ubuntu` user to test the setup:

   ```bash
   su - ubuntu
   ```

7. **Verify kubectl Works for the Non-Root User:**

   Run a Kubernetes command to verify that the non-root user can interact with the cluster:

   ```bash
   kubectl get nodes
   ```

   You should now be able to see the nodes in the cluster.
---

to get config file go to ubuntu user or root user and type this command 

```bash
cat ~/.kube/config | base64
```
---



