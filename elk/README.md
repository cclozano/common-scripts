# Configuracion de ELK stack
### la clave establecida en todos los archivos de configuracion es: "mypassword"
### y la ip de mi servidor de pruebas es: "192.168.100.4"
### por lo cual estos valores deben ser remplazados por su password y la ip de su server respectivamente
### 
###
### El stack esta configurado para levantar 3 nodos del elastic, por lo cual si no tienen configurados inicialmente los valores de memoria correctos, se deben ejecutar los siguientes comandos:

```
sudo sysctl -w vm.max_map_count=262144
```
### luego entramos como root con un:

```
sudo su
```
### y ejecutamos:
```
sudo echo "vm.max_map_count=262144" >> /etc/sysctl.conf
```

con esto ya podemos levantar el stack del elastic, logstash y kibana desde el directorio de este archivo usando:

```
docker-compose up
```

### para levantar el filebeat que lea los logs de los contenedores nos colocamos en el directorio:
```
cd filebeat/containers
```
### y levantamos el contenedor con:
```
docker-compose up --build
```
### si queremos levantar el filebeat para cargar logs de un directorio lo hacemos desde el directorio: filebeat/directories