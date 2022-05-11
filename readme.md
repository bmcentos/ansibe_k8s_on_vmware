###############################################################################

#Objetivo desse projeto é provisionar maquinas automaticamente no VmWare, instalanco um cluster Kubernetes em alta disponibilidade (Multi Mater), com load balancers e Rancher ja configurado
```
Deploy automatico no vSphere de cluster Kubernetes, Single e Multi master
- MetalLb
- Metrics Server
- Ingress Controller com service LoadBalancer
- Rancher
```

#####################################################################


#By Bruno Miquelini

#Versão: 1.0

#Released: 05/2022



Estrutura:

- Caso a flag "lb_enabled: yes" em vars/vars_files.yaml esteja habilitada, será realizado o deploy de um cluter multi master, para isso informar no arquivo de inventario hosts, no grupo lb, as maquinas que farão parte do cluster
- Caso utilize CentOs, adicionar a flag satellite_enabled:no
- O Load balancer irá trabalhar com o Keepalibed, para manter um IP flutuante entre os Nós
- Esse IP flutuante será o endereço de API Server do cluster Kubernetes
- Caso seja criado um cluster Single master, adicionar apenas um host no grupo [master], e remover as entradas de hosts do grupo [lb], assim como desabilitar a flag "lb_enabled


#Caso queira fazer modificações nos templates de VM, como por exemplo servidores DNS, modificar os aruivos
'''
	- playbooks/k8s.yaml
	- playbooks/haproxy.yaml
	- playbooks/rancher.yaml
'''


#Estrutura de arquivos
```
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
├── hosts	#Arquivo onde serão definido os hosts 
├── playbooks
│   ├── cluster.yaml
│   ├── haproxy.yaml
│   ├── k8s.yaml
│   ├── lb.yaml
│   ├── master.yaml
│   ├── rancher.yaml
│   └── worker.yaml
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
```
#uso:

Antes de executar, confirmar se seu arquivo hosts e vars/vars_file.yaml estão devidamente ajustados
```
ansible-playbook -i hosts deploy.yaml    #-k, caso prefira usar a senha
```

A Execução levará aproximadamente 30 minutos, com a conclusão das tarefas abaixo:

![image](https://user-images.githubusercontent.com/25855270/167884473-4f2eb3ed-dfc4-4d65-b214-dd4f018c968e.png)

Nodes instalados

![image](https://user-images.githubusercontent.com/25855270/167886518-cc5b1180-9d2d-4cd9-b922-3c653d228dcc.png)

Metric Server instalado

![image](https://user-images.githubusercontent.com/25855270/167886681-8b8f3253-1c2c-4298-9800-feecc67bdebc.png)

Ingress Controller ja instalado com endereço Externo

![image](https://user-images.githubusercontent.com/25855270/167886076-5b430d87-261e-4b85-9ba2-981bc627d127.png)

MetalLb ja instalado

![image](https://user-images.githubusercontent.com/25855270/167886231-8c8ee6b0-8fb9-4a08-9227-7750a59ea802.png)

Rancher Server

![image](https://user-images.githubusercontent.com/25855270/167887673-06b8b854-9d7d-4225-b555-a56037827969.png)


