# Guía de instalación de Rancher sobre vSphere 6.7
#### Fuentes:
https://rancher.com/docs/rancher/v2.x/en/installation/requirements/

https://rancher.com/docs/rancher/v2.x/en/installation/other-installation-methods/single-node-docker/

https://rancher.com/docs/rancher/v2.x/en/installation/requirements/installing-docker/

## Inslatando docker: 
```bash
curl https://releases.rancher.com/install-docker/19.03.sh | sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 15553  100 15553    0     0  20533      0 --:--:-- --:--:-- --:--:-- 20545
+ '[' centos = redhat ']'
+ sh -c 'yum install -y -q yum-utils'
+ sh -c 'yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo'
Complementos cargados:fastestmirror
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
+ '[' stable '!=' stable ']'
+ sh -c 'yum makecache fast'
Complementos cargados:fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.airenetworks.es
 * extras: mirror.airenetworks.es
 * updates: mirror.airenetworks.es
base                                                                                                                                                  | 3.6 kB  00:00:00     
docker-ce-stable                                                                                                                                      | 3.5 kB  00:00:00     
extras                                                                                                                                                | 2.9 kB  00:00:00     
updates                                                                                                                                               | 2.9 kB  00:00:00     
(1/2): docker-ce-stable/7/x86_64/updateinfo                                                                                                           |   55 B  00:00:00     
(2/2): docker-ce-stable/7/x86_64/primary_db                                                                                                           |  46 kB  00:00:00     
Se ha creado el caché de metadatos
+ sh -c 'yum install -y -q docker-ce-19.03.12 docker-ce-cli-19.03.12'
warning: /var/cache/yum/x86_64/7/docker-ce-stable/packages/docker-ce-19.03.12-3.el7.x86_64.rpm: Header V4 RSA/SHA512 Signature, key ID 621e9f35: NOKEY
No se ha instalado la llave pública de docker-ce-19.03.12-3.el7.x86_64.rpm 
Importando llave GPG 0x621E9F35:
 Usuarioid  : "Docker Release (CE rpm) <docker@docker.com>"
 Huella       : 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
 Desde      : https://download.docker.com/linux/centos/gpg
+ '[' -d /run/systemd/system ']'
+ sh -c 'service docker start'
Redirecting to /bin/systemctl start docker.service
+ sh -c 'docker version'
Client: Docker Engine - Community
 Version:           19.03.12
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        48a66213fe
 Built:             Mon Jun 22 15:46:54 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.12
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       48a66213fe
  Built:            Mon Jun 22 15:45:28 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.3.7
  GitCommit:        8fba4e9a7d01810a393d5d25a3621dc101981175
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
```

Habilitamos el servicio para que persista en el siguiente arranque:
```bash
systemctl enable docker
```

## Desactivamos firewalld y SeLinux:

```bash
systemctl disable firewalld
systemctl stop firewalld
sed -i 's/enforcing/disabled/g' /etc/selinux/config  #### requiere reinicio

init 6 #### reiniciamos, ojo!
```
## Instalamos rancher en un sólo nodo con certificado autogenerado:

```bash
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  rancher/rancher:latest
```
