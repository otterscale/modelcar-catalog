# qwen3.8-27b-fp8

https://huggingface.co/Qwen/Qwen3.8-27B-FP8

harbor.phison.com/ghcr-mirror/phison-ai/modelcar-catalog:qwen3.8-27b-fp8

## Building Image

This ModelCar build downloads the model locally then copies the files to a container in multiple layers to avoid podman memory issues.

The model is about 31 GB. Keep at least 70 GB free for the download plus the image.

### Downloading model files locally

```
mkdir -p ./modelcar-images/qwen/qwen3.8-27b-fp8/models
podman run --rm --platform linux/amd64 \
    -v ./modelcar-images/qwen/qwen3.8-27b-fp8/models:/models:Z \
    --env-file modelcar-images/qwen/qwen3.8-27b-fp8/downloader.env \
    quay.io/redhat-ai-services/huggingface-downloader:latest
```

### Building the ModelCar Image

```
podman build modelcar-images/qwen/qwen3.8-27b-fp8 \
    -t harbor.phison.com/ghcr-mirror/phison-ai/modelcar-catalog:qwen3.8-27b-fp8 \
    --platform linux/amd64
```

### Pushing to Harbor

```
podman login harbor.phison.com
podman push harbor.phison.com/ghcr-mirror/phison-ai/modelcar-catalog:qwen3.8-27b-fp8
```

## Using in a Phison HCI release

The tag must equal the lowercased last segment of the `hf://` URI in the LLMInferenceServiceConfig (`hf://Qwen/Qwen3.8-27B-FP8` -> `qwen3.8-27b-fp8`), because the dashboard rewrites it to `models/modelcar-catalog:<tag>`.

List it in `release.yaml` as `ghcr.io/phison-ai/modelcar-catalog:qwen3.8-27b-fp8`, then run the Model Weight Release workflow with `harbor_only=true`.
