# AWS-Docker 

<img src="https://img.shields.io/static/v1?label=Release&message=3.0&color=blue&style=for-the-badge"/>

## Tecnologias Utilizadas
<img style="width:70px; height:70px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original-wordmark.svg" /> <img style="width:70px; height:70px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" /><img style="width:70px; height:70px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/wordpress/wordpress-original.svg" /> <img style="width:70px; height:70px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original-wordmark.svg" />
          
          

## Atividade pratica de docker 

## Descrição 

- [x] 1- Instalação e configuração do DOCKER ou CONTAINERD no host EC2, utilizar a instalação via script de start instance(user_data.sh);         
- [x] 2- Efetuar Deploy de uma aplicação Wordpress com container de aplicação e container database Mysql;
- [x] 3- Configuração da utilização do serviço EFS AWS para estáticos do container de aplicação Wordpress;
- [x] 4- Configuração do serviço de Load Balancer AWS para aplicação Wordpress;
          
## Pontos adicionais
* Não utilizar ip público para saída do serviços WP (Evitem publicar o serviço WP via IP Público);
* Sugestão para o tráfego de internet sair pelo LB (Load Balancer Classic)pastas públicas e estáticos do wordpress; 
* Sugestão de utilizar o EFS (Elastic File Sistem);
* Fica a critério de cada integrante (ou dupla) usar Dockerfile ou Dockercompose;
* Necessário demonstrar a aplicação wordpress funcionando (tela de login);
* Aplicação Wordpress precisa estar rodando na porta 80 ou 8080;
* Utilizar repositório git para versionamento;
* Criar documentação;

### Configuração da Instância
*Tipo de Instância* | *AMI* | *Armazenamento* 
---|---|---
T3.Small  | AWS Linux | 16GiB (gp2)

## User Data
~~~~~~~~~~
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo mv /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
~~~~~~~~~~

## Configurando o EFS
- Instalação
~~~~~~~~
sudo yum -y install nfs-utils
~~~~~~~~
- Criando Diretório
~~~~~~~~
sudo mkdir /efs
~~~~~~~~
- Dando um mount
~~~~~~
 sudo mount -t nfs -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport mount-target-IP:/   /efs/
~~~~~~

## Configurando o fstab para deixar o efs permanente
~~~~~~
UUID=3443a7e4-b334-4ab5-a41f-6389879ce2fe    /efs         xfs    defaults          0   0 
~~~~~~

## docker-compose.yml
Crie o arquivo docker-compose.yml na pasta montada
~~~~~~
version: '3'
services:
  db:
    image: mysql:8.0.19
    volumes:
      - /efs/CarlaDaniela/db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: DFf18cc++
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    volumes:
      - /efs/CarlaDaniela/db_data:/var/lib/wordpress
 ~~~~~~
Execute o comando abaixo para realizar o deploy da aplicação(é necessário esta no mesmo diretório do arquivo docker-compose.yml):
~~~~
sudo docker-compose up -d
~~~~

## Configurando Target Group e Aplication Load Balancer
### Target group:
* Vá no serviço de EC2 e escolha a opção de target Group 
* Selecione Create Target Group 
* Escolha a opção Instance 
* Crie um nome do Target Group de sua preferência
* Escolha a mesma porta que voce esta utilizando para a aplicação wordpress
* Escolha a VPC que esta utilizando
* Health Checks: HTTP
* Selecione a porta e clique em Include as pending below
*  Clique em Create Target Group

### Aplication Load Balancer:
* Vá no serviço de Application Load Balancer e clique em Create Application Load Balancer
* Escolha um nome para seu Load balance
* No Scheme você deixará a opção internal-facing
* O tipo de endereço de IP será o IPv4
* No Networking Map você irá selecionar sua VPC que deve ter duas subredes uma pública e outra privada
* Selecione Security groups 
* Selecione o Target Group 
* Clique em Create Load Balancer 

## Acessando a aplicação
Utilize o dns criado pelo loadbalancer:8080/ para verificar a aplicação




