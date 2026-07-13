# gemma-4-31B

https://huggingface.co/google/gemma-4-31B

quay.io/redhat-ai-services/modelcar-catalog:gemma-4-31B

## Building Image

This ModelCar build downloads the model locally then copies the files to a container in multiple layers to avoid podman memory issues.

### Downloading model files locally

```
mkdir -p ./modelcar-images/google/gemma-4-31B/models
podman run --rm --platform linux/amd64 \
    -v ./modelcar-images/google/gemma-4-31B/models:/models \
    --env-file modelcar-images/google/gemma-4-31B/downloader.env \
    quay.io/redhat-ai-services/huggingface-downloader:latest
```

### Building the ModelCar Image

```
podman build modelcar-images/google/gemma-4-31B \
    -t quay.io/phison-hci/modelcar-catalog:gemma-4-31B  \
    --platform linux/amd64
```

## Deploying Model

This model can be deployed using vLLM on OpenShift AI using the following Helm Chart.

```
helm repo add redhat-ai-services https://redhat-ai-services.github.io/helm-charts/
helm repo update redhat-ai-services
helm upgrade -i gemma-4-31B redhat-ai-services/vllm-kserve \
    --values modelcar-images/google/gemma-4-31B/values.yaml
```

For more information on the above Helm Chart, you can find the source code for that chart here:

https://github.com/redhat-ai-services/helm-charts/tree/main/charts/vllm-kserve
