# Network-wide ad blocking with Pi-hole, Kubernetes, and Raspberry Pi

## Install

1. [Set up](https://hackmd.io/@santisbon/Hk9NddPqj#Raspberry-Pi) your Raspberry Pi with a static IP.
2. [Install and configure MicroK8s](https://hackmd.io/@santisbon/Hk9NddPqj#MicroK8s) (or any other lightweight Kubernetes distribution) on your Raspberry Pi.
3. On your Raspberry Pi:
    ```shell
    git clone https://github.com/santisbon/pi-hole-k8s.git && cd pi-hole-k8s
    nano ./piholechart/values.yaml
    # edit the values

    sudo mkdir -p /etc/pihole
    sudo mkdir -p /etc/dnsmasq.d
    ```
4. On your Pi, install the Helm chart which will [enforce the installation order](https://helm.sh/docs/intro/using_helm). Replace parameters with a secure password and the static IP of your Raspberry Pi if you didn't do it through the `values.yaml` file. If using MicroK8s type the commands as `microk8s helm` and `microk8s kubectl`.
    ```shell
    RELEASE=pihole

    helm install $RELEASE ./piholechart \
        --namespace $RELEASE-n \
        --create-namespace \
        --set webPassword=supersecret \
        --set externalIP=192.168.XXX.XXX
    ```
5. [Configure your router](https://docs.pi-hole.net/routers/asus/) to set Pi-hole as your DNS server.
6. From your desktop, access the web admin interface at http://192.168.XXX.XXX:8000/admin

## Upgrade:
```shell
helm upgrade $RELEASE ./piholechart -n $RELEASE-n
```

## Uninstall:
```shell
helm uninstall $RELEASE -n $RELEASE-n --wait
kubectl delete namespaces $RELEASE-n
```