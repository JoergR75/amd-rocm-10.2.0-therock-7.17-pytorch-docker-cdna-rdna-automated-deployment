# 🚀 Automated AMD AI Stack: ROCm 10.2.0 / TheRock 7.17 + amdgpu 31.70.0, PyTorch Stable, Transformers, Docker & vLLM

[![ROCm](https://img.shields.io/badge/ROCm-10.2.0-ff6b6b?logo=amd)](https://rocm.docs.amd.com/en/docs-10.2.0/about/release-notes.html)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.15%20%28Stable%29-ee4c2c?logo=pytorch)](https://rocm.docs.amd.com/projects/ai-ecosystem/en/latest/frameworks/pytorch/install.html)
[![Docker](https://img.shields.io/badge/Docker-29.10.x-blue?logo=docker)](https://www.docker.com/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04%20%7C%2026.04-e95420?logo=ubuntu)](https://ubuntu.com/download/server)
[![AMD Radeon AI PRO R9700](https://img.shields.io/badge/AMD-RDNA4%20Radeon(TM)%20AI%20PRO%20R9700-8B0000?logo=amd)](https://www.amd.com/en/products/graphics/workstations/radeon-ai-pro/ai-9000-series/amd-radeon-ai-pro-r9700.html)
[![AMD Ryzen AI](https://img.shields.io/badge/AMD-Ryzen%20AI%20-8B0000?logo=amd)](https://ryzen-ai.com/en/)
[![AMD CDNA MI300 Series](https://img.shields.io/badge/AMD-CDNA%20Instint(TM)%20Architecture-8B0000?logo=amd)](https://www.amd.com/en/technologies/cdna.html)

## 📌 Overview

This repository provides a fully automated, non-interactive deployment environment for AMD GPU development targeting **AI, HPC, and graphics workloads** on Ubuntu **24.04** and **26.04**.

The setup is based on **AMD TheRock 7.17 / ROCm 10.2.0** and **PyTorch 2.15 Stable**, with support for modern AMD GPUs including **RDNA4**.

### 🔧 Platform & GPU Driver

The deployment automatically installs the AMD GPU kernel driver (`amdgpu`) **31.70.0** from AMD's official APT repository, including repository signing keys, DKMS, and required kernel headers.

DKMS automatically rebuilds the driver when the Linux kernel is updated. The setup also configures the required `video`, `render`, and `sudo` permissions.

### 🎨 OpenGL Support

For OpenGL validation and GPU-accelerated graphics workloads, the environment installs Mesa utilities:
OpenGL can be validated with:

```bash
glxinfo -B
```

<img width="1348" height="1342" alt="image" src="https://github.com/user-attachments/assets/8cc55ebb-34f3-49ad-abcb-4f20a68abb8d" />

This reports the active OpenGL renderer, vendor, version, and hardware-acceleration status.

## 🤖 AI & Development Stack

The environment includes **PyTorch 2.15 with ROCm 10.2.0** together with commonly used AI libraries:

- Transformers
- Accelerate
- Diffusers
- Datasets
- SentencePiece

The developer and GPU toolchain includes:

- `cmake`
- `git` / `git-lfs`
- `libstdc++` development headers
- `libmsgpack`
- **TransferBench** for PCIe/HBM bandwidth testing
- `rocminfo`
- `amd-smi`
- `htop` / `ncdu`
- `mesa-utils`

### ✅ Automated Validation

A verification script is generated automatically to validate:

- ROCm and HIP runtime
- PyTorch HIP availability
- GPU enumeration
- On-device tensor execution
- OpenGL renderer and hardware acceleration

The entire deployment is **fully unattended and reproducible**, with automatic cleanup of conflicting ROCm and PyTorch installations before configuring the AMD driver and software stack.

---

## 🖥️ Supported Platforms

| **Component**      | **Supported Versions**                                |
|---------------------|------------------------------------------------------|
| **OS**            | Ubuntu 24.04.5 LTS (Noble Numbat), Ubuntu 26.04.1 LTS (Resolute Raccoon) |
| **Kernels** tested       | 6.8.0-151 (24.04.5) • 7.0.0-38 (26.04.1)                      |
| **GPUs**          | AMD **RDNA4** • **RDNA3** • **CDNA4** • **CDNA3** • **CDNA2** • **CDNA1**           |
| **APUs**        | AMD Ryzen™ AI 300 and 400 series                                    |
| **ROCm/TheRock**          | 10.2.0 / 7.17                                                |
| **PyTorch**       | torch 2.15.0+rocm10.2.0, torchvision 0.29.2+rocm10.2.0, torchaudio 2.11.1+rocm10.2.0      |       |

**⚠️ Note**: **Ubuntu 20.04.x (Focal Fossa)** and **Ubuntu 22.04.x (Jammy Jellyfish)** is **not supported**. The last compatible ROCm version for 20.04 is **6.4.0** and **7.4.2** for 22.04.

---

## ⚡ Features
- Automated **ROCm/TheRock GPU + HIP SDK** installation
- **PyTorch Stable** with GPU acceleration
- Preinstalled **Transformers**, **Accelerate**, **Diffusers**, and **Datasets**
- Integrated **Docker environment** with ROCm GPU passthrough
- **vLLM Docker images** for **RDNA4** and **CDNA**
- Optimized for **AI workloads**, **LLM inference**, and **model fine-tuning**

---

## 🚀 Installation

### 1️⃣ **System preparation**
Install **Ubuntu 24.04.5 LTS** or **Ubuntu 26.04.1 LTS** (Server or Desktop version).

**⚠️ Note**: This Guide uses Ubuntu **24.04.5 LTS**

**Recommendations:**
- Use a fresh Ubuntu installation if possible
- Assign the full storage capacity during installation
- Install **OpenSSH** for remote SSH management
- The script automatically checks the system for installed versions of ROCm/TheRock, PyTorch, and Docker, and removes them if found
  - On a fresh Ubuntu installation, the script automatically skips the deinstallation routine, as illustrated below
    <img width="1555" height="334" alt="image" src="https://github.com/user-attachments/assets/bb2e0c09-af3f-490e-a388-4da4e62d74f2" />
  - If an existing version is detected, it will be deleted, regardless of whether it is the same or an older release.
    <img width="1739" height="607" alt="image" src="https://github.com/user-attachments/assets/7d618bd5-c910-4118-b1b4-a332af4ee152" />

- SBIOS settings:
  - When using Linux, you should disable Secure Boot
  - On WRX80 and WRX90 motherboard solutions, make sure SR-IOV is enabled — there are known issues with Ubuntu Linux detecting the network

### 2️⃣ **Download the Script from the Repository**
```bash
wget https://raw.githubusercontent.com/JoergR75/amd-rocm-10.2.0-therock-7.17-pytorch-docker-cdna-rdna-automated-deployment/refs/heads/main/script_module_ROCm_10.2.0_TheRock_7.17_Ubuntu_24.04-26.04_pytorch.sh
```

<img width="2265" height="471" alt="image" src="https://github.com/user-attachments/assets/a95d7c92-5db8-4dfb-87c8-6787bcea8cbe" />

### 3️⃣ **Run the Installer**
```bash
bash script_module_ROCm_10.2.0_TheRock_7.17_Ubuntu_24.04-26.04_pytorch.sh
```
**⚠️ Note**: Entering the user password may be required.

<img width="1817" height="571" alt="image" src="https://github.com/user-attachments/assets/5a38e42d-85fd-4711-9479-0c423001ed67" />

The installation takes ~15 minutes depending on internet speed and hardware performance.

### 4️⃣ **Reboot the System**
After the successful installation, press "y" to reboot the system and activate all installed components.

<img width="2271" height="822" alt="image" src="https://github.com/user-attachments/assets/08243859-040d-4618-bea9-ef013dfa4d6c" />

## 🧪 Testing ROCm/TheRock + PyTorch Version and GPU Hardware Setup

After rebooting the system, run the diagnostic script to verify that the ROCm software stack, PyTorch installation, and AMD GPU hardware are correctly detected and functioning.

The script creates and runs a simple Python diagnostic file, `test.py`, to confirm that PyTorch with ROCm support is installed correctly and that GPU compute is working end-to-end.

### What the script verifies

- ✅ Detects the operating system version and Linux kernel version.
- ✅ Reports the installed CPU model and total system memory.
- ✅ Verifies the installed AI software stack, including:
  - PyTorch version
  - TheRock/HIP runtime version
  - ROCm version
  - AMDGPU driver version
  - Transformers version
  - AMD SMI tool and library versions
- ✅ Confirms that ROCm is available to PyTorch using `torch.cuda.is_available()`.
- ✅ Automatically detects all installed AMD GPUs and reports key hardware details:
  - GPU model
  - Free and total VRAM
  - PCIe device address
  - PCIe link width
  - PCIe link speed
- ✅ Verifies that each GPU is operating at the expected PCIe bandwidth, for example PCIe Gen5 x16.
- ✅ Performs a PyTorch tensor operation on every detected GPU.
- ✅ Confirms successful GPU initialization, memory allocation, and compute execution for each GPU.
- ✅ Provides a quick system health check to confirm that the workstation is ready for AI inference and training workloads.

Example usage:
```bash
python3 test.py
```
Expected Output Example:

| Radeon AI PRO R9700 Ubuntu 26.04.1 LTS | Radeon AI PRO R9700 Ubuntu 24.04.5 LTS | Sixunited Notebook Ryzen AI 9 HX 470 Ubuntu 26.04.1 LTS |
|--------|--------|--------|
| ![](https://github.com/user-attachments/assets/4c0da6f9-68f2-41f7-a5dc-3b250d38e575) | ![](https://github.com/user-attachments/assets/43b5aae1-0202-4734-bddd-a104d2fdfc8d) | ![](https://github.com/user-attachments/assets/afaa76f9-446f-4056-bbfa-f268e43d4c07) |

With `amd-smi`, you can verify all available GPUs (in this case, 2x Radeon AI PRO R9700 GPUs).

<img width="1699" height="751" alt="image" src="https://github.com/user-attachments/assets/4b14dac1-702f-48c6-b467-2eb282c8e2ac" />

⚠️ **Caution:**  
Make sure **"Re-Size BAR"** is enabled in the **SBIOS**.  
If it is disabled, **P2P** will be deactivated.

## 📶 TransferBench

**TransferBench** is a utility for benchmarking simultaneous memory transfers between user-specified devices (CPUs, GPUs, and NICs).

### What it does

`TransferBench` is a diagnostic tool that measures **memory bandwidth performance** between:

- ✅ Host (CPU) ↔ GPU(s)  
- ✅ GPU ↔ GPU (if multiple GPUs are installed)  
- ✅ GPU internal memory

### Features
TransferBench supports the following features:
- ✅ Multiple Executors: CPU threads, GPU compute kernels (GFX), GPU DMA engines (SDMA), and NIC RDMA.
- ✅ Multi-input or multi-output (MIMO) transfers: Element-wise sum from multiple SRCs to multiple DSTs.
- ✅ Multinode execution: Runs across distributed systems using MPI or sockets.
- ✅ Flexible configuration: Configure benchmarks using config files or presets.
- ✅ Flexible hardware: Runs HIP and CUDA programs on both AMD and NVIDIA hardware.

### How to install?
1️⃣ Install prerequisites
```bash
sudo apt install -y amdrocm-core-sdk10.2
sudo apt install -y rdma-core libibverbs-dev ibverbs-utils
sudo apt install -y openmpi-bin libopenmpi-dev
sudo apt-get install -y mpich libmpich-dev
```

2️⃣ Compile from Source
```bash
cd ~
rm -rf TransferBench
git clone https://github.com/ROCm/TransferBench.git
cd TransferBench
mkdir build && cd build
cmake ..
make
```
Compilation can take several minutes depending on HW used

<img width="2277" height="1712" alt="image" src="https://github.com/user-attachments/assets/99e735dc-78c0-4cb4-8427-46670e7a29ab" />

3️⃣ Get P2P transfer and bandwidth tested
```bash
./TransferBench p2p
```
The results (256MB transfer) indicate that GPU-to-GPU P2P communication is working correctly and that both GPUs exhibit nearly identical performance.

<img width="2066" height="879" alt="image" src="https://github.com/user-attachments/assets/f8e4b121-002d-4c37-a79d-d7a6a003f654" />

### ⚙️ How to Enable **Re-Size BAR** in SBIOS (example ASRock WRX90 evo)

1. Enter **SBIOS**

<img width="1007" height="760" alt="{F9649127-0F1F-4E14-8008-1F3782FBBDEF}" src="https://github.com/user-attachments/assets/9685c1a4-ecab-4fea-8e91-dd21b9869c7e" />

3. Navigate to **Advanced**

<img width="1018" height="761" alt="{135D3B4C-0732-4652-A3C0-1224D275A515}" src="https://github.com/user-attachments/assets/b1cdc3ce-b526-4cdc-b44f-71d1119cf6d7" />

5. Go to **PCI Subsystem Settings** and change **Re-Size BAR Support** to **Enable** 

<img width="1016" height="761" alt="{3C54C3DA-8B82-483C-AEA5-D0A511508780}" src="https://github.com/user-attachments/assets/60536e2b-e59f-4486-a1fc-ab3ff33a3cd8" />

## 🐋 Docker Integration

The script sets up a Docker environment with GPU passthrough support via ROCm.

Check Docker installation and version
```bash
docker -v
```

<img width="1490" height="100" alt="image" src="https://github.com/user-attachments/assets/bf27d291-d859-4854-9fd5-ac8be24ca646" />

### 🤖 vLLM Docker Images

To use vLLM optimized for RDNA4 and CDNA:

Use the container image you need.

**RDNA4** architecture running on Ubuntu 24.04
```bash
docker pull rocm/vllm:rocm10.2.0_ubuntu24.04_py3.14-pytorch_2.14.0_vllm-0.32.0
```

<img width="2129" height="1213" alt="image" src="https://github.com/user-attachments/assets/69dcf2c4-35ed-45eb-977c-1b324d860ba2" />

Further vLLM Docker versions can be verified on AMD Docs:
https://rocm.docs.amd.com/projects/ai-ecosystem/en/latest/inference/vllm.html
or:
https://hub.docker.com/r/vllm/vllm-openai-rocm/tags

Run vLLM with all available AMD GPU access (example for RDNA4 on Ubuntu 24.04)
```bash
sudo docker run -it \
    --device=/dev/kfd \
    --device=/dev/dri \
    --network=host \
    --ipc=host \
    --security-opt seccomp=unconfined \
    --group-add video \
    --entrypoint /bin/bash \
    rocm/vllm:rocm10.2.0_ubuntu24.04_py3.14-pytorch_2.14.0_vllm-0.32.0
```

<img width="1813" height="409" alt="image" src="https://github.com/user-attachments/assets/58b6a1f5-c90c-4e63-8927-3deb04791aba" />

With `amd-smi`, you can verify all available GPUs (in this case, 2× Radeon AI PRO R9700 GPUs).

<img width="2033" height="746" alt="image" src="https://github.com/user-attachments/assets/fec0aa45-af97-4569-9804-eb5aa728b0fd" />

Verify installed vLLM version
```bash
pip3 show vllm
```

<img width="2266" height="598" alt="image" src="https://github.com/user-attachments/assets/e3d00217-4939-4854-8b5e-cafa7ee224b0" />

If you need to add a specific GPU, you can use the **passthrough** option.  
First, verify the available GPUs in the `/dev/dri` directory (host).
```bash
cd /dev/dri && ls
```

<img width="1893" height="98" alt="image" src="https://github.com/user-attachments/assets/789bff7e-b7fd-41e6-afb7-a4a33934e843" />

Let's choose **GPU2**, also referred to as **"card2"** or **"renderD129"**.
```bash
sudo docker run -it \
    --device=/dev/kfd \
    --device=/dev/dri/card2 \
    --device=/dev/dri/renderD129 \
    --network=host \
    --ipc=host \
    --security-opt seccomp=unconfined \
    --group-add video \
    --entrypoint /bin/bash \
    rocm/vllm:rocm10.2.0_ubuntu24.04_py3.14-pytorch_2.14.0_vllm-0.32.0
```
GPU2 has been added to the container

<img width="1826" height="966" alt="image" src="https://github.com/user-attachments/assets/f4d52aba-5458-471e-bee0-a3cf316066ae" />

## How to Save a Modified Docker Container

1️⃣ Open your container and modify it as needed (e.g., install packages, change configurations).

**⚠️ Note: Do not stop or close the container!**

2️⃣ Open another terminal (CLI) window.

3️⃣ Verify the running and stopped containers:
```bash
sudo docker ps -a
```

<img width="844" height="126" alt="image" src="https://github.com/user-attachments/assets/b879c0a2-a071-4307-adba-0da66534fd15" />

4️⃣ In this example, we want to save the running container `loving_wescoff` as a new image named `rocm/vllm-dev:rocm7.2.1_navi_ubuntu24.04_py3.12_pytorch_2.9_vllm_0.16.0_2`:
```bash
docker commit loving_wescoff vllm/vllm-openai-rocm:v0.20.1_2
```

<img width="842" height="46" alt="image" src="https://github.com/user-attachments/assets/968c0c38-20c9-4cac-8928-c4a7797e15a7" />

5️⃣ Verify that the new image was created successfully:
```bash
sudo docker images
```

<img width="855" height="138" alt="image" src="https://github.com/user-attachments/assets/86a03be1-e4e2-4e88-8a28-6d362fb14d7b" />

6️⃣ Start the new container with one GPU (renderD129):
```bash
sudo docker run -it \
    --device=/dev/kfd \
    --device=/dev/dri/card2 \
    --device=/dev/dri/renderD129 \
    --security-opt seccomp=unconfined \
    --group-add video \
    vllm/vllm-openai-rocm:v0.20.1_2
```

<img width="828" height="395" alt="image" src="https://github.com/user-attachments/assets/e7349f84-b08b-4500-988d-19aff77025be" />
