### Comandos do Docker v29
**A instalação deve seguir os passos da documentação oficial do Docker v29 para sua distribuição Linux ou Windows.**

#### Verificar versão do Docker
* Verificar a versão instalada do Docker:
```bash
docker --version
```
* Verificar a versão do Docker Compose:
```bash
docker-compose --version
```
* Restartar o serviço Docker:
```bash
sudo systemctl restart docker
```
* Habilitar o serviço Docker para iniciar automaticamente na inicialização do sistema:
```bash
sudo systemctl enable docker
```
* Desabilitar o serviço Docker para não iniciar automaticamente na inicialização do sistema:
```bash
sudo systemctl disable docker
```
* Verificar o status do serviço Docker:
```bash
sudo systemctl status docker
```
* Baixar uma imagem de um servidor não seguro
```bash
sudo nano /etc/docker/daemon.json
```
Adicione o seguinte conteúdo:
```json
{
  "insecure-registries" : [
    "seu_servidor:porta"
 ]
}
```
Restarte o serviço Docker:
```bash
sudo systemctl restart docker
```

#### Build de uma imagem Docker
* Build de uma imagem Docker a partir de um Dockerfile padrão:
```bash
docker build -t nome_da_imagem:tag .
```
* Build de uma imagem com Dockerfile.personalizado:
```bash
docker build -t nome_da_imagem:tag -f Dockerfile.personalizado .
```

Os comandos acima criam uma imagem Docker com o nome `nome_da_imagem` e a tag `tag` a partir do Dockerfile localizado no diretório atual.

#### Listar imagens Docker
* Listar todas as imagens Docker disponíveis localmente:
```bash
docker images
```
* Remover uma imagem Docker:
```bash
docker rmi nome_da_imagem:tag
```
* Remover uma imagem forçada (mesmo que esteja em uso por contêineres):
```bash
docker rmi -f nome_da_imagem:tag
```
* Remover todas as imagens não utilizadas:
```bash
docker image prune -a
```
#### Criar uma rede Docker
* Criar uma rede Docker personalizada:
```bash
docker network create nome_da_rede
```
* Listar redes Docker:
```bash
docker network ls
```
* Remover uma rede Docker:
```bash
docker network rm nome_da_rede
```

#### Executar um contêiner Docker
* Criar e executar um container:
```bash
docker run -d --restart always --network nome_da_rede --name nome_do_container -v /caminho/para/diretorio:/caminho/no/container -p porta_host:porta_container nome_da_imagem:tag
```
* Listar contêineres em execução:
```bash
docker ps
```
* Listar todos os contêineres (em execução e parados):
```bash
docker ps -a
```
* Parar um contêiner em execução:
```bash
docker stop nome_do_container
```
* Iniciar um contêiner parado:
```bash
docker start nome_do_container
```
* Remover um contêiner parado:
```bash
docker rm nome_do_container
```
* Acessar o terminal de um contêiner em execução:
```bash
docker exec -it nome_do_container /bin/bash
```
* Visualizar logs de um contêiner:
```bash
docker logs nome_do_container
```
* Visualizar logs em tempo real de um contêiner:
```bash
docker logs -f nome_do_container
```
* Remover todos os contêineres parados:
```bash
docker container prune
```
#### Docker Compose
* Iniciar serviços definidos no arquivo docker-compose.yml:
```bash
docker-compose up -d
```
* Parar serviços definidos no arquivo docker-compose.yml:
```bash
docker-compose down
```
* Visualizar logs dos serviços:
```bash
docker-compose logs -f
```
* Recriar serviços (útil após alterações no docker-compose.yml):
```bash
docker-compose up -d --build
```
* Listar contêineres gerenciados pelo Docker Compose:
```bash
docker-compose ps
```
* Remover volumes não utilizados:
```bash
docker volume prune
```