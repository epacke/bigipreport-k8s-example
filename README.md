# Bigipreport repo

* Upstream image code can be found [https://github.com/net-utilities/BigIPReport-Docker](here)
* Upstream BigIPReport code can be found [https://github.com/net-utilities/BigIPReport](here)

## Application works like this:
1. The data-collector collects info from the loadbalancers and creates a JSON in a PV
2. The frontend has node affinity based on where the data-collector runs and reads the same PV as read-only

Config can be found in the [helm folder](helm/).

*Important: The data-collector runs as a batchjob and it will say "Not ready" when it has finished.
This is due to the Istio sidecar not quitting when the job has been done but it is OK.*

## Changing the configuration

### The configuration file
Update the [BigIPReport helm values file](helm/values.yaml)

*Note that credentials are not supposed to be saved here, look below for more on that*

## Upgrading BigIPReport
Update the image tags in [BigIPReport helm values file](helm/values.yaml) with one from
the [upstream repo](https://github.com/net-utilities/BigIPReport/releases) and install with helm upgrade.

## Installation
1. Create a namespace that bigip should reside in
1. Create a kubernetes secret in the namespace you've chosen for `bigipreport` however you're deploying secrets:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: bigipreport-secrets
  namespace: bigipreport
data:
  F5_USERNAME: YmlnaXByZXBvcnQ=
  F5_PASSWORD: aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj1kUXc0dzlXZ1hjUQ==
  F5_SUPPORT_USERNAME: bXlAZW1haWwuY29t
  F5_SUPPORT_PASSWORD: aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj1kUXc0dzlXZ1hjUQ==
```
3. Run `helm install`
