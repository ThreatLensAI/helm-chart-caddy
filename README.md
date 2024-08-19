# Helm Chart to deploy static-site using Caddy

This repository contains Helm chart designed to deploy a static site hosted using the Caddy web server. It includes configurations for creating a ConfigMap, a Pod, and a Secret to manage sensitive information.

## Helm Installation

Please follow the installation instructions required for setting up the project [here](INSTALLATION.md).

## Chart Structure

The chart contains the following key files:

- `Chart.yaml`: Contains metadata for the chart.
- `values.yaml`: Contains default values for the chart.
- `templates/config-map.yaml`: Defines a ConfigMap for storing configuration data.
- `templates/pod.yaml`: Defines a Pod with the necessary specifications and probes for health checks.
- `templates/secrets.yaml`: Defines a Secret for storing DockerHub credentials.

## Installation

To install the chart with default variables set in `values.yaml`, helm install requires dockerhubcredentials to be provided.

This can be provided in following two ways:

1. To install the chart with the release name `static-site` in custom namespace `caddy` the following command can be used:

    ```bash
    export DOCKERHUBCREDENTIALS=$(cat ~/.docker/config.json | base64 -w 0)
    helm install static-site . --set secrets.dockerhub=${DOCKERHUBCREDENTIALS} --create-namespace --namespace caddy
    ```

2. Update the `values.yaml` file variable:

    ```yaml
    secrets:
        dockerhub: <base64 encoded dockerhub config/credentials>
    ```

    and then run the following command:

    ```bash
    helm install static-site .
    ```

    This will install the chart with the default values set in `values.yaml` in the `default` namespace.

## Uninstallation

To uninstall the chart, use the following command:

```bash
helm uninstall static-site
```

In case the chart was installed in a custom namespace, use the following command:

```bash
helm uninstall static-site -n caddy
```

## Configuration

The following table lists the configurable parameters of the Helm chart and their default values.

\* - Required

| Parameter                             | Description                               | Default                  |
| ------------------------------------- | ----------------------------------------- | ------------------------ |
| `secrets.dockerhub`*                  | DockerHub secret for pulling images       | `''`                     |
| `pod.image.name`*                     | Name of the Docker image                  | `marlapativ/static-site` |
| `pod.image.tag`*                      | Tag of the Docker image                   | `jenkins-static-site-1`  |
| `pod.imagePullPolicy`                 | Image pull policy                         | `IfNotPresent`           |
| `pod.port`*                           | Port on which the pod will run            | `8080`                   |
| `pod.readinessProbe.path`             | Path for the readiness probe              | `/`                      |
| `pod.readinessProbe.failureThreshold` | Failure threshold for the readiness probe | `6`                      |
| `pod.readinessProbe.successThreshold` | Success threshold for the readiness probe | `2`                      |
| `pod.readinessProbe.periodSeconds`    | Period in seconds for the readiness probe | `10`                     |
| `pod.livenessProbe.path`              | Path for the liveness probe               | `/`                      |
| `pod.livenessProbe.failureThreshold`  | Failure threshold for the liveness probe  | `10`                     |
| `pod.livenessProbe.successThreshold`  | Success threshold for the liveness probe  | `1`                      |
| `pod.livenessProbe.periodSeconds`     | Period in seconds for the liveness probe  | `10`                     |
| `pod.startupProbe.path`               | Path for the startup probe                | `/`                      |
| `pod.startupProbe.failureThreshold`   | Failure threshold for the startup probe   | `30`                     |
| `pod.startupProbe.periodSeconds`      | Period in seconds for the startup probe   | `10`                     |
