# rootless_nginx_helm

## Summary:
[configmap.yaml](templates/configmap.yaml) contains 2 nginx files:
1. One that will be mounted to /etc/nginx.conf - "nginx.conf"
2. One that is used as a template - "nginx-http.conf-template"
   - The template is currently setup to have 8443 commented out, and 8080 is used.
   - The template should be adjusted to the configurations you need.

[deployment.yaml](templates/deployment.yaml) uses environment variables defined in [values.yaml](values.yaml)
- Example environment variable passed to container: `BACKEND_NGINX_*`
- When container starts, it will run `envsubst` command against the nginx template, and it'll generate a new nginx file that will be used in the final start of nginx.

## Helm structure
```
├── Chart.yaml - helm chart
├── templates
│   ├── configmap.yaml - contains /etc/nginx.conf and another nginx config file used as a template
│   ├── deployment.yaml - creates final nginx config based off of configmap template and values.yaml
│   ├── _helpers.tpl
│   ├── ingress.yaml - entrypoint to service
│   └── service.yaml - entrypoint to container (http=8080, and https=8443)
└── values.yaml - helm configurations
```


## Benefits
- No Dockerfile/Containerfile needed.
- Structured to be a template with many values-x.yaml files for environment separation.
- Designed with UBI (Red Hat Enterprise Linux 9) in mind.
- Runs on a rootless environment, such as OpenShift.


