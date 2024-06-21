# Configuração do Ambiente Kubernetes

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

## Instalando o Docker no Ubuntu 18.04.6 LTS

1. Abra o terminal Gnome:
   ```bash
   sudo apt install gnome-terminal
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
## Instalando o KubeCtl

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
