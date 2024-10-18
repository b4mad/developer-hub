### Install the GitHub App

```shell
npx @backstage/cli create-github-app b4mad
```

Callback URL: https://rhdh.b4mad.racing/api/auth/github/handler/frame


### Kubernetes

Get the token for the service account backstage-read-only for the env var K8S_CLUSTER_TOKEN
```shell
kubectl -n b4mad-developer-hub get secret backstage-read-only -o go-template='{{.data.token | base64decode}}'
```