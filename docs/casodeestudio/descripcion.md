
## Descripción
La aplicación consiste en el monitoreo ambiental a través de dispositivos que envíen datos de contaminantes a una infraestructura de cómputo en la nube para ser consumidos y procesados por diferentes aplicaciones.
Es importante contar con la información histórica capturada por los dispositivos para su posterior análisis y visualización.
El análisis de la evolución de contaminantes se realiza a través de series de tiempo (una secuencia de datos indexados en función del tiempo) en donde la visualización de los datos es de gran ayuda para su interpretación.


[Datos abiertos de la calidad del aire de la Ciudad de México](./datosabiertos//airecdmx.md)

## Esquema de datos en FIWARE

A continuación se describirá el proceso (paso a paso) para desplegar y utilizar el componente de FIWARE Orion context Broker y servicios adicionales para almacenar datos históricos y su visualización tomando como ejemplo el modelo de datos de calidad del aire.

El diagrama presenta el flujo de información que seguiŕa el ejercicio:

![Esquema de datos en FIWARE](./images//esquemadatos.png)


- Orion Context Broker
- [CrateDB](./herramientas//cratedb.md)
- [Grafana](./herramientas//grafana.md)


## Requisitos de Software

Dichos servicios se instalarán a través de la plataforma de virtualización [Docker](./herramientas//docker.md) https://docs.docker.com/

[Docker](./herramientas//docker.md) https://docs.docker.com/

Adicionalmente utilizamos un cliente para realizar llamadas REST a las APIs (Application Programming Interface) de los servicios.

https://insomnia.rest/
https://www.getpostman.com/


## Instalación del ambiente

### Dependiendo del sistema operativo utilizado, realizar la instalación de Docker, docker-compose (sólo Linux)
- Linux
- Windows
- macOS

- Crear una carpeta *calidadaire* y archivo *docker-compose.yml*

```
calidadaire
├── docker-compose.yml
├── rawdata.json
```

El archivo describe cada uno de los contenedores a utilizar.

(a manera de ejemplo, docker-compose se utiliza, para instalar los servicios adecuadamente es necesario utilizr docker stack deploy (https://docs.docker.com/engine/reference/commandline/stack_deploy/) )


La estructura del archivo docker-compose (https://docs.docker.com/compose/overview/) consiste en describir cada uno de los servicios que se depleglarán así como
instrucciones específicas par cada uno, p.ej. los puertos por donde se accederá, variables de entorno, si depende de algún otro servicio, comandos en shell, etc.

La etiqueta *image* indica el nombre de la imagen seguido de una etiqueta : , normalmente es la versión de la imagen

p. Ej. el contenedor *orion* se crea a apartir de la imagen *fiware/orion* con la etiqueta *1.14.0*, que descrito en el archivo docker-compose es:

```
orion:
  image: fiware/orion:1.14.0
```

Docker cuenta con un repositorio de imagenes https://hub.docker.com/


El siguiente archivo depliega los servicios:
- orion
- quantumleap
- crate
- grafana


```
version: '3'
services:

    orion:
        image: fiware/orion:1.14.0
        ports:
          - "1026:1026"
        command: -dbhost smart-sdk-mongodb
        depends_on:
          - smart-sdk-mongodb
        healthcheck:
          test: ["CMD", "curl", "-f", "http://0.0.0.0:1026/version"]
          interval: 1m
          timeout: 10s
          retries: 3

    quantumleap:
      image: smartsdk/quantumleap
      ports:
        - "8668:8668"
      depends_on:
        - smart-sdk-mongodb
        - smart-sdk-orion
        - crate
      environment:
        - CRATE_HOST=crate

    crate:
      image: crate:1.0.5
      ports:
        # Admin UI
        - "4200:4200"
        # Transport protocol
        - "4300:4300"
      command: -Ccluster.name=democluster -Chttp.cors.enabled=true -Chttp.cors.allow-origin="*"

    grafana:
      image: grafana/grafana
      ports:
        - "3000:3000"
      environment:
        - GF_INSTALL_PLUGINS=crate-datasource,grafana-clock-panel
      depends_on:
        - crate
```


en una consola o terminal, acceder a la ubicación de la carpeta *calidadaire*
y ejecutar el comando, el cual :

docker-compose up -d


Once the services are running, you can access Grafana on the **0.0.0.0:3000** CrateDB on **0.0.0.0:4200** and QuantumLeap in **0.0.0.0:8668**.


## Entidad de Calidad del Aire en Orion Context Broker

Siguiendo el modelo de datos AirQualityObserved


### Creación de entidades

Utilizando insomnia (o postman),crearemos una entidad que representará una unidad de monitoreo de calidad del aire, utilizando el modelo de datos AirQualityObserved:

### Consulta de entidades

### Actualización de datos y eliminación de una entidad



## Quantumleap

Verify that QuantumLeap is working correclty by querying:

```
0.0.0.0:8668/v2/version
```


### Suscripción de entidades de calidad del aire

1. Create a subscription in OCB for AirQualityObserved where the notifications fall into QuantumLeap's endpoint ***http://0.0.0.0:8668/v2/notify***.

```
curl -v localhost:1026/v2/subscriptions -s -S -H 'Content-Type: application/json' --header "Fiware-Service:airquality" --header "Fiware-ServicePath:/" -d @- <<EOF
 {
  "description": "QuantumLeap AirQaulityCDMX",
  "subject": {
    "entities": [
      {
        "idPattern": ".* ",
        "type": "AirQualityObserved"
      }
    ],
    "condition": {
      "attrs": [
        "CO",
        "O3",
        "PM10",
        "SO2",
        "NO2",
        "temperature",
        "relativeHumidity",
        "dateObserved"
      ]
    }
  },
  "notification": {
    "attrs": [
      "id",
      "CO",
      "O3",
      "PM10",
      "SO2",
      "NO2",
      "temperature",
      "relativeHumidity",
      "dateObserved",
      "address",
      "location",
      "latitude",
      "longitude"
    ],
    "attrsFormat": "normalized",
    "http": {
      "url": "http://0.0.0.0:8668/v2/notify"
    },
    "metadata": [
      "dateCreated",
      "dateModified"
    ]
  },
  "throttling": 1
}
EOF
```

### API QuantumLeap



## CreateDB

### Acceso a datos en CrateDB

We can access cratedb via command-line or through web browser in the **localhost:4200**

1. To access via console, connect to the container:

```
docker exec -ti cratedb /bin/sh
```

2. Call the cratedb bash console (named crash) from inside the container:
```
crash
```

### Consultas



- Creación de tablas
CREATE TABLE IF NOT EXISTS "doc"."etstation" (
  "entity_id" STRING,
  "name" STRING,
  "acron" STRING,
  PRIMARY KEY ("entity_id")
)

CREATE TABLE IF NOT EXISTS "doc"."etpollutant" (
  "idpollutant" INTEGER,
  "name" STRING,
  "namefront" STRING,
  PRIMARY KEY ("idpollutant")
)

insert into etstation (entity_id, name, acron) values('CDMX-AmbientObserved-484150020109','Acolman','ACO');

## Grafana

### Acceso a panel de administración


###1. Instalar contenedor grafana.
docker run -d -p 3000:3000 --name=grafana -e "GF_INSTALL_PLUGINS=crate-datasource,grafana-clock-panel,grafana-worldmap-panel" grafana/grafana

###2. Obtener key para uso de API GRafana

Grafana comes with an API which lets you manage many options including the configuration of dashboards and datasources.

1. As a first step, you need to obtain a key to use the API:

```
POST http://admin:admin@localhost:3000/api/auth/keys
Content-Type: application/json
{"name":"apikeycurl", "role": "Admin"}
```
It returns a key that you'll need to send as an "Authorization" header in the following steps.
```
{
	"name": "apikeycurl",
	"key": "eyJrIjoiUDNGQlM5YldXbUdVU2JreDJiVkZDYW81aWZCTlZFSlkiLCJuIjoiYXBpa2V5Y3VybCIsImlkIjoxfQ=="
}
```



### Creación de Datasource

2. Create a crateDB datasource by indicating the crateDB URL within the json payload ***url: http://localhost:4200***.

```
POST http://localhost:3000/api/datasources
Content-Type: "application/json"
Authorization: "Bearer eyJrIjoiUDNGQlM5YldXbUdVU2JreDJiVkZDYW81aWZCTlZFSlkiLCJuIjoiYXBpa2V5Y3VybCIsImlkIjoxfQ=="

{
  "id": null,
  "orgId": 1,
  "name": "AIRQUALITY",
  "type": "crate-datasource",
  "typeLogoUrl": "public/plugins/crate-datasource/img/crate_logo.png",
  "access": "proxy",
  "url": "http://localhost:4200",
  "password": "",
  "user": "",
  "database": "",
  "basicAuth": false,
  "isDefault": true,
  "jsonData": {
    "keepCookies": [],
    "schema": "doc",
    "table": "etairqualityobserved",
    "timeColumn": "dateobserved",
    "timeInterval": "auto_gf"
  },
  "readOnly": false
}
```


### Creación de Dashboard

- Conexión y consultas a CrateDB




# Install application with swarm (step by step using docker-machine VMs)

**Pre-requisites:**

- Install virtualbox
```
sudo apt-get install virtualbox
```
- Install docker-machine

https://docs.docker.com/machine/install-machine/#install-machine-directly

1. Create two VMs

```
docker-machine create --driver virtualbox vm1
docker-machine create --driver virtualbox vm2
```

2. Set main node for swarm.

```
docker-machine ls  #To see the virtual ip adress of the main node
docker-machine ssh vm1 "docker swarm init --advertise-addr 192.168.99.100"
```

Returns token to add workers to the swarm

3. Add worker node to the swarm
```
docker-machine ssh vm2 "docker swarm join --token SWMTKN-1-4kwbrascwqpye99rcs252okjgbj67eg5hpachf9dppkh6x5ff7-3dxfvrmtzd71yyezx2e0l1v7z 192.168.99.100:2377"

docker-machine ssh vm1 "docker node ls"
```
4. Use environment from main docker node

```
docker-machine env myvm1
eval $(docker-machine env myvm1)
```

5. Deploy application using the docker-compose file
```
docker stack deploy -c docker-compose.yml greenroute
docker stack ps greenroute
```

6. Remove application
```
docker stack rm greenroute
```

7. Close docker main node environment
```
eval $(docker-machine env -u)
```


## Referencias
