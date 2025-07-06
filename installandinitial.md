


install cuda and gpu driver and container runtime for gpu

        sudo ubuntu-drivers autoinstall
        sudo reboot
        
        
        nvidia-smi
        
        lsmod | grep nvidia
        
        nvidia-settings
        
        


What is nvcc for? If you only plan to use frameworks like PyTorch or TensorFlow and only run ready-made models (inference or training), you usually don't need nvcc. If you want to write your own CUDA code or build custom kernels, nvcc is required:

        sudo apt install nvidia-cuda-toolkit
        
        
        nvcc --version
        python3 -c "import torch; print(torch.cuda.is_available())"
        


Install NVIDIA Container Toolkit on Ubuntu (Step by Step)
Adding GPG key and NVIDIA repository:


    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
    curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
    sudo apt-get update
    
    Install nvidia-docker2 (includes container runtime):
    
    
    
    sudo apt-get install -y nvidia-docker2
    
    
    sudo systemctl restart docker

test it with:

which nvidia-container-runtime

run a container with gpu:


docker run --gpus all --rm nvidia/cuda:12.1.1-base-ubuntu22.04 nvidia-smi


If you see the output from the graphics card, everything is correct.





#### un ubuntu 24.04 i give an issue with installation about unsupported distro:

    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
    curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
    sudo apt-get update
    Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
    OK
    # Unsupported distribution!
    # Check https://nvidia.github.io/nvidia-docker
    Hit:1 http://ir.archive.ubuntu.com/ubuntu noble InRelease
    Hit:2 http://ir.archive.ubuntu.com/ubuntu noble-updates InRelease                                        
    Hit:3 http://ir.archive.ubuntu.com/ubuntu noble-backports InRelease                                      
    Hit:4 http://security.ubuntu.com/ubuntu noble-security InRelease                                                                                         
    Hit:5 https://dl.google.com/linux/chrome/deb stable InRelease                                                               
    Hit:6 https://download.docker.com/linux/ubuntu noble InRelease
    Reading package lists... Done
    root@zizi:~# 




it means: 

This means that at the stage of adding the repository, your Linux distribution (Ubuntu "noble" i.e. 23.04) is not yet officially supported by NVIDIA Container Toolkit.

Solutions:
Use a version closer to your distribution:

In the following command, the distribution variable is automatically taken as noble, which is for Ubuntu 23.04.

But NVIDIA only supports the following versions in this date normally:


ubuntu18.04

ubuntu20.04

ubuntu22.04

You can manually replace ubuntu22.04:


    curl -s -L https://nvidia.github.io/nvidia-docker/ubuntu22.04/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
    sudo apt-get update
    sudo apt-get install -y nvidia-docker2
    sudo systemctl restart docker


If you're still having trouble, you can manually install the nvidia-docker2 package from the Ubuntu 22.04 or 20.04 repositories instead of the official distribution (less risk of incompatibility, but doable).











