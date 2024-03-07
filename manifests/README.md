## helm chart -> kustomize

The Red Hat Developer Hub Helm Chart is available at https://github.com/redhat-developer/rhdh-chart

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add backstage https://backstage.github.io/charts
helm repo add redhat-developer https://redhat-developer.github.io/rhdh-chart

helm template b4mad-racing-developer-hub redhat-developer/backstage \
  --values values.yaml \
  --namespace b4mad-racing-developer-hub \
  --output-dir rhdh
```

Then restore unwanted changes

```shell
git restore rhdh/backstage/charts/upstream/charts/postgresql/templates/secrets.yaml
git restore rhdh/backstage/templates/secrets.yaml
git restore rhdh/backstage/templates/tests/test-connection.yaml
```

## secrets

We are using [sealed secrets](https://sealed-secrets.netlify.app/),
a sops-encrypted version is kept to generate the sealed secrets and keep them human-readable.

```shell
sops --decrypt rhdh-secrets.enc.yaml \
| kubeseal --controller-namespace=sealed-secrets \
    --format yaml \
> rhdh-secrets.sealed.yaml
```

Make sure, that the input to `kubeseal` is using the correct namespace!


