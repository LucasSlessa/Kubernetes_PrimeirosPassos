# Configuração do Ambiente Kubernetes   <img align="center" alt="Kubernets" height="50" width="50" src="https://hermes.dio.me/articles/cover/d15641bf-9cee-493e-a5a4-f41ca0ffe7f7.png" />

## Preparação do Ambiente

Para configurar nosso ambiente Kubernetes, vamos precisar de:
- Uma ferramenta de contêiner, como o Docker.
- `kubectl` - linha de comando do Kubernetes (cliente que se comunica com a API do Kubernetes).
- Uma ferramenta para execução local do Kubernetes.

### Links úteis:
- Docker: [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- kubectl: [kubectl Installation Guide](https://kubernetes.io/docs/tasks/tools/)
- Ferramentas para simular Kubernetes localmente:
  - [Minikube](https://minikube.sigs.k8s.io/docs/start/)
  - [Kind](https://kind.sigs.k8s.io/)
  - [k3d](https://k3d.io/)

## Instalando o Docker no Ubuntu 18.04.6 LTS  <img align="center" alt="Docker" height="50" width="50" src="https://logopng.com.br/logos/docker-27.png" /> <img align="center" alt="Docker" height="50" width="50" src="https://brandslogos.com/wp-content/uploads/images/large/ubuntu-logo.png" />

1. Abra o terminal Gnome:
```bash
sudo apt install gnome-terminal
   ```
2. Remova versões antigas do Docker, se existirem:

  ```bash
  sudo apt-get remove docker docker-engine docker.io containerd runc
  ```
3. Atualize o índice de pacotes:

  ```bash
  sudo apt-get update
  ```
4. Instale pacotes necessários para usar repositórios HTTPS:
 
  ```bash
  sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
  ```
5. Adicione a chave GPG oficial do Docker:

  ```bash
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  ```
6. Adicione o repositório Docker às fontes do APT:

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
7. Atualize o índice de pacotes novamente:

```bash
sudo apt-get update
```
8. Instale o Docker:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
9. Inicie e habilite o Docker para iniciar no boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```
10. Verifique a instalação do Docker:

```bash
docker --version
```
11. Instale o Docker Compose:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -oP '"tag_name": "\K(.*)(?=")')/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
12. Verifique a instalação do Docker Compose:

```bash
docker-compose --version
Instalando o kubectl
```
## Instalando o KubeCtl <img align="center" alt="Kubernets" height="50" width="50" src="https://hermes.dio.me/articles/cover/d15641bf-9cee-493e-a5a4-f41ca0ffe7f7.png" />

1. verificando a versão mais recente
2. 
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
2. Baixe o arquivo de checksum SHA256:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
3. Verifique o checksum do binário:

```bash
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
```
4. Torne o binário executável:

```bash
chmod +x kubectl
```
5. Mova o binário para um diretório incluído no PATH do sistema:

```bash
sudo mv kubectl /usr/local/bin/
```
6. Verifique a instalação do kubectl:

```bash
kubectl version --client
```
7. Verifique a versão do kubectl em formato YAML (opcional):
```bash
kubectl version --client --output=yaml
```

## Instalando o Kind <img align="center" alt="Kubernets" height="40" width="80" src="https://kind.sigs.k8s.io/logo/logo.png" />

Baixe o binário do Kind e mova-o para um diretório incluído no seu PATH, como /usr/local/bin:

```Bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.12.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```
Certifique-se de substituir v0.12.0 pela versão mais recente do Kind disponível no site oficial do Kind.

Verifique se o Kind foi instalado corretamente:

```Bash
kind version
```
Com o Kind instalado, você pode agora criar e gerenciar clusters Kubernetes locais usando o Docker.






# Primeiros passos configurando nosso cluster:
## Configuração de Cluster Kubernetes com `kind`

## Pré-requisitos

- Docker instalado e em execução.
- `kind` instalado. Você pode instalar `kind` seguindo as instruções na [documentação oficial](https://kind.sigs.k8s.io/docs/user/quick-start/).

## Passos para Configuração

### 1. Criar o Diretório de Trabalho

Crie um diretório chamado `kubernetes` e navegue até ele:

```bash
mkdir kubernetes
cd kubernetes
```
2. Verifique a Instalação do Docker
Certifique-se de que o Docker está instalado e em execução corretamente:

```bash
docker --version
```
2.1 Inicie o Docker
Se o Docker não estiver em execução, você pode iniciá-lo com o seguinte comando:

```bash
sudo systemctl start docker
```
Para garantir que o Docker inicie automaticamente ao ligar o sistema:

```bash
sudo systemctl enable docker
```

3. Verifique a Instalação do Kind
Certifique-se de que o kind está instalado e acessível no seu PATH:

```bash
kind --version
```
4. Criando o Arquivo de Configuração do Cluster
Crie um arquivo chamado cluster-kind.yaml com o seguinte conteúdo:

```yaml

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```
5. Criando o Cluster
Para criar um cluster com kind utilizando o arquivo de configuração criado, utilize o seguinte comando:

```bash
kind create cluster --name sd --config cluster-kind.yaml
```
6. Possíveis Problemas e Soluções
Problema: Erro ao puxar a imagem kindest/node
Se você encontrar o seguinte erro:

```bash
ERROR: failed to create cluster: failed to pull image "kindest/node:v1.23.4@sha256:..."
Command Output: Error response from daemon: Get "https://registry-1.docker.io/v2/": dial tcp: lookup registry-1.docker.io on 10.255.255.254:53: no such host
```
Solução:
Verifique sua conexão de internet:

Certifique-se de que sua conexão de internet está funcionando corretamente.
Reinicie o Docker:

Reinicie o serviço Docker para resolver possíveis problemas de rede.
```bash
sudo systemctl restart docker
```
Verifique as configurações de DNS do Docker:

Adicione uma configuração de DNS customizada no arquivo daemon.json do Docker.
```bash
sudo nano /etc/docker/daemon.json
```
Adicione a seguinte linha (caso ainda não exista):

```json
{
  "dns": ["8.8.8.8", "8.8.4.4"]
}
```
Salve e saia do editor. Reinicie o Docker:

```bash
sudo systemctl restart docker
```
Verifique as configurações de proxy do Docker (se aplicável):

Se você estiver atrás de um proxy, configure o Docker para usar as configurações de proxy corretas.
Verifique os logs do Docker:

Verifique os logs do daemon do Docker para mais informações sobre o problema.
```bash
sudo journalctl -u docker.service
```
7. Verifique o Cluster
Depois de criar o cluster, verifique se ele está funcionando corretamente:

```bash
kubectl cluster-info --context kind-sd
```
Você deve ver informações sobre o cluster, confirmando que ele foi criado com sucesso.

8. Trabalhe com seu Cluster
Agora você pode começar a trabalhar com seu cluster Kubernetes. Por exemplo, para listar os nós no cluster:

```bash
kubectl get nodes
```

### Comandos Úteis do kubectl

Aqui estão alguns comandos úteis do `kubectl` que podem ajudar na administração do seu cluster Kubernetes:

- Visualizar a configuração atual do kubectl:
  ```bash
  kubectl config view
- Obter informações sobre os contextos disponíveis:

  ```bash
  kubectl config get-contexts
  ```
- Visualizar informações do cluster com um contexto específico:

  ```bash
  kubectl cluster-info --context kind-<nome do cluster>
  ```
- Dump de informações detalhadas do cluster:

  ```bash
  kubectl cluster-info dump
  ```
- Listar todos os nós do cluster:

  ```bash
  kubectl get nodes
  ```
- Listar todos os nós do cluster com detalhes adicionais:

  ```bash
  kubectl get nodes -o wide
  ```
- Listar todos os nós do cluster exibindo suas labels:
    
  ```bash
    kubectl get nodes --show-labels
    ```

# Desplegando uma Aplicação Nginx no Kubernetes
### Pré-requisitos
Certifique-se de que você já tem o Docker, kubectl, e kind instalados e configurados corretamente.

## Passos para Desplegar a Aplicação
1. Criar um Manifesto YAML para o Deployment
Um manifesto YAML descreve o estado desejado do objeto que você deseja criar no cluster Kubernetes. Vamos criar um manifesto para um Deployment do Nginx.

Crie um arquivo chamado nginx-deployment.yaml com o seguinte conteúdo:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

2. Aplicar o Manifesto no Cluster
Utilize o kubectl para aplicar o manifesto YAML que criamos e desplegar a aplicação no cluster.

```bash
kubectl apply -f nginx-deployment.yaml
```
3. Verificar o Status do Deployment
Após aplicar o manifesto, verifique se o Deployment foi criado com sucesso e se os pods estão em execução.

```bash
kubectl get deployments
kubectl get pods
```
4. Expor o Deployment com um Serviço
Para tornar a aplicação acessível externamente, precisamos expor o Deployment com um Serviço. Crie um arquivo chamado nginx-service.yaml com o seguinte conteúdo:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```
5. Aplicar o Manifesto do Serviço
Utilize o kubectl para aplicar o manifesto YAML do Serviço.

```bash
kubectl apply -f nginx-service.yaml
```
6. Verificar o Status do Serviço
Verifique se o Serviço foi criado corretamente e obtenha o endereço IP para acessar a aplicação.

```bash
kubectl get services
```
Se o tipo LoadBalancer não estiver disponível no seu ambiente local, você pode usar o tipo NodePort para acessar a aplicação via um dos nós do cluster.

7. Acessar a Aplicação
Depois que o Serviço estiver em execução, você poderá acessar a aplicação Nginx através do endereço IP e porta fornecidos pelo Serviço.

### Comandos Úteis para Gerenciamento

comandos úteis para gerenciar sua aplicação no Kubernetes:

Ver detalhes de um Deployment específico:
```bash
kubectl describe deployment nginx-deployment
```
Ver detalhes de um Serviço específico:
```bash
kubectl describe service nginx-service
```
Escalar o número de réplicas de um Deployment:
```bash
kubectl scale deployment nginx-deployment --replicas=4
```
Atualizar a imagem de um contêiner em um Deployment:
```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.19.10
```

<img align="center" alt="Java" height="600" width="800" src="https://blog.4linux.com.br/wp-content/uploads/2019/06/Devops1-1900x1241_c.jpeg">



