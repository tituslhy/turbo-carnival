# turbo-carnival
LiteLLM Exploration

This GitHub repository is a companion resource for an upcoming Medium article.

## Setup

### Helm Chart
Pull the LiteLLM Helm chart
```
helm pull oci://ghcr.io/berriai/litellm-helm
```

Unzip LiteLLM-Helm
```
tar -zxvf litellm-helm-0.1.2.tgz
```

Start the Minikube service
```
minikube start 
```

Install the LiteLLM helm chart
```
helm install lite-helm ./litellm-helm
```

Access application
```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=litellm,app.kubernetes.io/instance=lite-helm" -o jsonpath="{.items[0].metadata.name}")
export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
```

Edit your values.yaml file in the litellm-helm folder to include:
```
proxy_config:
    ... # other model deployments
    - model_name: llama3.1-8b
      litellm_params:
        model: ollama/llama3
        api_base: http://localhost:11434
```

## Spindown

### Helm Chart
Uninstall the helm chart
```
helm uninstall lite-helm
```

Stop minikube
```
minikube stop
```