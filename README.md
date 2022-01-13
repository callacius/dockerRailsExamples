# Exemplos de uso do docker com Rails

# Dockerfile
Exemplo de uso de dockerfile para usar junto ao docker ou docker-compose, substituir '{nome do projeto}'.
```dockerfile
FROM ruby:2.7.1
 
# add nodejs and yarn dependencies for the frontend
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
 
# Instala nossas dependencias
RUN apt-get update && apt-get install -qq -y --no-install-recommends \
nodejs yarn build-essential libpq-dev imagemagick git-all nano
# Seta nosso path
ENV INSTALL_PATH /"{nome do projeto}"
# Cria nosso diretório
RUN mkdir -p $INSTALL_PATH
# Seta o nosso path como o diretório principal
WORKDIR $INSTALL_PATH
# Copia o nosso Gemfile para dentro do container
COPY Gemfile ./
# Seta o path para as Gems
ENV BUNDLE_PATH /gems
# Copia nosso código para dentro do container
COPY . .
```
# Exemplos de "docker-compose.yml"
PostgreSQL
```yml
version: "3.8"
 
services:
  db:
    image: "postgres:12.2"
    environment: 
      - POSTGRES_PASSWORD=postgres
    volumes:
      - postgres:/var/lib/postgresql/data
 
  app:
    build: .
    command: bash start.sh
    ports:
      - "3000:3000"
    environment:
      - DB_PASSWORD=postgres
    volumes:
      - .:/rentalcars
      - gems:/gems
    depends_on:
      - db
    
volumes:
  postgres:
  gems:
```
# Criar "start.sh" parar iniciar Servidor 
```sh
# Instala as Gems
bundle check || bundle install
 
# Roda nosso servidor
bundle exec puma -C config/puma.rb
```


# Comandos Rails docker
Criar container temporário para criar o projeto em ruby.
```sh
$ docker run --rm --user "$(id -u):$(id -g)" -v $(pwd):/usr/src -w /usr/src -ti ruby:2.7.1 bash
$ gem install rails
```
Exemplos de criar projetos Rails.
```sh
$ rails new {nome do projeto} --database=mysql --skip-bundle
$ rails new {nome do projeto} --database=postgresql --skip-bundle
$ rails new {nome do projeto} --api --database=sqlite3 --skip-bundle
```
Instalar gems com docker-compose
```sh
$ docker-compose run --rm app bundle install
```
instalar rspec com docker-compose 
```sh
$ docker-compose run --rm app bundle exec rails g rspec:install
```
Instalar devise com docker-compose 
```sh
$ docker-compose run --rm app bundle exec rails g devise:install
$ docker-compose run --rm app bundle exec rails g devise:views --views=sessions registrations
$ docker-compose run --rm app bundle exec rails g devise User
```
Instalar webpacker com docker-compose 
```sh
$ docker-compose run --rm app bundle exec rails webpacker:install
```
Criar banco de dados e executar migrations
```sh
$ docker-compose run --rm app bundle exec rails db:create db:migrate
```
Gerar controller example
```sh
$ docker-compose run --rm app bundle exec rails g controller home index
```
Instalar bootstrap
```sh
$ docker-compose run --rm app bundle exec yarn add bootstrap
```
Abrir credentials do rails
```sh
$ docker-compose run --rm -e EDITOR=nano app bundle exec rails credentials:edit
```
Construir projeto Rails
```sh
$ docker-compose build
```
Subir projeto Rails
```sh
$ docker-compose up
```
Subir projeto Rails em segundo plano
```sh
$ docker-compose up -d
```
Criar model forçado Rails example
```sh
$ docker-compose run --rm app bundle exec rails generate model Product id:Integer nome:string preco_custo:double --force
```
Destruir model forçado Rails example
```sh
$ docker-compose run --rm app bundle exec rails destroy scaffold Product --force
```
Criar todas as estruturas
```sh
$ docker-compose run --rm app bundle exec rails g scaffold Product id:Integer nome:string preco_custo:double --force
```

# gerando um novo aplicativo em rails sem ruby instalado
```sh
$ docker run -i -t -rm -v ${PWD}:/usr/src/app ruby:2.6 bash
$ cd /usr/src/app
$ gem install rails
$ rails new myapp --skip-test --skip-bundle
$ exit
```

# Comandos docker

Lista de containers em execução
```sh
$ docker ps
```
Lista de todos os containers
```sh
$ docker ps -a
```
Parar todos os containers
```sh
$ docker stop $(docker ps -a -q)
```
Excluir um container específico
```sh
$ docker rm {'Preencher com Id do container'}
```
Excluir todos os containers
```sh
$ docker rm $(docker ps -a -q)
```
Excluir todas as imagens
```sh
$ docker image rm $(docker image ls -a -q)
```
Excluir todos os volumes
```sh
$ docker volume prune
```
