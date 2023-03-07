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

