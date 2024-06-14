# gpu - graphics processing unit


Kubernetes GPU support simplifies the process of running and managing applications that need the extra power of GPUs, making it easier to deploy and scale these applications efficiently.

when it comes to running applications that need heavy computational power, like machine learning or scientific simulations, you might need GPUs (Graphics Processing Units). GPUs are much faster than CPUs (Central Processing Units) for these tasks.

 Kubernetes GPU support means that Kubernetes can help manage applications that require GPUs. Here's how it works in simple terms:

* Resource Allocation: Just like Kubernetes can allocate CPU and memory resources to your applications, it can also allocate GPU resources. This means it can assign specific GPUs to your applications based on their needs.

* Scheduling: Kubernetes decides where your application should run. With GPU support, it can ensure that applications needing GPUs are scheduled on nodes (computers in the cluster) that have GPUs available.
 
* Isolation and Sharing: Kubernetes can ensure that multiple applications can share GPUs without interfering with each other. It manages the allocation so that each application gets the GPU resources it needs without hogging all the resources.

* Ease of Use: By using Kubernetes with GPU support, developers don’t have to worry about the nitty-gritty details of managing GPUs. Kubernetes handles this, making it easier to build and deploy GPU-intensive applications.


#### Configuring GPUs in an existing Kubernetes cluster involves several steps to ensure your nodes can schedule and run GPU-accelerated workloads. Here's a detailed guide on how to achieve this:

1. Install NVIDIA Drivers within the worker hosts:
   need to install the NVIDIA drivers on each node that will run GPU workloads. This allows the nodes to recognize and utilize the GPU hardware.

   GPU isn’t a fully autonomous device, it requires a management tool and drivers to be installed on the host machine in order to operate properly.

2. Install NVIDIA Container Toolkit:
   This toolkit allows Docker to use the NVIDIA runtime to manage GPU resources. the NVIDIA Container Toolkit allows you to easily create and run containers that use NVIDIA GPUs for enhanced performance.

3. Label the GPU Nodes:
   Kubernetes uses labels to schedule GPU workloads on nodes with GPU resources.
   Label each node with a GPU-  `kubectl label nodes <node-name> accelerator=nvidia`

4. Deploy the NVIDIA Device Plugin:
   The NVIDIA device plugin for Kubernetes allows GPU resources to be managed by Kubernetes.

5. Verify GPU Node Configuration:
   Ensure that the GPU is recognized and properly configured in the cluster.
   `kubectl describe nodes | grep nvidia.com/gpu`

6. Schedule GPU Workloads:
   schedule GPU-accelerated workloads by specifying the GPU resource requirements in your pod specifications.
   `apiVersion: v1
    kind: Pod
    metadata:
      name: gpu-pod
    spec:
      containers:
      - name: gpu-container
        image: nvidia/cuda:10.0-base
        resources:
          limits:
            nvidia.com/gpu: 1 # requesting 1 GPU
        command: ["nvidia-smi"]
      nodeSelector:
        accelerator: nvidia`
              
