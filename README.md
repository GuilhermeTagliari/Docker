# Projeto de Conteinerização: Docker Nginx PHP MySQL

## Integrantes:
- Guilherme Tagliari - 1134870
- Lorenzo Pasa - 1134869
- João Ricardo - 1134269
- Alan Godoi - 1135335

## Descrição

Este projeto utiliza a conteinerização para rodar um ambiente de desenvolvimento com Nginx, PHP-FPM, MySQL, Composer e PHPMyAdmin, usando Docker e Docker Compose. A aplicação foi configurada para ser executada de forma simples e eficiente em containers, facilitando o desenvolvimento e a implantação.

## Imagens Utilizadas

- **Nginx**: Servidor web.
- **MySQL**: Banco de dados.
- **PHP-FPM**: Processamento de scripts PHP.
- **Composer**: Gerenciador de dependências PHP.
- **PHPMyAdmin**: Interface de gerenciamento do MySQL.

## Como Executar o Projeto

### Pré-Requisitos

Para rodar os comandos Docker sem o `sudo`, adicione o usuário ao grupo Docker:

```bash
sudo usermod -aG docker your-user

Verifique se o Docker e o Docker Compose estão instalados:

which docker-compose

Se necessário, instale os pacotes:

    Git: Controle de versão.
    Docker: Plataforma para conteinerização.
    Docker Compose: Ferramenta para orquestrar containers.

Verifique se o Docker Compose está instalado corretamente com:

which docker-compose

Clonando o Projeto

Clone o repositório para a sua máquina local:

git clone https://github.com/GuilhermeTagliari/Docker.git

Vá para o diretório do projeto:

cd docker-nginx-php-mysql

Estrutura do Projeto

A estrutura de diretórios do projeto é a seguinte:

.
├── Makefile
├── README.md
├── data
│   └── db
│       ├── dumps
│       └── mysql
├── doc
├── docker-compose.yml
├── etc
│   ├── nginx
│   │   ├── default.conf
│   │   └── default.template.conf
│   ├── php
│   │   └── php.ini
│   └── ssl
└── web
    ├── app
    │   ├── composer.json.dist
    │   ├── phpunit.xml.dist
    │   ├── src
    │   │   └── Foo.php
    │   └── test
    │       ├── FooTest.php
    │       └── bootstrap.php
    └── public
        └── index.php

Configurando o Ambiente
Gerar Certificados SSL para Nginx

Para garantir a segurança da conexão, você pode gerar certificados SSL para o Nginx:

source .env && docker run --rm -v $(pwd)/etc/ssl:/certificates -e "SERVER=$NGINX_HOST" jacoelho/generate-certificate

Configurar o Xdebug

Caso deseje usar o Xdebug para depuração no PHP, edite o arquivo etc/php/php.ini e configure o IP remoto conforme o seu ambiente. Para o PHPStorm, consulte a documentação oficial.
Rodando a Aplicação

Após configurar o ambiente, inicie os containers com o comando:

docker-compose up -d

Este comando irá rodar os containers em segundo plano. Para visualizar os logs:

docker-compose logs -f

Agora, você pode acessar os seguintes serviços no navegador:

    Nginx: http://localhost:8000
    PHPMyAdmin: http://localhost:8080 (usuário: dev, senha: dev)

Parando e Limpando os Containers

Para parar e remover os containers, redes e volumes:

docker-compose down -v

Usando o Makefile

Se você tiver o Make instalado, pode usar comandos do Makefile para facilitar o desenvolvimento. Exemplos de comandos:

    Start containers: make docker-start
    Gerar documentação: make apidoc
    Atualizar dependências PHP: make composer-up

Exemplos de Comandos Docker

    Instalar pacotes com o Composer:

docker run --rm -v $(pwd)/web/app:/app composer require symfony/yaml

Atualizar dependências PHP com o Composer:

docker run --rm -v $(pwd)/web/app:/app composer update

Testar a aplicação com PHPUnit:

docker-compose exec -T php ./app/vendor/bin/phpunit --colors=always --configuration ./app

Verificar extensões do PHP:

    docker-compose exec php php -m

Trabalhando com Banco de Dados MySQL

    Acessar o shell do MySQL:

docker exec -it mysql bash
mysql -u"$MYSQL_ROOT_USER" -p"$MYSQL_ROOT_PASSWORD"

Criar backup de todos os bancos de dados:

mkdir -p data/db/dumps
source .env && docker exec $(docker-compose ps -q mysqldb) mysqldump --all-databases -u"$MYSQL_ROOT_USER" -p"$MYSQL_ROOT_PASSWORD" > "data/db/dumps/db.sql"

Restaurar backup de um banco de dados:

    source .env && docker exec -i $(docker-compose ps -q mysqldb) mysql -u"$MYSQL_ROOT_USER" -p"$MYSQL_ROOT_PASSWORD" < "data/db/dumps/db.sql"

Conclusão

Este projeto fornece uma solução completa e fácil de configurar para rodar uma aplicação PHP com Nginx e MySQL, utilizando Docker e Docker Compose. Você pode desenvolver e testar localmente, além de utilizar ferramentas como PHPMyAdmin para gerenciar seu banco de dados MySQL.
Fontes Utilizadas

    Docker Compose
    Nginx
    PHP-FPM
    MySQL
    PHPMyAdmin
