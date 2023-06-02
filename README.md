
## Vagrant - Provisionando Minikube (Kubernetes local) para estudos e testes

Passo a passo de como criar uma VM com Minikube pronta para uso, para ser usada durante o desenvolvimento e testes do Kubernetes, rodando no CentOS 7.

Inclusive já será feito uma mapeamento entre a pasta host e vm.

No exemplo a seguir será criado uma VM no VirtualBox, que terá a seguinte identificação:

| Hostname   | IP       |
| :---------- | :--------- |
| minikube | 192.168.56.150 |

Tudo que for implementado ou criado na pasta raiz desse tutorial, já ficará disponível dentro do diretório da VM em:

**/home/vagrant/projeto**

### Observação

Minikube só é recomendado para testes e estudos sobre Kubernetes.

Vagrant só é recomendado para ambientes de testes e desenvolvimento, conforme pode ser visto no link abaixo.

https://developer.hashicorp.com/vagrant/intro/vs/terraform

### Versões de softwares utilizados nesse tutorial

SO Windows 11, Vagrant 2.3.3 e VirtualBox 7.0.6

### Requisitos mínimos

Ter em seu SO os seguintes softwares.
Abaixo de cada um segue o link para download.

- VirtualBox (Software de virtualização)

  https://www.virtualbox.org/wiki/Downloads

- Vagrant (Software para configurar ambientes)

  https://developer.hashicorp.com/vagrant/downloads

- Putty ou MobaXterm (Software de cliente remoto)

  https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

  https://mobaxterm.mobatek.net/download-home-edition.html

- Vagrant - plugin vbguest, via linha de comando, execute:

  vagrant plugin install vagrant-vbguest

### Siga as etapas

1 - Crie uma pasta com nome de sua preferência em qualquer lugar do seu SO. No meu caso vou chama lá de “**projeto**” e dentro dela crie um arquivo chamado **Vagrantfile**, sem extensão.

![App Screenshot](images/img1.png)

2 - O arquivo **Vagrantfile** terá algumas configurações pontuais para nossa única VM.

- **SO:** CentOS 7
- **Hostname:** minikube
- **IP:** 192.168.56.150
- **CPU:** 4
- **Memória RAM:** 6

![App Screenshot](images/img2.png)

Ainda dentro do **Vagrantfile**, criaremos um apontamento para o arquivo **script.sh**, que será nosso script externo com toda a instalação do Minikube.

![App Screenshot](images/img3.png)

Conteúdo completo arquivo Vagrantfile

https://github.com/tiagotsc/vagrant-minikube/blob/dad0f0990808d7711e0f0c9ea7996f1de4c22123/Vagrantfile#L1-L61

Depois de tudo configurado, salve o arquivo.

3 - Agora dentro da pasta **projeto**, crie o arquivo **script.sh** .

![App Screenshot](images/img4.png)

E coloque o seguinte conteúdo:

https://github.com/tiagotsc/vagrant-minikube/blob/948079cecfa86fa6db65e72b2cc1b4a8ae9a2567/script.sh#L1-L26

Depois de inserir o conteúdo, salve o arquivo.

4 - Via linha de comando, dentro da pasta “**projetos**”, vamos subir nossa VM, execute:

```bash
# Liga ou cria a VM, caso ainda não existam
vagrant up
```

![App Screenshot](images/img5.png)

O processo demora um pouco e todos os passos que são executados podem ser acompanhados em tempo real via linha de comando.

5 - Quando todo o processo terminar, abra o putty e forneça o IP da VM:

**IP:** 192.168.56.150

**Usuário:** root ou vagrant

**Senha:** vagrant

![App Screenshot](images/img6.png)

Na figura abaixo, já estando logado na VM, é possível ver a versão do Minikube e kubectl já pronto para uso.

```bash
# Ver versão do minikube
minikube version
# Ver versão do kubectl
kubectl version
```

![App Screenshot](images/img7.png)

### Alguns comandos úteis

Via linha de comando, é preciso estar na pasta "**projeto**", pasta onde está o **Vagrantfile**,  para executar os comandos:

```bash
##### VAGRANT - é preciso estar na pasta que contém o Vagrantfile #####

# Liga VM, ou cria, caso ainda não exista
vagrant up

# Desliga a VM
vagrant halt

# Reiniciar a VM
vagrant reload

# Destrói a VM, mas pergunta antes
vagrant destroy

# Destrói a VM sem perguntar
vagrant destroy -f

##### MINIKUBE #####

# Crie um cluster, com 3 nós, por exemplo
minikube start --nodes 3 --driver='docker'

# Para o cluster
minikube stop

# Lista os pods com mais detalhe
kubectl get pods -o wide

# Lista nós do cluster com mais detalhe
kubectl get nodes -o wide
```

Todo o tutorial aplicado aqui, já se encontra pronto para uso nesse repositório, se você tiver em seu SO o **Vagrant** e **VirtualBox**, basta clonar e subir o ambiente.

#### Documentação oficial do Vagrant

https://developer.hashicorp.com/vagrant/docs

#### Documentação oficial do Minikube
https://minikube.sigs.k8s.io

## 🔗 Links
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/tiago-s-costa)
