# Home Assistant Chart with Traefik Ingress
Houses Home Assistant Helm umbrella chart that includes an ingress route for those using Traefik.

## Installation
1. Install Home Assistant. Folow the instructions below (assuming the command is ran at {git_repo_root}/home-assistant). Change any values you need in the values file based on [charts default values file](https://artifacthub.io/packages/helm/helm-hass/home-assistant) under `home-assistant`
```bash
helm upgrade --install home-assistant . \
    --namespace home-assistant \
    --create-namespace \
    --set ingressRoute.url=olivetin.example.com \
    --atomic
```