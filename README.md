# Helm Cheat Sheet

This README serves as a comprehensive guide and cheat sheet for working with Helm, a package manager for Kubernetes.

## Installation

To install Helm, follow the instructions [here](https://helm.sh/docs/intro/install/).

## Adding and Managing Repositories

### Adding a Repository

```bash
helm repo add <repo_name> <repo_link>
```

For example:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### Listing Repositories

To see added Helm repositories:

```bash
helm repo list
```

### Removing a Repository

```bash
helm repo remove <repo_name>
```

## Searching for Charts

### Searching on Repositories

```bash
helm search repo <keyword>
```

For example, to search for MySQL:

```bash
helm search repo mysql
```

### Searching on Artifact Hub

```bash
helm search hub <keyword>
```

## Installing and Managing Charts

### Installing a Chart

```bash
helm install <release_name> <repo_name>/<chart_name>
```

For example:

```bash
helm install my-redis bitnami/redis --version 18.4.0
```

### Uninstalling a Chart

```bash
helm uninstall <release_name>
```

### Reusing Names with Different Versions

To reuse the same name for a Helm deployment with a different version, create another namespace:

```bash
kubectl create namespace <ns>
helm install -n <ns> <release_name> <repo_name>/<chart_name> --version <ver>
```

### Regenerating Output for Previous Deployments

```bash
helm status <helm_dep_name> -n <ns>
```

### Customizing Input for Charts

```bash
helm install <release_name> --set <key1>=<v1>,<key2>=<v2> <repo_name>/<chart_name>
```

Alternatively, use a YAML file for custom values:

```bash
helm install <release_name> --value <path_to_yaml_file> <repo_name>/<chart_name>
```

### Managing Custom Values

To see custom values used for installing a chart:

```bash
helm get values <helm_dep_name> -n <ns>
```

### Updating Repositories

```bash
helm repo update
```

### Upgrading a Release

```bash
helm upgrade -n <ns> <release_name> --value <path_to_new_yaml_file> <repo_name>/<chart_name> --version <new_ver>
```

### Keeping Revision History

```bash
helm uninstall <release_name> -n <ns> --keep-history
```

### Rollback

```bash
helm rollback <release_name> <revision_number> -n <ns>
```

## Chart Development

### Creating a Chart

```bash
helm create <chart_name>
```

### Packaging a Chart

```bash
helm package <path_to_custom_chart_folder>
```

### Validating Charts

```bash
helm lint <path_to_custom_chart_folder>
helm template <path_to_custom_chart_folder>
helm install <release_name> <path_to_custom_chart_folder> --dry-run
```

### Managing Dependencies

To install other charts as dependencies:

1. Add dependencies in `Chart.yaml`.
2. Use `helm dependency update <path_to_custom_chart_folder>` to fetch dependencies.

### Conditional Dependencies

Add conditional dependencies in `values.yaml` and `Chart.yaml`.

### Customizing Dependencies

Use values from dependency charts in your custom chart by adding `import-values` in `Chart.yaml`.

## Conclusion

This README.md serves as a comprehensive guide and cheat sheet for Helm usage and management. For more detailed information, refer to Helm's official documentation.