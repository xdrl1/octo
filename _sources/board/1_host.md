# Host Machine Setup

## Docker Image

This project requires Xilinx Vitis-AI ([v3.5](https://xilinx.github.io/Vitis-AI/3.5/html/index.html)) docker containers, which are only officially tested in

- Ubuntu 20.04
- CentOS 7.8, 7.9, 8.1
- RHEL 8.3, 8.4

**This repo is tested on Ubuntu 20.04** using a **PyTorch & CUDA** compatible docker container built by us. You can obtain the docker image by

```bash
docker pull tumbgd/vai-pt-cuda
```

You can also use [official docker images](https://xilinx.github.io/Vitis-AI/3.5/html/docs/install/install.html#option-1-leverage-the-pre-built-docker) if your host machine contains AMD GPUs (ROCm). **Host machines without a GPU are not recommended**, as both model pruning and quantization require GPU acceleration. The Vitis-AI library does not provide API for machines without GPUs (e.g., ).

Alternatively, if you want to build compatible docker images by yourself, the following steps may be helpful for building `CUDA`-enabled ones.

1. Install docker engine. Please follow the [official documents](https://docs.docker.com/engine/install/ubuntu/#installation-methods).

    - You can verify by

        ```bash
        sudo docker run hello-world
        ```

    - Sometimes, you may feel annoying that docker always requires `sudo`. Please refer to [this solution](https://stackoverflow.com/questions/48957195/how-to-fix-docker-got-permission-denied-issue).

2. Install NVIDIA Container Toolkit. Please follow the [official documents](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#docker).

3. Run official building scripts. Note that this step usually costs a long time (approx. 40 mins for a laptop with a low-voltage Intel i7 CPU and 16G DDR4 memory). 42 GB or more free space on your disk is preferred.

    ```bash
    git clone https://github.com/Xilinx/Vitis-AI
    cd ./Vitis-AI/docker
    ./docker_build.sh -t gpu -f <pytorch|tf1|tf2>
    ```

4. Run the built docker image by

    ```bash
    docker run -it --runtime=nvidia --gpus all <docker-id>
    ```

    - You can perform a simple test for GPU compatibility within the container by

        ```bash
        nvidia-smi
        ```
        The output should be like this
        ```
        /Time
        /+-----------------------------------------------------------------------------+
        /| NVIDIA-SMI ___.___.__   Driver Version: ___.___.__   CUDA Version: __._     |
        /|-------------------------------+----------------------+----------------------+
        /| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
        /| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
        /|                               |                      |               MIG M. |
        /|===============================+======================+======================|
        /|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  N/A |
        /|  0%   40C    P8     1W / 120W |       MiB /      MiB |      0%      Default |
        /|                               |                      |                  N/A |
        /+-------------------------------+----------------------+----------------------+
        /
        /+-----------------------------------------------------------------------------+
        /| Processes:                                                                  |
        /|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
        /|        ID   ID                                                   Usage      |
        /|=============================================================================|
        /+-----------------------------------------------------------------------------+
        ```

    - You can also test GPU compatibility w.r.t. DL libraries. Taking `PyTorch` for example:

        ```python
        import torch
        print(torch.cuda.is_available())    # OK if True
        ```

    Note that above steps also work for WSL2. NVIDIA drivers on Windows also support WSL2 (see [here](https://docs.nvidia.com/cuda/wsl-user-guide/)). Installing NVIDIA drivers in Linux is not necessary. Have a try :)

    ```bash
    wsl --install Ubuntu-20.04
    ```
