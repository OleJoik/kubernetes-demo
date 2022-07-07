# Helm

### Table of contents

- What is helm?
- What are helm charts?
- How/when should I use them?
- What is Tiller?

## What is helm?
- Package manager for kubernetes, ala homebrew/apt
- To package YAML-files 
- Distribute them in private and public repositories
- [Helm installation](https://helm.sh/docs/intro/install/)

## Helm charts
### A bundle of YAML files
- Create your own helm charts with Helm
- Push them to Helm repository
- Reuse configuration from someone 

### Resources
- Helm search CLI:  `helm search <keyword>`
- Helm hub/[artifacthub](https://artifacthub.io/)

### Private registries
- Typically shared within organizations
- There are specific tools for this

### Structure
Helm charts are typically created with [the following example folder structure](mychart/):
```
mychart/
    Chart.yaml
    values.yaml
    charts/
    templates/
    ...
```

where...

the top level `mychart` folder is the <b>name</b> of the chart.

`Chart.yaml` holds the meta information about the chart.

`values.yaml` -> Values for the template files (default values you can override later)

`charts` folder -> chart dependencies

`templates` the actual template files

other files may include Readme or licence files

```
helm install <chartname>
```

## Templating engine
1) Define a common blueprint
2) Dynamic values are replaced by placeholders in a template file. It may look something like the [template-pod.yaml](template-pod.yaml) file.
3) Values are injected into the yaml-files from [values.yaml](values.yaml).
4) Values can also be set with the `--set` flag
5) Values can be set by overriding default values with an alternative values file as such:
```
helm install --values=other-values.yaml
```

This will merge any values from `other-values.yaml` with those in the default `values.yaml`, into a "`.Values object`"

These features might be particiularly useful when deploying the same application across different environments.


## Release management
Different in version 2 and 3 of Helm.
### Version 2
Helm CLI sends request to Tiller, which runs inside of cubernetesand Tiller will then install that chart

```
helm install <chartname>
```

Tiller keeps a chart execution history, making version control of helm charts possible.

```
helm upgrade <chartname>
```

```
helm rollback <chartname>
```

Security-issues due to Tiller having too much "power" inside of kubernetes.


### Version 3

Tiller is removed, but it would appear that most of the functionality remains.