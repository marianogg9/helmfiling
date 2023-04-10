# Helmfile(ing)

Running an example [Helmfile](https://helmfile.readthedocs.io/en/latest/) release deploying [httpbin](https://httpbin.org/) app in [Minikube](https://minikube.sigs.k8s.io/docs/start/).

Please have a look at the [blog article](https://blog.mariano.cloud/helm-declaration-environments-helmfile) I wrote for a better overview.

## Schema

```bash
.
├── README.md
├── charts
│   └── httpbin
│       ├── Chart.yaml
│       └── templates
│           ├── deployment.yaml
│           ├── service.yaml
│           └── serviceaccount.yaml
├── default-values.yaml
├── environments.yaml
├── example-environment-values.yaml
├── helmfile.yaml
└── values.yaml.gotmpl
```

- `charts/` contains a minimal `httpbin` Helm chart definition and all the resources' templates.
- `default-values.yaml` is the `default` environment set of values.
- `environments.taml` defines both `default` and `example` environments with their respective values files.
- `example-environment-values.yaml` is the `example` environment set of values.
- `helmfile.yaml` is where all releases are defined.
- `values.yaml.gotmpl` uses GO templating to reference different values. This template will be used as a centralised definition to be used by different environments.

## Installation

I am using a Mac, so every installation is pointing to using that OS; all the below have their options when it comes to different platforms.

- As a first prerequisite, [Minikube](https://minikube.sigs.k8s.io/docs/start/).

    ```bash
    brew install minikube
    ```

- [Kubectl](https://kubernetes.io/docs/tasks/tools/).

    ```bash
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
    ```

- Then [Helmfile](https://helmfile.readthedocs.io/en/latest/)
    
    ```bash
    brew install helmfile
    ```

## Deploying

Start Minikube cluster.
```bash
minikube start
```

Clone this repository.
```bash
git clone https://github.com/marianogg9/helmfiling.git
cd helmfiling
```

Run Helmfile in dry-run mode (to see what will be deployed).
```bash
helmfile -e example diff
```

Then deploy resources.
```bash
helmfile -e example apply
```

To check deployment status.
```bash
helmfile status
```

And many more [CLI commands](https://helmfile.readthedocs.io/en/latest/#cli-reference) to explore.


## Updating

To modify a value, release, template or resource, just update its definition and visualize future changes.
```bash
helmfile -e example diff
```

And then deploy.
```bash
helmfile -e example apply
```

## Cleaning up

Once finished, let's clean all up by deleting Helmfile releases (all resources defined in a release will be deleted).
```bash
helmfile -e example destroy
```

Keep in mind the above `destroy` command **will not** ask for a confirmation.

Then delete Minikube cluster.
```bash
minikube delete --all
```
