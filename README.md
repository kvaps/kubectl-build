# kubectl build
*(formerly known as **kubectl-kaniko**)*

Kubectl build mimics the kaniko executor, but performs building on your Kubernetes cluster side.  
This allows you to simply build your local dockerfiles remotely without leaving your cozy environment.

![demo](https://gist.githubusercontent.com/kvaps/7d823b727a87d244d1f25deb5ff592da/raw/13062e62deb269f9385bc1c995382a589c34f04b/kubectl-build.gif)

## Installation

using [krew](https://krew.sigs.k8s.io/):

<pre>
kubectl krew index add kvaps <a href="https://github.com/kvaps/krew-index">https://github.com/kvaps/krew-index</a>
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

# Use cache building
kubectl build --context . --destination docker.io/some/image:latest --cache --cache-repo docker.io/some/cache

# Save image name and digest to file
kubectl build --context . --destination docker.io/some/image:latest --digest-file /tmp/digest --image-name-with-digest-file /tmp/image

# Build from stdin
tar -cvf- . | kubectl build --destination docker.io/some/image:latest --context tar://stdin
```
