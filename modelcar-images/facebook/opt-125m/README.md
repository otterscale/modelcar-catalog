# opt-125m

https://huggingface.co/facebook/opt-125m

quay.io/redhat-ai-services/modelcar-catalog:opt-125m

## Building Image

This ModelCar build downloads the model locally then copies the files to a container in multiple layers to avoid podman memory issues.

### Downloading model files locally

```
mkdir -p ./modelcar-images/facebook/opt-125m/models
podman run --rm --platform linux/amd64 \
    -v ./modelcar-images/facebook/opt-125m/models:/models \
    --env-file modelcar-images/facebook/opt-125m/downloader.env \
    quay.io/redhat-ai-services/huggingface-downloader:latest
```

### Building the ModelCar Image

```
podman build modelcar-images/facebook/opt-125m \
    -t quay.io/phison-hci/modelcar-catalog:opt-125m  \
    --platform linux/amd64
```

## Deploying Model

This model can be deployed using vLLM on OpenShift AI using the following Helm Chart.

```
helm repo add redhat-ai-services https://redhat-ai-services.github.io/helm-charts/
helm repo update redhat-ai-services
helm upgrade -i opt-125m redhat-ai-services/vllm-kserve \
    --values modelcar-images/facebook/opt-125m/values.yaml
```

For more information on the above Helm Chart, you can find the source code for that chart here:

https://github.com/redhat-ai-services/helm-charts/tree/main/charts/vllm-kserve
