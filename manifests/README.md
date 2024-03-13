### janus

The Janus showcase is available at https://github.com/redhat-developer/rhdh-chart

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add backstage https://backstage.github.io/charts
helm repo add redhat-developer https://redhat-developer.github.io/rhdh-chart

helm template b4mad-racing-developer-hub redhat-developer/backstage \
  --values values.yaml \
  --namespace b4mad-racing-developer-hub \
  --output-dir helm/janus
```

Then restore unwanted changes

```shell
git restore helm/janus/backstage/charts/upstream/charts/postgresql/templates/secrets.yaml
git restore helm/janus/backstage/templates/secrets.yaml
git restore helm/janus/backstage/templates/tests/test-connection.yaml
```

### rhdh

```
helm repo add openshift-helm-charts https://charts.openshift.io/
helm show values openshift-helm-charts/redhat-developer-hub > values-rhdh.yaml

helm template b4mad-racing-developer-hub openshift-helm-charts/redhat-developer-hub \
  --values values-downstream.yaml \
  --namespace b4mad-racing-developer-hub \
  --output-dir helm/rhdh

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


