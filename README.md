# kubectl kaniko plugin

Simple kaniko wrapper for kubectl.
*(inspired by [GoogleContainerTools/kaniko#1289](https://github.com/GoogleContainerTools/kaniko/pull/1289))*

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

# Specify namespace and build from current directory
kubectl kaniko --context=. --namespace builds  --no-push

# Use cache building
kubectl kaniko --context=. --destination docker.io/some/image:latest --cache-repo docker.io/some/image-cache

# Build from stdin
tar -cvf- . | kubectl kaniko --destination docker.io/some/image:latest --context=tar://stdin
```
