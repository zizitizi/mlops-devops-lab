


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
        










