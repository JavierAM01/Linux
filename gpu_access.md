# Using GPUs in Python

This guide provides a quick overview of how to check and utilize GPUs in Python, including checking how many are available and verifying your access, especially in larger clusters with resource managers.

## 1. Checking How Many GPUs Are Available

To check how many GPUs are available on your system, use the `nvidia-smi` command in the terminal:

```bash
nvidia-smi
```

- This will display details for each GPU on the system, including ID, memory usage, and current processes.
- **Note:** Seeing GPUs listed here only indicates that they are present on the system, not necessarily that you have access to them.

## 2. Verifying GPU Access with Python

To verify how many GPUs you can access in Python, use the following small script with PyTorch:

```python
import torch

num_gpus = torch.cuda.device_count()
for i in range(num_gpus):
    print(f"GPU {i}: {torch.cuda.get_device_name(i)}")
```

- This script lists the GPUs your code has access to. If it lists all GPUs without errors, you likely have access to them.
- **If you encounter issues:** It could indicate restricted permissions or configurations limiting your access.

## 3. Working with Resource Managers in Larger Clusters

In larger clusters, GPU usage is often controlled by resource managers like SLURM or Kubernetes. Even if GPUs are visible via `nvidia-smi`, you may need to request access through these systems. For example:

- **SLURM**: Use the following command to request 2 GPUs for your job:
  
  ```bash
  srun --gres=gpu:2 python my_script.py
  ```

- **Kubernetes**: You may need to define the GPU resource in your deployment YAML file or job specification.

Resource managers help allocate and manage resources efficiently, ensuring fair usage across users. Always check your cluster's specific documentation for the correct usage.

## 4. Additional Notes

- **Permissions**: GPU access can be restricted by system administrators or policies. Always check with your system administrator if you suspect access issues.
- **CUDA Installation**: Ensure that CUDA is installed and compatible with your deep learning libraries (e.g., PyTorch, TensorFlow) for GPU utilization.
- **Library Compatibility**: PyTorch and TensorFlow both detect GPUs automatically if configured correctly. Make sure your environment is set up properly.

By following these steps, you can efficiently check GPU availability, verify access, and utilize GPUs in your Python scripts, even within larger clusters.
