+++
title = 'On_my_zsh'
date = '2024-05-25'
draft = true
author = 'aiclr'
categories = ['tools','archlinux']
tags = ['tools','bash']
Summary='Zsh is a powerful shell that operates as both an interactive shell and as a scripting language interpreter.'
+++

## Oh My Zsh 

[github](https://github.com/ohmyzsh/ohmyzsh)\
config file: [~/.zshrc](conf/zshrc)

### install

archlinux 
```bash
pacman -S zsh curl git
```
opensuse
```bash
zypper in zsh curl git
```

install
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### update

Manual Updates
```bash
omz update
```

Getting Updates:
By default, you will be prompted to check for updates every ***2*** weeks.\
You can choose other update modes by adding a line to your ***~/.zshrc*** file, before **Oh My Zsh** is loaded:

```properties
# Uncomment one of the following lines to change the auto-update behavior
# zstyle ':omz:update' mode disabled  # disable automatic updates
# zstyle ':omz:update' mode auto      # update automatically without asking
zstyle ':omz:update' mode reminder  # just remind me to update when it's time

# Uncomment the following line to change how often to auto-update (in days).
zstyle ':omz:update' frequency 13  # This will check for updates every 13 days
# zstyle ':omz:update' frequency 0   # This will check for updates every time you open the terminal (not recommended)

# Updates verbosity
# You can also limit the update verbosity with the following settings:
zstyle ':omz:update' verbose default  # default update prompt
# zstyle ':omz:update' verbose minimal  # only few lines
# zstyle ':omz:update' verbose silent  # only errors
```

### config

config your ***~/.zshrc*** file

```properties
# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
#ZSH_THEME="darkblood"
ZSH_THEME="linuxonly"
#ZSH_THEME="frisk"
#ZSH_THEME="random"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
#ZSH_THEME_RANDOM_CANDIDATES=("linuxonly" "frisk" "darkblood")

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git docker-compose kubectl)
```

### [docker-compose plugins](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/docker-compose)

| Alias     | Command                   | Description                                                      |
|:----------|:--------------------------|:-----------------------------------------------------------------|
| dco       | docker-compose            | Docker-compose main command                                      |
| dcb       | docker-compose build      | Build containers                                                 |
| dce       | docker-compose exec       | Execute command inside a container                               |
| dcps      | docker-compose ps         | List containers                                                  |
| dcrestart | docker-compose restart    | Restart container                                                |
| dcrm      | docker-compose rm         | Remove container                                                 |
| dcr       | docker-compose run        | Run a command in container                                       |
| dcstop    | docker-compose stop       | Stop a container                                                 |
| dcup      | docker-compose up         | Build, (re)create, start, and attach to containers for a service |
| dcupb     | docker-compose up --build | Same as dcup, but build images before starting containers        |
| dcupd     | docker-compose up -d      | Same as dcup, but starts as daemon                               |
| dcdn      | docker-compose down       | Stop and remove containers                                       |
| dcl       | docker-compose logs       | Show logs of container                                           |
| dclf      | docker-compose logs -f    | Show logs and follow output                                      |
| dcpull    | docker-compose pull       | Pull image of a service                                          |
| dcstart   | docker-compose start      | Start a container                                                |
| dck       | docker-compose kill       | Kills containers                                                 |

### [kubectl plugins](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/kubectl)

| Alias   | Command                                          | Description                                                                                      |
|:--------|:-------------------------------------------------|:-------------------------------------------------------------------------------------------------|
| k       | kubectl                                          | The kubectl command                                                                              |
| kca     | kubectl --all-namespaces                         | The kubectl command targeting all namespaces                                                     |
| kaf     | kubectl apply -f                                 | Apply a YML file                                                                                 |
| keti    | kubectl exec -ti                                 | Drop into an interactive terminal on a container                                                 |
|         |                                                  | Manage configuration quickly to switch contexts between local, dev and staging                   |
| kcuc    | kubectl config use-context                       | Set the current-context in a kubeconfig file                                                     |
| kcsc    | kubectl config set-context                       | Set a context entry in kubeconfig                                                                |
| kcdc    | kubectl config delete-context                    | Delete the specified context from the kubeconfig                                                 |
| kccc    | kubectl config current-context                   | Display the current-context                                                                      |
| kcgc    | kubectl config get-contexts                      | List of contexts available                                                                       |
|         |                                                  | General aliases                                                                                  |
| kdel    | kubectl delete                                   | Delete resources by filenames, stdin, resources and names, or by resources and label selector    |
| kdelf   | kubectl delete -f                                | Delete a pod using the type and name specified in -f argument                                    |
|         |                                                  | Pod management                                                                                   |
| kgp     | kubectl get pods                                 | List all pods in ps output format                                                                |
| kgpw    | kgp --watch                                      | After listing/getting the requested object, watch for changes                                    |
| kgpwide | kgp -o wide                                      | Output in plain-text format with any additional information. For pods, the node name is included |
| kep     | kubectl edit pods                                | Edit pods from the default editor                                                                |
| kdp     | kubectl describe pods                            | Describe all pods                                                                                |
| kdelp   | kubectl delete pods                              | Delete all pods matching passed arguments                                                        |
| kgpl    | kgp -l                                           | Get pods by label. Example: kgpl "app=myapp" -n myns                                             |
| kgpn    | kgp -n                                           | Get pods by namespace. Example: kgpn kube-system                                                 |
|         |                                                  | Service management                                                                               |
| kgs     | kubectl get svc                                  | List all services in ps output format                                                            |
| kgsw    | kgs --watch                                      | After listing all services, watch for changes                                                    |
| kgswide | kgs -o wide                                      | After listing all services, output in plain-text format with any additional information          |
| kes     | kubectl edit svc                                 | Edit services(svc) from the default editor                                                       |
| kds     | kubectl describe svc                             | Describe all services in detail                                                                  |
| kdels   | kubectl delete svc                               | Delete all services matching passed argument                                                     |
|         |                                                  | Ingress management                                                                               |
| kgi     | kubectl get ingress                              | List ingress resources in ps output format                                                       |
| kei     | kubectl edit ingress                             | Edit ingress resource from the default editor                                                    |
| kdi     | kubectl describe ingress                         | Describe ingress resource in detail                                                              |
| kdeli   | kubectl delete ingress                           | Delete ingress resources matching passed argument                                                |
|         |                                                  | Namespace management                                                                             |
| kgns    | kubectl get namespaces                           | List the current namespaces in a cluster                                                         |
| kcn     | kubectl config set-context --current --namespace | Change current namespace                                                                         |
| kens    | kubectl edit namespace                           | Edit namespace resource from the default editor                                                  |
| kdns    | kubectl describe namespace                       | Describe namespace resource in detail                                                            |
| kdelns  | kubectl delete namespace                         | Delete the namespace. WARNING! This deletes everything in the namespace                          |
|         |                                                  | ConfigMap management                                                                             |
| kgcm    | kubectl get configmaps                           | List the configmaps in ps output format                                                          |
| kecm    | kubectl edit configmap                           | Edit configmap resource from the default editor                                                  |
| kdcm    | kubectl describe configmap                       | Describe configmap resource in detail                                                            |
| kdelcm  | kubectl delete configmap                         | Delete the configmap                                                                             |
|         |                                                  | Secret management                                                                                |
| kgsec   | kubectl get secretGet                            | secret for decoding                                                                              |
| kdsec   | kubectl describe secret                          | Describe secret resource in detail                                                               |
| kdelsec | kubectl delete secret                            | Delete the secret                                                                                |
|         |                                                  | Deployment management                                                                            |
| kgd     | kubectl get deployment                           | Get the deployment                                                                               |
| kgdw    | kgd --watch                                      | After getting the deployment, watch for changes                                                  |
| kgdwide | kgd -o wide                                      | After getting the deployment, output in plain-text format with any additional information        |
| ked     | kubectl edit deployment                          | Edit deployment resource from the default editor                                                 |
| kdd     | kubectl describe deployment                      | Describe deployment resource in detail                                                           |
| kdeld   | kubectl delete deployment                        | Delete the deployment                                                                            |
| ksd     | kubectl scale deployment                         | Scale a deployment                                                                               |
| krsd    | kubectl rollout status deployment                | Check the rollout status of a deployment                                                         |
| kres    | kubectl set env $@ REFRESHED_AT=...              | Recreate all pods in deployment with zero-downtime                                               |
|         |                                                  | Rollout management                                                                               |
| kgrs    | kubectl get rs                                   | To see the ReplicaSet rs created by the deployment                                               |
| krh     | kubectl rollout history                          | Check the revisions of this deployment                                                           |
| kru     | kubectl rollout undo                             | Rollback to the previous revision                                                                |
|         |                                                  | Port forwarding                                                                                  |
| kpf     | kubectl port-forward                             | Forward one or more local ports to a pod                                                         |
|         |                                                  | Tools for accessing all information                                                              |
| kga     | kubectl get all                                  | List all resources in ps format                                                                  |
| kgaa    | kubectl get all --all-namespaces                 | List the requested object(s) across all namespaces                                               |
|         |                                                  | Logs                                                                                             |
| kl      | kubectl logs                                     | Print the logs for a container or resource                                                       |
| klf     | kubectl logs -f                                  | Stream the logs for a container or resource (follow)                                             |
|         |                                                  | File copy                                                                                        |
| kcp     | kubectl cp                                       | Copy files and directories to and from containers                                                |
|         |                                                  | Node management                                                                                  |
| kgno    | kubectl get nodes                                | List the nodes in ps output format                                                               |
| keno    | kubectl edit node                                | Edit nodes resource from the default editor                                                      |
| kdno    | kubectl describe node                            | Describe node resource in detail                                                                 |
| kdelno  | kubectl delete node                              | Delete the node                                                                                  |
|         |                                                  | Persistent Volume Claim management                                                               |
| kgpvc   | kubectl get pvc                                  | List all PVCs                                                                                    |
| kgpvcw  | kgpvc --watch                                    | After listing/getting the requested object, watch for changes                                    |
| kepvc   | kubectl edit pvc                                 | Edit pvcs from the default editor                                                                |
| kdpvc   | kubectl describe pvc                             | Describe all pvcs                                                                                |
| kdelpvc | kubectl delete pvc                               | Delete all pvcs matching passed arguments                                                        |
|         |                                                  | StatefulSets management                                                                          |
| kgss    | kubectl get statefulset                          | List the statefulsets in ps format                                                               |
| kgssw   | kgss --watch                                     | After getting the list of statefulsets, watch for changes                                        |
| kgsswid | e kgss -o wide                                   | After getting the statefulsets, output in plain-text format with any additional information      |
| kess    | kubectl edit statefulset                         | Edit statefulset resource from the default editor                                                |
| kdss    | kubectl describe statefulset                     | Describe statefulset resource in detail                                                          |
| kdelss  | kubectl delete statefulset                       | Delete the statefulset                                                                           |
| ksss    | kubectl scale statefulset                        | Scale a statefulset                                                                              |
| krsss   | kubectl rollout status statefulset               | Check the rollout status of a deployment                                                         |
|         |                                                  | Service Accounts management                                                                      |
| kdsa    | kubectl describe sa                              | Describe a service account in details                                                            |
| kdelsa  | kubectl delete sa                                | Delete the service account                                                                       |
|         |                                                  | DaemonSet management                                                                             |
| kgds    | kubectl get daemonset                            | List all DaemonSets in ps output format                                                          |
| kgdsw   | kgds --watch                                     | After listing all DaemonSets, watch for changes                                                  |
| keds    | kubectl edit daemonset                           | Edit DaemonSets from the default editor                                                          |
| kdds    | kubectl describe daemonset                       | Describe all DaemonSets in detail                                                                |
| kdelds  | kubectl delete daemonset                         | Delete all DaemonSets matching passed argument                                                   |
|         |                                                  | CronJob management                                                                               |
| kgcj    | kubectl get cronjob                              | List all CronJobs in ps output format                                                            |
| kecj    | kubectl edit cronjob                             | Edit CronJob from the default editor                                                             |
| kdcj    | kubectl describe cronjob                         | Describe a CronJob in details                                                                    |
| kdelcj  | kubectl delete cronjob                           | Delete the CronJob                                                                               |
