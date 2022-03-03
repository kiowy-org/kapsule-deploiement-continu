# KIND : Kubernetes IN Docker

Dans ce TP, nous allons installer kind, afin de créer un cluster sur notre machine.
Contrairement à un cluster de production, qui nécessiterait des VMs ou des machines physiques, KIND utilise des conteneurs afin de jouer le rôle des Nodes de votre Cluster.

## 1. Installation de kubectl
Avant de commencer l'installation de KIND, nous allons installer kubectl.
`kubectl` est l'outil en ligne de commande de Kubernetes. Il nous permettra de manipuler les objets contenus dans le cluster.

```shell
{
    export KUBECTL_VERSION=$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$KUBECTL_VERSION/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
    kubectl version --client
}
```

Ou via le gestionnaire de paquets de votre choix :
Homebrew :
```shell
{
    brew install kubectl
    kubectl version --client
}
```

Résultat :
```
Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.4", ...
```

Pour plus de confort, vous pouvez ajouter l'autocomplétion à votre shell, ainsi que l'alias `k` pour `kubectl`.

```shell
{
    # Cette procédure est pour bash
    sudo apt install bash-completion
    source <(kubectl completion bash)
    echo "source <(kubectl completion bash)" >> ~/.bashrc
    alias k=kubectl
    complete -F __start_kubectl k
}
```

## 2. Installation de KIND

Installons KIND depuis le binaire :
```shell
{
    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind
}
```

Une fois la CLI Kind installée, créez un cluster Kubernetes avec la commande suivante (le fichier `cluster-config.yaml`) :

```shell
kind create cluster --config cluster-config.yaml
```

Une fois le démarrage de votre cluster terminé, vérifiez que vos noeuds sont bien présents grâce à la commande `kubectl get nodes` (ou `k get nodes`).

```
NAME                 STATUS   ROLES                  AGE   VERSION
kind-control-plane   Ready    control-plane,master   59m   v1.21.1
kind-worker          Ready    <none>                 58m   v1.21.1
```

Si vous obtenez un résultat comme ci-dessus, félicitation, Kind est installé.

Une dernière chose, afin d'assurer le fonctionnement du cluster, un certain nombre d'addons sont nécessaires.
Utilisez la commande suivante afin d'installer l'addons metrics-server dans votre cluster.

```shell
kubectl apply -f metrics-server.yaml
```

## 3. Installation de Argo-rollout

### 1. Installation du controller dans le cluster
```shell
    kubectl create namespace argo-rollouts
    kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```

### 2. Installation du plugin kubectl

```shell
{
    curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-darwin-amd64
    chmod +x ./kubectl-argo-rollouts-darwin-amd64
    sudo mv ./kubectl-argo-rollouts-darwin-amd64 /usr/local/bin/kubectl-argo-rollouts
}
```

Homebrew :
```shell
    brew install argoproj/tap/kubectl-argo-rollouts
```

Resultat :
```shell
    ❯ kubectl argo rollouts version
    kubectl-argo-rollouts: v1.1.0+ff3471a
        BuildDate: 2021-10-11T20:20:08Z
        GitCommit: ff3471a2fc3ccb90dbb1f370d7e399ff3064043a
        GitTreeState: clean
        GoVersion: go1.16.3
        Compiler: gc
        Platform: darwin/amd64
```