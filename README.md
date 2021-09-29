# Kubectl build

Kubectl build mimics the kaniko executor, but performs building on your Kubernetes cluster side.  
This allows you to simply build your local dockerfiles remotely without leaving your cozy environment.

![demo](https://gist.githubusercontent.com/kvaps/7d823b727a87d244d1f25deb5ff592da/raw/13062e62deb269f9385bc1c995382a589c34f04b/kubectl-build.gif)

## Installation

using [krew](https://krew.sigs.k8s.io/):

<pre>
kubectl krew index add kvaps https://github.com/kvaps/krew-index
kubectl krew install kvaps/build
</pre>

or using curl:

```bash
curl -LO https://github.com/kvaps/kubectl-build/raw/master/kubectl-build
chmod +x ./kubectl-build
sudo mv ./kubectl-build /usr/local/bin/kubectl-build
```

## Usage

```bash
kubectl build [args]
```

## Examples

```bash
# Show all kaniko commands
kubectl build --help

# Build from current directory
kubectl build --context . --no-push

# Specify namespace and kubeconfig
kubectl build --context . --no-push --namespace default --kubeconfig ~/.kube/someconfig

# Login to remote registry
docker login docker.io

# Short form
kubectl build -c . -d docker.io/some/image:latest

# Run debug shell
kubectl build -c . --no-push --debug

# Use cache building
kubectl build --context . --destination docker.io/some/image:latest --cache --cache-repo docker.io/some/cache

# Save image name and digest to file
kubectl build --context . --destination docker.io/some/image:latest --digest-file /tmp/digest --image-name-with-digest-file /tmp/image

# Build from stdin
tar -cvf- . | kubectl build --destination docker.io/some/image:latest --context tar://stdin
```

## Extra configuration

While standard behavior of kubectl-build plugin intend to repeat kaniko executor options. The additional configuration can be specified by setting environment variables.

This may be useful for both having permanent configuration and setting CI-systems.

| Enivroment Variable                | Description                                                                    | Default value                    |
|------------------------------------|--------------------------------------------------------------------------------|----------------------------------|
| `KUBECTL_BUILD_CONTEXT`            | Kubernetes context for creating pod (may be overriden by `--kubecontext`)      | current context                  |
| `KUBECTL_BUILD_DOCKER_CONFIG`      | Path to dockerconfig file to forward                                           | `~/.docker/config.json`          |
| `KUBECTL_BUILD_IMAGE`              | Kaniko-executor image                                                          | `gcr.io/kaniko-project/executor` |
| `KUBECTL_BUILD_KEEP_POD`           | If set to `true` do not delete pod after finising process                      | `false`                          |
| `KUBECTL_BUILD_KUBECONFIG`         | Path to kubeconfig file for creating pods (may be overriden by `--kubeconfig`) | kubectl defaults                 |
| `KUBECTL_BUILD_METADATA_OVERRIDES` | Json patch to override metadata for creating pods                              | `{}`                             |
| `KUBECTL_BUILD_NAME_OVERRIDE`      | Name for creating pod                                                          | `kaniko-rand6n`                  |
| `KUBECTL_BUILD_NAMESPACE`          | Kubernetes namespace for creating pod (may be overriden by `--namespace`)      | current namespace                |
