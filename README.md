## How to install openfaas in an ubuntu virtual server.

#### Digital Ocean Refeal Link
Use the following link to get 100 credit from Digital Ocean. If you use this refeal some credit will be added to my account

```
https://m.do.co/c/c5f51307a8a2
```

#### Install Server Requirenemtns

1. Create a non root user,
```
$ adduser faasusr
```

2. Add the user to sudo group
```
$ usermod -aG sudo faasusr
```

3. Copy the SSH key to user's home directory.
```
$ rsync --archive --chown faasusr:faasusr .ssh /home/openfaas 
```

4. Install K3S.
```
$ curl -sfL https://get.k3s.io | sh -
```

5. Configure K3S
```
cd ~
mkdir .kube
sudo rsync --archive --chown openfaas:openfaas /etc/rancher/k3s/k3s.yaml ~/.kube/config
export KUBECONFIG=$HOME/.kube/config
export KUBE_EDITOR=nano
```

6. Install Arkade
```
$ sudo curl -SLsf https://dl.get-arkade.dev/ | sudo sh
```

#### Install and Configure OpenFaas

7. Install OpenFaas.
```
$ arkade install openfaas --load-balancer
```

8. At this point OpenFaas is installed, to get the password for the gateway use this.
```
$ echo $(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode; echo)
```

9. To install the faas-cli use 
```
# for unix
$ curl -SLsf https://cli.openfaas.com | sudo sh

# for windows
$ https://github.com/openfaas/faas-cli/releases/download/0.13.9/faas-cli.exe

# to see the list of available commands run 
$ faas-cli -h
```

## To list and pull templates

1. List oficial templates
```
faas-cli template store list
```
2. List custom store templates.
```
faas-cli template store list --url https://your.server.net/templates.json
```

3. Pull custom templates
```
faas-cli template store pull <<template-name>> --url https://your.server.net/templates.json
```

4. To creae a function
```
faas-cli new <<func-name>> --lang <<template-name>> --prefix <<your-container-register-username>>
```

##### Happy Codding !!!
