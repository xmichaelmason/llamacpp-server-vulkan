# llama.cpp vulkan server
This docker compose file defines a llama.cpp service running with vulkan drivers. This enables GPU support for GPUs not supported by current AI stack drivers, such as AMD iGPUs.

Workarounds to enable ROCm support on AMD iGPUs exist, however current implementations only utilize GTT memory, which performs no better than CPU inference.

Vulkan drivers on an AMD 780m perform nearly 50% faster on llama-3.1-8b-instruct than applying the ROCm workaround to enable GFX1103 support (and CPU inference).

- update volumes:llama-storage:device to reflect your local models folder
- update LLAMA_ARG_MODEL to reflect model name
- update LLAMA_ARG_CTX_SIZE to adjust memory usage

### fedora
```sh
install vulkan-devel podman-docker podman-compose
```
