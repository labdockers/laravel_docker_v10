****
[INSTAGRAM](https://www.instagram.com/wesllycode/)
[LINKEDIN](https://www.linkedin.com/in/weslly-sousa-a0bb2647/)
[DEV.TO](https://dev.to/wesllycode)
[TWITTER](https://twitter.com/wesllycode)

# SOBRE O PROJETO
- Uma estrutura completa para criar um ambiente no docker para se trabalhar com Laravel. 
- Essa estrutura inicial é excelente para desenvolver uma SPA com Laravel ou ambiente para API

# ESTRUTURA
 * [Laravel](https://laravel.com)
 * [nginx:alpine](https://hub.docker.com/_/nginx) - [versions](https://nginx.org/en/CHANGES)
 * [php:8.1.0-fpm](https://hub.docker.com/_/php)
 * [redis:alpine](https://hub.docker.com/_/redis)
 * [phpmyadmin:latest](https://hub.docker.com/_/phpmyadmin)
 * [Mysql 5.7.22](https://hub.docker.com/_/mysql)
 * [Node 16 LTS](https://github.com/nodesource/distributions#debmanual)
 * [Mail Hog](https://github.com/mailhog/MailHog)


 # PROCEDIMENTO A SER EXECUTADO

Clone o repositório 
```sh
git clone https://github.com/labdockers/laravel_docker_v10.git
```

Acesse a pasta laravel_docker_v10 e clone o repositório para criar um projeto do Laravel mais recente
```sh
git clone https://github.com/laravel/laravel.git nome-do-meu-projeto
```
<br>

**Observação:**
 - Você pode renomear em vez de *nome-do-meu-projeto* para o nome *app* ou qualquer outro tipo de nome que identifique o seu projeto. 
   não esqueça de substituir os volumes no arquivo docker-compose para caminho da sua pasta do laravel.
 - Na caminho da pasta ./nome-da-pasta/ ele vai copiar o que tiver dentro desssa pasta e copiar para /var/www dentro do container. Nos volumes coloque sempre o caminho
   da pasta onde está  sua aplicação laravel.


 ```sh
 volumes:
    - ./nome-do-meu-projeto/:/var/www
 ```

**Observação:**
 - Esses comandos funciona em ambient linux.

<br>

Acesse a pasta do seu projeto (nome-do-meu-projeto) e vamos criar o arquivo .env
```sh
cp -r .env.example .env
```

<br>

**Observação:** 
- Você pode mudar nome da sua pasta nome-do-meu-projeto para o que você quiser.

<br>

No seu arquivo **docker-compose**, uma observação importante, **DB_HOST**, **CACHE_DRIVER**, **REDIS_HOST** todos eles tem que ter o nome igual do seu container que está configurado no docker-compose. Automaticamente o docker faz referência e identifica o IP do container e acrescenta no ambiente de variável do .env, por isso é suficiente só colocar nome do container em vez do ip.

----

Vamos fazer o deploy dos containers do projeto
```sh
docker-compose up -d
```

Para acessar o container
```sh
docker-compose exec nomedomeuprojeto bash
```

Execute os seguintes comandos
```sh
composer install
```
depois
```sh
php artisan key:generate
```
e depois
```sh
php artisan config:cache
```
e para finalizar o 
```sh
npm install
```

- Para acessar a sua aplicação  http://localhost:8180
- Para acessar o phpmyadmin http://localhost:8181
- Para acessar o e-mail http://localhost:8100/

<br>

**Observação:**
- Não esqueça de adicionar no seu arquivo **.gitignore** a pasta **/docker/mysql/*** 
- Caso tente refazer build da sua aplicação com **docker-compose up -d --build** e se aparecer um  erro, 
  verifique  a pasta **docker/mysql/dbdata** se ela estiver protegida para escrever, alterar ou excluir, use o esse
  comando do linux para da permissão na pasta.
- Não esqueça que para executar esse comando, abra o terminal dentro da pasta **docker/mysql/**

```sh
  chmod -R 777 dbdata
```

<br>

## ALGUMAS ALTERAÇÕES QUE VOCÊ PODE PERSONALIZAR
Sim, é só ir nas configurações docker-compose e alterar as portas do serviço.
Por exemplo, quero de 8181

```sh
ports:
        - 8181:80
```
para 8182
```sh
ports:
        - 8182:80
```


No docker-compose, você pode alterar o nome do network de acordo com o nome do seu projeto, para ficar mais fácil identificar o projeto que está trabalhando.
Não esqueça de alterar de todos os containers.
```sh
networks:
    - nome-da-minha-aplicacao
```

Você pode mudar o nome do container do seu projeto, veja o exemplo abaixo

```sh
 container_name: alguma coisa
```
para
```sh
 container_name: para-nome-da-minha-aplicacao 
```

Se mudar o nome do serviço, por exemplo
```sh
services:
    # project
    app:
        container_name: nome-da-minha-aplicacao
        build:
```        
de
```sh
services:
    # project
    nome-novo-aqui:
        container_name: nome-da-minha-aplicacao
        build:
```  
Presta atenção no documento todo e onde estiver nome app troca pelo nome novo que colocou.


E também Não esqueça de atualizar também no arquivo docker/nginx/laravel.conf
```sh
fastcgi_pass app:9000;
```

para
```sh
fastcgi_pass nome-novo-aqui:9000;
```
## COMANDOS DOCKER

Para parar todos containers de uma vez
```sh
docker stop $(docker ps -qa)
```

Para remover os containers
```sh
docker rm $(docker ps -qa)
```

Para remover todas as imagens de uma vez
```sh
docker rmi $(docker images -qa)
```

Para remover uma imagem especifica a força
```sh
docker rmi -f aqui_voce_digita_codigo_da_imagem
```
- **Observação:** Não precisa digitar o código todo,  os 3 primeiros caracteres do código já serve.
