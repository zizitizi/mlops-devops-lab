
### step 1
My new offline model in docker


docker run --gpus all -d --name ollama -p 11434:11434 ollama/ollama


docker exec -it ollama ollama pull llama3


test it with :

    curl http://localhost:11434/api/generate -d '{
      "model": "llama3",
      "prompt": "hi.how are you?"
    }'




