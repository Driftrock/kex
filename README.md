# KEX - Kubernetes EXec
_Run command in a new pod created with deployment configuration - just like you would with `heroku run`_

In production environment applications are deployed in k8s
using Deployment. That creates Replica Sets and Pods

In case of need to connect to the production console,
usually one do `kubectl exec` on the existing pod. That has few disadvantages:
- production pod can be terminated by new deployment during the session
- mistake in production pod can cause pod to die causing application outage
- one needs to look for the pod name which is randomize by deployment

This script uses deployment name to get the pod spec template from it,
enhance the spec to allow headless application container to run, then
creates the pod and exec into it. After session closes it deletes the pod.

## Usage:

```sh
kex [kubectl options] -- deployment_name command...
```

Example:

```sh
kex -n staging --cluster=mycompany-eu -- landing-web rails c
```

## Deployment and Pod assumptions

To make the `kex` command work out of box we choosed to adopt few assumptions
about the deployment and the pod specification in it:

- Deployment pod specification has to container container with the same name as the deployment
- Pod should have label with name "app". This will be altered with suffix '-kex'
- The container with deployment name will be used to run the command

## Known limitations

- will not work with deployments using persistant volumes

## Contributions

- Fill an issue in GitHub.
- For feature requests please provide your reasoning.

