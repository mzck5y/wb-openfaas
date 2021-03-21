## How to install openfaas in an ubuntu virtual server.

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
$ cd ~
$ mkdir .kube
```
```
$ sudo rsync --archive --chown openfaas:openfaas /etc/rancher/k3s/k3s.yaml ~/.kube/config
```
```
$ export KUBECONFIG=$HOME/.kube/config
```

6. Install Arkade

```
$ sudo curl -SLsf https://dl.get-arkade.dev/ | sudo sh
```

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
```