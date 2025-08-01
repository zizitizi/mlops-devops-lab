
## step 1: run an ai model in your local

after install nvidea toolkit for container runtime do this.

to use gpu in docker runtime use this code in docker daemon:


nano /etc/docker/daemon.json


    {
        "runtimes": {
            "nvidia": {
                "path": "nvidia-container-runtime",
                "runtimeArgs": []
            }
        }
    }



My new offline model in docker:

There are two popular and simple options for this:

🔹 Option 1 — LLaMA (Meta) or its derivatives like Ollama Pre-built and optimized models for GPU Relatively lightweight and accurate Tools like Ollama are very easy to install and use

🔹 Option 2 — Mistral or Mixtral (open models) Good quality Very suitable for troubleshooting and DevOps GPU (CUDA) support




💚 Using Ollama:


        docker run --gpus all -d --name ollama -p 11434:11434 ollama/ollama
        
        
        docker exec -it ollama ollama pull llama3


or using docker compose


first create ollama network


docker network create ollama-net



nano docker-compose.yml


    services:
      ollama:
        image: ollama:latest
        container_name: ollama
        ports:
          - "11434:11434"
        volumes:
          - ollama_data:/root/.ollama
        restart: always
        networks:
           - ollama-net
        deploy: {}
        runtime: nvidia
        environment:
          - NVIDIA_VISIBLE_DEVICES=all
          - NVIDIA_DRIVER_CAPABILITIES=compute,utility
    volumes:
      ollama_data:
    networks:
       ollama-net:



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


## step 2: web ui to chat with ai model

### Building a simple chat client with jq

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



### Building a simple chat client with ollama-webui



nano docker-comppose.yml

    services:
      ollama-webui:
        image: ghcr.io/ollama-webui/ollama-webui:latest
        ports:
           - "3000:8080"
        environment:
           - OLLAMA_API_BASE_URL=http://ollama:11434/api
    
        networks:
          - ollama-net
        restart: always
    networks:
       ollama-net:
         external: true


for environment hit to below command:

http://ollama:11434/tags 

http://ollama:11434/api/tags

if first line respone then this line change to:

 - OLLAMA_API_BASE_URL=http://ollama:11434

after everything be ok then open the ai chat in browser:

http://localhost:3000/


in next step will be to how tune the ai chat for t-shoot and devops.

enjoy it


### Building a profesional customize chat web ui client with open-webui

i run that in my docker to keep my system clean then you can do it like me. after mkdir a folder then copy and pase and run it:

https://github.com/open-webui/open-webui/blob/main/docker-compose.yaml


    services:
      ollama:
        volumes:
          - ollama:/root/.ollama
        container_name: ollama
        pull_policy: always
        tty: true
        restart: unless-stopped
        image: ollama/ollama:${OLLAMA_DOCKER_TAG-latest}
    
      open-webui:
        build:
          context: .
          args:
            OLLAMA_BASE_URL: '/ollama'
          dockerfile: Dockerfile
        image: ghcr.io/open-webui/open-webui:${WEBUI_DOCKER_TAG-main}
        container_name: open-webui
        volumes:
          - open-webui:/app/backend/data
        depends_on:
          - ollama
        ports:
          - ${OPEN_WEBUI_PORT-3000}:8080
        environment:
          - 'OLLAMA_BASE_URL=http://ollama:11434'
          - 'WEBUI_SECRET_KEY='
        extra_hosts:
          - host.docker.internal:host-gateway
        restart: unless-stopped
    
    volumes:
      ollama: {}
      open-webui: {}
    


  just enjoy it! after all dont worry about internet disconnection and chat limit!!! :) 
  














