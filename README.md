# forgeops

ForgeRock 7.0 sandbox for experimentation running on a 16GB MacBook Pro

## Installation

Based on <https://backstage.forgerock.com/docs/forgeops/7/devops-minikube-implementation-env.html>

### git

    git clone https://github.com/ForgeRock/forgeops.git
    cd forgeops
    git checkout tags/2020.08.07-ZucchiniRicotta.1 -b paulcromey

### brew

    brew cask install virtualbox
    brew install docker kubernetes-cli skaffold kustomize kubectx minikube

### minikube

    minikube start --memory=8192 --cpus=4 --disk-size=40g \
    --vm-driver=virtualbox --bootstrapper kubeadm --kubernetes-version=1.18.3
    minikube addons enable ingress
    minikube ssh sudo ip link set docker0 promisc on

### kubectl

    kubectl create namespace [namespace]
    kubens [namespace]

### hostname

    minikube ip
    sudo vi /etc/hosts

add entry [minikube-ip-address] [namespace].iam.example.com

### docker engine

    eval $(minikube docker-env)
    skaffold config set --kube-context minikube local-cluster true

### update namespace

    vi forgeops/kustomize/overlay/7.0/all/kustomization.yaml 

update namespace from default to [namespace] and FQDN to [namespace].iam.example.com

    forgeops/bin/config.sh init --profile cdk --version 7.0

### skaffold

    cd forgeops
    skaffold run

## testing

    kubectl get pods

## password

    cd bin                    
    ./print-secrets.sh amadmin

## clean up

shutdown

    skaffold delete

remove pods

    kubectl delete pvc --all

remove minikube instance

    minikube delete
