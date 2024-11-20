# valheim-dedicated-helm
Helm chart for deploying Valheim dedicated server

This is mostly catered to my setup. Very janky. Update your desired variables in values.yaml. Dedicated server is based on the container image [theoriginalbrian/phvalheim-server](https://github.com/brianmiller/phvalheim-server).

A note about PhValheim worlds port range. I've default set for 10 to open wich sould be enough for a handful of individual servers, but you can expand that range if you wish. Update the `range seq` in `lotsofdeployment.yaml` and `lotsofports.yaml` then, using [gomplate](https://docs.gomplate.ca/installing/) run the following commands

```
cat servicepart.yaml > templates/service.yaml
gomplate -f lotsofports.yaml >> servicepart.yaml && sed -i '/^\s*$/d' servicepart.yaml

cat deploymentpart.yaml > templates/deployment.yaml
gomplate -f lotsofdeployment.yaml >> deploymentpart.yaml && sed -i '/^\s*$/d' deploymentpart.yaml && sed -i 's/placeholder/{{ .Values.env.hostIP }}/g' deploymentpart.yaml
```
This is because kubernetes doesn't allow for opening port ranges. I'll eventually make this a more sane process, but this gets the job done for now.

