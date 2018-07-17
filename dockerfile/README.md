## Build das imagens

Acessar as pastas dos dockerfiles ./dockerfile

```
# autenticar no registry
docker login registry-domain-aqui

# caso precise buildar novamente a imagem e atualizar o registry
docker build -t registry-domain-aqui/centos7-php-mssql:7.2 . -f ./centos7_php72_mssql --no-cache

docker build -t registry-domain-aqui/centos7-angular:latest . -f ./centos7_angular --no-cache

# subir a imagem para basis
docker push registry-domain-aqui/centos7-php-mssql:7.2

docker push registry-domain-aqui/centos7-angular:latest

```
