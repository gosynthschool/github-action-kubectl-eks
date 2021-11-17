# kubectl for EKS Github Action

Slim wrapper around kubectl Docker image

## Options

This action supports the following options.

### exec

The command to execute inside the Docker image.

* *Required*: `Yes`
* *Type*: `string`
* *Example*: `kubectl version`

## kubeconfig

The contents of the `~/.kube/config` used by kubectl and helm to authenticate and communicate with your kubernetes
cluster. *Note: this can be empty if you want to use this action to do helm lints. The contents of this input will
be appended to `~/.kube/config`, and will always be removed afterwards.*

* *Required*: `no`
* *Type*: `string`

## Examples

The following example is triggered on the tagging of a new release and apply the yaml to kubernetes:

```yaml
name: Deploy
on:
  release:
    types: [created]
jobs:
  deployment:
    runs-on: 'ubuntu-latest'
    steps:
      # checkout the code
      - name: Checkout code
        uses: actions/checkout@v1
      - name: Deploy
        uses: craftyc0der/github-action-kubectl-eks@v1
        env:
          AWS_DEFAULT_REGION: ${{ secrets.AWS_REGION }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        with:
          exec: kubectl apply -f ./file.yaml
          kubeconfig: '${{ secrets.KUBECONFIG }}'
```

To deploy try this:

```bash
git tag -d v1
git push origin :v1
git tag -a -m "v1" v1
git push --follow-tags
```
