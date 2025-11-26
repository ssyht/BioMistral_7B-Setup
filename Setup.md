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

**Step 2: Pull & Run the Model**

Once the container is running (it takes about 2 seconds to start), retry the command you were using:

```bash
docker exec -it ollama ollama run cniongolo/biomistral
```

What you will see: It will start downloading the BioMistral model layers (this might take a few minutes).

**Success**: Once finished, you will see a >>> prompt. You can then start typing your medical questions to test the raw model.

