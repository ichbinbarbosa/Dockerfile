DocumentRoot on c:/Apache24/htdocs

```
docker build -t ichbinbarbosa/php-ns:72 nanoserver\php\72
docker build -t ichbinbarbosa/php-ns:72-mssql nanoserver\php\72\mssql

docker push ichbinbarbosa/php-ns:72
docker push ichbinbarbosa/php-ns:72-mssql

docker pull ichbinbarbosa/php-ns:72
docker pull ichbinbarbosa/php-ns:72-mssql
```