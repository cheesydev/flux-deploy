## Install Flux

Generate the initial manifests using `fluxctl install` but do not apply them
yet.

```
fluxctl install \
  --git-user=cheesybot \
  --git-email=bot@cheesy.dev \
  --git-url=git@github.com:cheesydev/flux-deploy \
  --git-path=workloads \
  --namespace=rafael-flux > flux.yaml
```

My user account (rafael.portela@container-solutions.com) does not have
permissions to create cluster roles and bindings in our shared CS cluster. So
i pushed a change extending rolebinding to a new service account that will be
used by flux.

This service account does not have permission to patch secrets. This would
happen at start up if there's no SSH provided, making Flux create a new pair.

To go around this, will just create a new SSH key pair and then, include it in
a secret.

```
# create the keys (you'll be prompted for path and key name)
ssh-keygen

kubectl create secret generic flux-git-deploy \
  --from-file=identity=/path/to/private_key
```

Then, install Flux.
```
kubectl apply -f install/flux.yaml
```

Finally, don't forget to add the public key in your repo settings to give Flux
permission to it. You can also get the key value with `fluxctl identity`.
```
fluxctl identity --k8s-fwd-ns rafael-flux
```
