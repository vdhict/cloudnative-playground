# Introduction to FluxCD v2

## Create Demo Cluster

Using [k3d](https://k3d.io/) </br>

```
k3d cluster create fluxcd
```
## Run a container to work in

### Run Alpine Linux

```
docker run -it --rm -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
```

### Install Tools

``` bash
# install curl
apk add --no-cache curl

# install kubectl 
curl -sLO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
```

### test cluster access:
``` bash
/work # kubectl get nodes
NAME                   STATUS   ROLES           AGE   VERSION
fluxcd-control-plane   Ready    control-plane   9h    v1.27.3
```

## Install Flux CLI

Download the `flux` command-line utility. </br>
We can get this utility from the GitHub [Releases page](https://github.com/fluxcd/flux2/releases) </br>

It's also worth noting that you want to ensure you get a compatible version of flux which supports your version of Kubernetes. Checkout the [prerequisites](https://fluxcd.io/flux/installation/#prerequisites) page. </br>

``` bash
curl -o /tmp/flux.tar.gz -sLO https://github.com/fluxcd/flux2/releases/download/v2.2.3/flux_2.2.3_linux_amd64.tar.gz
tar -C /tmp/ -zxvf /tmp/flux.tar.gz
mv /tmp/flux /usr/local/bin/flux
chmod +x /usr/local/bin/flux
```

## Check cluster

``` bash
/work # flux check --pre
► checking prerequisites
✔ Kubernetes 1.27.3 >=1.26.0-0
✔ prerequisites checks passed
```

## Documentation

Flux [Core Concepts](https://fluxcd.io/flux/concepts/)

Follow the steps under the [bootstrap](https://fluxcd.io/flux/installation/#bootstrap) section for GitHub </br>

Generate a [personal access token (PAT)](https://github.com/settings/tokens/new) that can create repositories by checking all permissions under `repo`.  </br>

Set Github token variable

``` bash
export GITHUB_TOKEN=<gh_token>
```

``` bash
flux bootstrap github \
  --token-auth \
  --owner=vdhict \
  --repository=cloudnative-playground \
  --path=kubernetes/fluxcd/repositories/clusters/dev-cluster \
  --personal \
  --force
