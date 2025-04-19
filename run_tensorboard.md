# Connecting to a Cluster and Running TensorBoard Remotely

To visualize logs (e.g., from PyTorch or TensorFlow) using **TensorBoard** on a remote cluster, you can forward a port from the cluster to your local machine.

## Step 1: Connect to the Cluster (Forward Port)

Run the following command on your **local machine** to connect to the cluster and forward the desired port:

```
ssh -L 6006:localhost:6006 username@cluster.dir
```

> 💡 This command tells SSH:  
> **"Connect this port to my local"** — It forwards port `6006` from the **remote** machine to port `6006` on your **local** machine.

## Step 2: Run TensorBoard on the Cluster

Once connected, run **TensorBoard** on the **cluster**, specifying the same port:

```
tensorboard --logdir=runs/ --port=6006
```

> 🗂️ Replace `runs/` with the path to your actual log directory if different.

## Step 3: Open TensorBoard in Your Browser

Now open your browser and go to:

```
http://localhost:6006
```

You should see the TensorBoard dashboard, served from the remote cluster, as if it were local.


