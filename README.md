# kubectl-kaniko

Kaniko wrapper for kubectl.
*(inspired by [GoogleContainerTools/kaniko#1289](https://github.com/GoogleContainerTools/kaniko/pull/1289))*

This plugin allows you to simple build your dockerfiles directly in your Kubernetes cluster.

![demo](https://gist.githubusercontent.com/kvaps/7d823b727a87d244d1f25deb5ff592da/raw/1c4c3e5d9c7a0666ba44631250411b4d9123c802/kubectl-kaniko.gif)

## Installation

```bash
curl -LO https://github.com/kvaps/kubectl-kaniko/raw/master/kubectl-kaniko
chmod +x ./kubectl-kaniko
sudo mv ./kubectl-kaniko /usr/local/bin/kubectl-kaniko
```

## Usage

```bash
kubectl kaniko [args]
```

## Examples

```bash
# Show all kaniko commands
kubectl kaniko --help

# Build from current directory
kubectl kaniko --context . --no-push

# Specify namespace and kubeconfig
kubectl kaniko --context . --no-push --namespace default --kubeconfig ~/.kube/someconfig

# Login to remote registry
docker login docker.io

# Sort form
kubectl kaniko -c . -d docker.io/some/image:latest

# Use cache building
kubectl kaniko --context . --destination docker.io/some/image:latest --cache-repo docker.io/some/cache

# Save image name and digest to file
kubectl kaniko --context . --destination docker.io/some/image:latest --digest-file /tmp/digest --image-name-with-digest-file /tmp/image

# Build from stdin
tar -cvf- . | kubectl kaniko --destination docker.io/some/image:latest --context tar://stdin
```
