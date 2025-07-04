
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


or using docker compose

nano docker-compose.yml


    version: '3.8'
    
    services:
      ollama:
        image: ollama/ollama:latest
        container_name: ollama
        ports:
          - "11434:11434"
        volumes:
          - ollama_data:/root/.ollama
        restart: always
    
    volumes:
      ollama_data:


docker compose up -d


docker ps


docker exec -it ollama ollama pull llama3


to save your image as your library image

docker exec -it ollama ollama pull llama3



docker commit ollama ollama





test it with :

        curl http://localhost:11434/api/generate -d '{
          "model": "llama3",
          "prompt": "hi.how are you?"
        }'
    


curl http://localhost:11434


or in browser 

http://localhost:11434

now its ready

Building a simple chat client with jq

apt install jq



nano chat.sh


    
    #!/bin/bash
    echo "Type your prompt (Ctrl+C to exit)"
    while true; do
      read -p "> " prompt
      curl -s -X POST http://localhost:11434/api/generate -d "{\"model\": \"llama3\", \"prompt\": \"$prompt\"}" | jq -r '.response'
    done
    


chmod +x chat.sh



./chat.sh


