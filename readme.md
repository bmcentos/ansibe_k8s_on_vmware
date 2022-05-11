###############################################################################3
'''
Deploy automatico no vSphere de cluster Kubernetes, Single e Multi master
'''

#####################################################################
#By Bruno Miquelini
#Version: 1.0
#Released: 05/2022



#Estrutura:
#	Caso a flag "lb_enabled: yes" em vars/vars_files.yaml esteja habilitada, será realizado o deploy de um cluter multi master, para isso informar no arquivo de inventario hosts, no grupo lb, as maquinas que farão parte do cluster
#	O Load balancer irá trabalhar com o Keepalibed, para manter um IP flutuante entre os Nós
#	Esse IP flutuante será o endereço de API Server do cluster Kubernetes
#	Caso seja criado um cluster Single master, adicionar apenas um host no grupo [master], e remover as entradas de hosts do grupo [lb], assim como desabilitar a flag "lb_enabled
#

#Caso queira fazer modificações nos templates de VM, como por exemplo servidores DNS, modificar os aruivos
'''
	- playbooks/k8s.yaml
	- playbooks/haproxy.yaml
	- playbooks/rancher.yaml
'''


#Estrutura de arquivos

├── deploy.yaml    #Main, arquivo que fará o deploy da stack
├── files
│   ├── 10-flannel.conflist
│   ├── cni.yaml
│   ├── crictl.yaml
│   ├── internet-ingress.yaml
│   ├── kubelet.service
│   ├── kubernetes.repo
│   ├── metallb.yaml
│   └── metrics-server.yaml
├── get_cni.sh
├── hosts
├── playbooks
│   ├── cluster.yaml
│   ├── haproxy.yaml
│   ├── k8s.yaml
│   ├── lb.yaml
│   ├── longhorn.yaml
│   ├── master.yaml
│   ├── rancher.yaml
│   ├── satellite.yaml
│   └── worker.yaml
├── task.yaml
├── templates
│   ├── cm_metallb.yaml.j2
│   ├── config.toml.j2
│   ├── containerd.service.j2
│   ├── containerdInstall.sh.j2
│   ├── docker-http-proxy.conf.j2
│   ├── haproxy.cfg.j2
│   ├── import-cluster-rancher.sh.j2
│   ├── keepalived.conf.j2
│   └── yum.conf.j2
└── vars
    └── vars_file.yaml   #Arquivo de definição de variaveis

#uso:
'''
ansible-playbook -i hosts deploy.yaml    #-k, caso prefira usar a senha
