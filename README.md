# Getting Started With Kubernetes and Container Orchestration Notes

The purpose of this example is to provide instructions for running the Dockercoins sample app using Kind.

## create cluster

```zsh
kind create cluster --name dockercoins --config ./kind-config.yaml
```

## create necessary environment variables

```zsh
export KUBECONFIG="$(kind get kubeconfig --name='dockercoins')"
export REGISTRY=dockercoins
export TAG=v0.1
```

## create Redis deployment

```zsh
kubectl create deployment redis --image=redis
```

## create other deployments

```zsh
for SERVICE in hasher rng webui worker; do
  kubectl create deployment $SERVICE --image=$REGISTRY/$SERVICE:$TAG
done
```

## create redis, rng, and hasher services using ClusterIP type

```zsh
kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80
```

## create webui service using NodePort type

```zsh
kubectl create service nodeport webui --node-port=30080 --tcp=8082:80
```

## navigate to webui service in the browser

```zsh
open http://localhost:8082
```

## Scaling The Worker Service

```zsh
kubectl scale deploy/worker --replicas=10
```

## teardown cluster

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

Dockercoins Using Kind is released under the [MIT license](https://mit-license.org).

## Copyright

copyright:: (c) Copyright 2020 Conrad Taylor. All Rights Reserved.
