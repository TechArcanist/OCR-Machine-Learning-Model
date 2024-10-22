# OCR with WSL for Enhanced Machine Learning

<h1 align="center"> <img src="https://readme-typing-svg.herokuapp.com/?font=Righteous&size=35&center=true&vCenter=true&width=900&height=100&duration=4000&lines=OCR+with+WSL+for+Enhanced+Machine+Learning!+🚀;" /> </h1>

![Screenshot 2024-10-22 205817](https://github.com/user-attachments/assets/aff6d84e-19a3-48bf-81a2-8aa66191ec57)

## Introduction

To enhance your machine learning capabilities on Windows, especially for OCR tasks, this guide provides a setup to use **WSL2 (Windows Subsystem for Linux)**. If you're facing GPU acceleration issues or out-of-memory errors, it's recommended to switch to WSL, as TensorFlow no longer supports native Windows GPU installations beyond version 2.10. From TensorFlow 2.11 onward, users must install TensorFlow in WSL2, or alternatively, use `tensorflow-cpu` or the `TensorFlow-DirectML-Plugin`.

Follow the [TensorFlow installation guide](https://www.tensorflow.org/install/pip?_gl=1*1p24mar*_ga*MTg0NDgwOTAzNC4xNzI4NDMyMjQy*_ga_W0YLR4190T*MTcyODQzMjI0Mi4xLjEuMTcyODQzMjMwMS4wLjAuMA..#windows-native_1) or use this README to complete the installation.

# WSL2 Installation and Configuration Guide

This guide provides step-by-step instructions on how to install **WSL2** (Windows Subsystem for Linux) on your Windows machine, set it to use **D:** as storage, and configure it for use with Ubuntu.

## Prerequisites

- You need to be running **Windows 10 version 2004** and higher (Build 19041 and higher) or **Windows 11** to follow this guide.

---

## Step 1: Enable WSL and Install Ubuntu

1. Open **PowerShell** as Administrator:
   - Right-click on the **Start menu** and select **Windows PowerShell (Admin)**.

2. Run the following command to install **WSL** and enable **Ubuntu** as the default distribution:
   ```bash
   wsl --install
   ```

3. Restart your machine when prompted.

4. After restarting, Ubuntu will launch, and you'll be asked to create a **Linux user** and password. Follow the on-screen instructions to set up the user.

---

## Step 2: Set WSL Default Version to WSL 2

1. In **PowerShell** (run as Administrator), set WSL to default to **WSL 2**:
   ```bash
   wsl --set-default-version 2
   ```

---

## Step 3: Move WSL Installation to the D: Drive

By default, WSL stores its Linux distributions on the **C:** drive. Follow these steps to move it to the **D:** drive:

### 3.1 Export the WSL Distribution

1. First, create a backup folder on the **D:** drive:
   ```bash
   mkdir D:\wsl-backup
   ```

2. Export the Ubuntu distribution to the **D:** drive as a `.tar` file:
   ```bash
   wsl --export Ubuntu D:\wsl-backup\ubuntu.tar
   ```

### 3.2 Unregister the WSL Distribution

1. Unregister the Ubuntu distribution to remove it from the **C:** drive:
   ```bash
   wsl --unregister Ubuntu
   ```

### 3.3 Import the WSL Distribution to the D: Drive

1. Create a new folder for your WSL installation on the **D:** drive:
   ```bash
   mkdir D:\wsl\ubuntu
   ```

2. Import the WSL distribution to the **D:** drive:
   ```bash
   wsl --import Ubuntu D:\wsl\ubuntu D:\wsl-backup\ubuntu.tar --version 2
   ```

---

## Step 4: Set the Default WSL User

1. To set the default user (instead of logging in as `root`), run the following command in PowerShell:
   ```bash
   ubuntu config --default-user techarcanist
   ```
   Replace `techarcanist` with the user you set up during Ubuntu's first launch.

---

## Step 5: Verify WSL is Using the D: Drive

1. Launch **WSL** by typing `wsl` in **PowerShell** or **Command Prompt**.

2. In Ubuntu, navigate to the D: drive to check that WSL is running from there:
   ```bash
   cd /mnt/d/wsl/ubuntu
   ls
   ```
   You should see the `ext4.vhdx` file, which is the virtual hard disk storing your Ubuntu installation.

---

## Step 6: Use WSL Normally

You can now use **WSL** (Ubuntu) as usual. All operations and file storage will use the **D:** drive, freeing up space on your **C:** drive.

To launch **WSL** in the future, simply type:
```bash
wsl
```
in **PowerShell** or **Command Prompt**.

---

## Additional Resources

- [Microsoft's WSL Documentation](https://docs.microsoft.com/en-us/windows/wsl/)
- [Best practices for setting up a WSL development environment](https://docs.microsoft.com/en-us/windows/wsl/setup/environment)

---

This guide should help you install and configure WSL2, move its storage to a different drive, and set up a default user for smooth usage. If you encounter any issues, feel free to reach out for further assistance.

# CUDA & RAPIDS Setup on WSL2 for TensorFlow and RAPIDS AI

## Prerequisites

- **WSL2** with Ubuntu installed
- **NVIDIA GPU** with CUDA support
- **Docker** installed and running in WSL2

## 1. Install Python 3.12

Update your system and install Python 3.12 from the `deadsnakes` PPA:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.12
```

### 2. Create a Virtual Environment

Install the Python virtual environment package and create your environment:

```bash
sudo apt install python3.12-venv
python3.12 -m venv ~/myenv
source ~/myenv/bin/activate
```

### 3. Install pip

Ensure `pip` is installed and up-to-date within your environment:

```bash
python -m ensurepip --upgrade
```

## 4. Set Up CUDA on WSL2

Follow the [CUDA on WSL guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html) to set up CUDA. Ensure your GPU drivers and CUDA toolkit are installed correctly.

## 5. Install Docker and RAPIDS

Add your user to the `docker` group, run a test, and pull the RAPIDS Docker container:

```bash
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

Pull the RAPIDS AI Docker image:

```bash
docker pull rapidsai/rapidsai:22.08-cuda11.5-runtime-ubuntu20.04-py3.9
```

Run RAPIDS in the container with GPU support:

```bash
docker run --gpus all -it rapidsai/rapidsai:22.08-cuda11.5-runtime-ubuntu20.04-py3.9
```

To expose ports for Jupyter and Dask:

```bash
#exit or ctrl+D

docker run --gpus all -it -p 8888:8888 -p 8787:8787 -p 8786:8786 rapidsai/rapidsai:22.08-cuda11.5-runtime-ubuntu20.04-py3.9
```

## 6. Install NCCL for Multi-GPU Communication

Install NVIDIA's NCCL libraries:

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt update
```

Install the NCCL libraries:

```bash
sudo apt install libnccl2 libnccl-dev
```

Verify the installation:

```bash
dpkg -l | grep nccl
```

## 7. Install CUDA Toolkit

To install the CUDA toolkit, run the following commands:

```bash
sudo apt-key del 7fa2af80
sudo apt install cuda-toolkit-12-6
```

This installs CUDA Toolkit version 12.6.

## 8. Install TensorFlow with CUDA Support

Check if the GPU is detected:

```bash
nvidia-smi
```

Install TensorFlow with CUDA support in your virtual environment:

```bash
pip install --upgrade pip
pip install tensorflow[and-cuda]  # For GPU users
```

For CPU users, simply install TensorFlow:

```bash
pip install tensorflow
```

## 9. Verify TensorFlow Installation

### For CPU Verification

Run the following command to verify TensorFlow is installed correctly:

```bash
python3 -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```

If a tensor value is returned, the installation is successful.

### For GPU Verification

Run this command to check if TensorFlow detects the GPU:

```bash
python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

If a list of GPU devices is returned, the GPU installation was successful.

## 10. Install Jupyter Notebook
```bash
pip install notebook
```
### To access it

```bash
jupyter notebook
```
For more detailed instructions, visit:
- [NVIDIA CUDA on WSL](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)
- [NVIDIA AI Enterprise RAPIDS Guide](https://docs.nvidia.com/ai-enterprise/deployment-guide-vmware/0.1.0/docker.html)

--- 

## Running OCR with Trained Models
Once your setup is complete, check out the example notebook for the OCR implementation:
- [Trained by Lavitra Notebook](https://github.com/TechArcanist/OCR-with-WSL-for-Enhanced-Machine-Learning/blob/main/Trained%20by%20Lavitra.ipynb)

## Credits and Learning Resources
A big thanks to the tutorial on this [YouTube Channel](https://www.youtube.com/watch?v=-8a7j6EVjs0) that helped guide this project.

---

Feel free to tweak and enhance as needed! This setup guide ensures smooth transitions for anyone looking to enhance their machine learning workflow with OCR on WSL.
<h1 align="center">
    <img src="https://readme-typing-svg.herokuapp.com/?font=Righteous&size=35&center=true&vCenter=true&width=500&height=70&duration=4000&lines=Thanks+for+Visiting!+👋;" />
</h1>
