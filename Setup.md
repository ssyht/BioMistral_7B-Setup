### You need to launch the Ollama server first before you can talk to it.

**Step 1: Start the Ollama Container**

Run this command in your terminal to start the server. This command mounts your GPU (--gpus all) and your data volume so the model can run efficiently.

```bash
docker run -d --gpus all \
  -v /data/ollama:/root/.ollama \
  -p 11434:11434 \
  --name ollama \
  --restart always \
  ollama/ollama
```

