# Damabox

O Damabox é uma alternativa ao XAMPP construída sobre o Docker. Com ele, você pode rodar sua aplicação PHP com servidor Nginx.

## Modo de usar

Crie um arquivo **.env** na raíz do Damabox se baseando no arquivo **env-example**. Você pode serguir os passos abaixo:

``` bash
# Crie um arquivo de configuração de ambiente
$ cp env-example .env

# Edite o arquivo de configuração (opcional)
$ vim .env

# Crie um arquivo php.ini dentro de config/php/VERSAO
# para sobrescrever as configurações default do PHP (opcional).
# Verifique o arquivo de exemplo config/php/7.1/php.ini-example
$ vim config/php/7.1/php.ini

# Inicie todos os containers
$ docker-compose up
```

Segue um exemplo do arquivo **.env**:

```
###########################################
# Damabox
###########################################

DB_CONFIG_DIR=./config/db
PHP55_CONFIG_DIR=./config/php/5.5
PHP56_CONFIG_DIR=./config/php/5.6
PHP70_CONFIG_DIR=./config/php/7.0
PHP71_CONFIG_DIR=./config/php/7.1

###########################################
# Project - Nginx
###########################################

PROJECT_DIR=./data/www

HTTP_PORT=80
NGINX_LOG_DIR=./logs/nginx
NGINX_CONFIG_DIR=./config/nginx

###########################################
# Firebird
###########################################

FIREBIRD_ROOT_USER=sysdba
FIREBIRD_ROOT_PASSWORD=masterkey
FIREBIRD_PORT=3050
FIREBIRD_DATADIR=./data/databases/firebird

###########################################
# MySQL
###########################################

MYSQL_TYPE=mysql
MYSQL_VERSION=5.7
MYSQL_PORT=3306
MYSQL_ROOT_PASSWORD=root
MYSQL_DATADIR=./data/databases/mysql
```

Dentro da pasta **config/nginx/servers** há um exemplo de configuração de server do Nginx que deverá ser duplicado com extensão **.conf**. O conteúdo do arquivo pode ser como no exemplo abaixo:

```
server {
    listen   80;

    root /app;
    index index.php index.html;

    autoindex on;

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php-7.1:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

Para sobrescrever as configurações default do PHP, crie um arquivo **php.ini** dentro do diretório **config/php/VERSAO-UTILIZADA** com os parâmetros que deseja sobrescrever.

Por padrão, os arquivos de banco de dados ficam em **data/databases/TIPO-DE-BANCO-DE-DADOS**. Os diretórios com seus projetos, por padrão, devem estar no diretório **data/www**. Esses diretórios podem ser alterados pelo arquivo **.env**.

## Contribuições

Toda contribuição é bem vinda!

Se gostou do projeto e quiser contribuir, faça um clone do projeto. Você também pode reportar bugs caso ache algum problema.

Não se esqueça de dar uma estrela no repositório :)

## Todo

- ~~Suporte ao PHP~~
- ~~Suporte ao Nginx~~
- ~~Suporte ao Firebird~~
- ~~Suporte ao MySQL~~
- Suporte ao Apache
- Suporte ao PostgreSQL
- Suporte ao MongoDB
- Suporte ao Ruby
- Suporte ao Node.js
