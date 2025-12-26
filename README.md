# llama.cpp Vulkan Server

This repository contains a `docker-compose.yml` file that defines a `llama.cpp` service running with Vulkan drivers. This setup enables GPU support for GPUs not supported by the current AI stack drivers, such as AMD iGPUs.

## Why Vulkan?

Workarounds to enable ROCm support on AMD iGPUs exist [Ollama on 780m](https://github.com/xmichaelmason/ollama-amd-780m), but current implementations only utilize GTT memory, which performs no better than CPU inference. Vulkan drivers on an AMD 780m perform nearly 50% faster on `llama-3.1-8b-instruct` than applying the ROCm workaround to enable GFX1103 support (and CPU inference).

## Prerequisites

Ensure you have the following installed on your system:
- `vulkan-devel`
- `podman-docker` (or docker engine)
- `podman-compose`

For Fedora, you can install these packages using:
```sh
sudo dnf install vulkan-devel podman-docker podman-compose
```

## Configuration

Before running the service, update the following fields in the `docker-compose.yml` file:

1. **Model Storage Path**
   Update the `volumes:llama-storage:device` field to reflect the path to your local models folder. For example:
   ```yml
   volumes:
     llama-storage:
       driver: local
       driver_opts:
         type: none
         device: /path/to/your/models
         o: bind
   ```

2. **Model Name**
   Update the `LLAMA_ARG_MODEL` environment variable to reflect the name of the model you want to use. For example:
   ```yml
   environment:
     - LLAMA_ARG_MODEL=/models/your-model-name.gguf
   ```

3. **Context Size**
   Adjust the `LLAMA_ARG_CTX_SIZE` environment variable to control memory usage. For example:
   ```yml
   environment:
     - LLAMA_ARG_CTX_SIZE=16384
   ```

4. **API Key**
   Replace the placeholder `API_KEY=n` with your actual API key if required.

## Running the Service

To start the service, run the following command in the directory containing the `docker-compose.yml` file:
```sh
podman-compose up -d
```
This will start the `llama.cpp` server in detached mode.

## Accessing the Service

The service will be accessible at `http://0.0.0.0:8000` by default. You can modify the port mapping in the `docker-compose.yml` file if needed:
```yml
ports:
  - "8000:8000"
```

## Notes

- Vulkan drivers provide significant performance improvements for certain GPUs, such as AMD iGPUs.
- Ensure that the Vulkan drivers are properly installed and configured on your system.
- The Web UI is enabled by default. To disable it, set the `LLAMA_ARG_NO_WEBUI` environment variable to `true` in the `docker-compose.yml` file.

For further details, refer to the comments in the `docker-compose.yml` file.
