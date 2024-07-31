# Getting Started With Kubernetes and Container Orchestration Notes

The purpose of this example is to provide instructions for running the Dockercoins sample app using Kind.

## Software Requirements

- Docker Desktop for Mac 4.28.0 or newer

- Kubernetes 1.20.2

- KinD 0.23.0 or newer

## Create Cluster

```zsh
kind create cluster --name dockercoins --config ./kind.yaml
```

## Create Necessary Environment Variables

```zsh
export KUBECONFIG="$(kind get kubeconfig --name='dockercoins')"
export REGISTRY=dockercoins
export TAG=v0.1
```

## Create Redis Deployment

```zsh
kubectl create deployment redis --image=redis
```

## Create Other Deployments

```zsh
for SERVICE in hasher rng webui worker; do
  kubectl create deployment $SERVICE --image=$REGISTRY/$SERVICE:$TAG
done
```

## Create Redis, Rng, And Hasher Services Using ClusterIP Type

```zsh
kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80
```

## Create WebUI Service Using NodePort Type

```zsh
kubectl create service nodeport webui --node-port=30080 --tcp=8082:80
```

## Navigate To WebUI Service In The Browser

```zsh
open http://localhost:8082
```

## Scaling The Worker Service

```zsh
kubectl scale deploy/worker --replicas=10
```

## Teardown Cluster

```zsh
kind delete cluster --name dockercoins
```

## References

- https://github.com/kubernetes-sigs/kind

- https://training.play-with-kubernetes.com/kubernetes-workshop

## Support

Bug reports and feature requests can be filed with the rest for the Phoenix project here:

- [File Bug Reports and Features](https://github.com/conradwt/dockercoins-using-kind/issues)

## License

Dockercoins Using Kind is released under the [MIT license](./LICENSE.md).

## Copyright

copyright:: (c) Copyright 2020 - 2024 Conrad Taylor. All Rights Reserved.
