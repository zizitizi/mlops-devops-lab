
### step 1

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
        





My new offline model in docker:

There are two popular and simple options for this:

🔹 Option 1 — LLaMA (Meta) or its derivatives like Ollama Pre-built and optimized models for GPU Relatively lightweight and accurate Tools like Ollama are very easy to install and use

🔹 Option 2 — Mistral or Mixtral (open models) Good quality Very suitable for troubleshooting and DevOps GPU (CUDA) support




💚 Using Ollama:


        docker run --gpus all -d --name ollama -p 11434:11434 ollama/ollama
        
        
        docker exec -it ollama ollama pull llama3


test it with :

    curl http://localhost:11434/api/generate -d '{
      "model": "llama3",
      "prompt": "hi.how are you?"
    }'




