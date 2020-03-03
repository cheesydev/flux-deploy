## Install Flux

```
fluxctl install \
  --git-user=cheesybot \
  --git-email=bot@cheesy.dev \
  --git-url=git@github.com:cheesydev/flux-deploy \
  --git-path=namespaces,workloads \
  --namespace=rafael-flux > install.yaml
```

