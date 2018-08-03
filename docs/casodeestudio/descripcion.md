## Descripción
La aplicación consiste en el monitoreo ambiental a través de dispositivos que envíen datos de contaminantes a una infraestructura de cómputo en la nube para ser consumidos y procesados por diferentes aplicaciones.
Es importante contar con la información histórica capturada por los dispositivos para su posterior análisis y visualización.
El análisis de la evolución de contaminantes se realiza a través de series de tiempo (una secuencia de datos indexados en función del tiempo) en donde la visualización de los datos es de gran ayuda para su interpretación.

## Esquema de datos en FIWARE

A continuación se describirá el proceso (paso a paso) para desplegar y utilizar el componente de FIWARE Orion Context Broker y servicios adicionales para almacenar datos históricos y su visualización tomando como ejemplo el modelo de datos de calidad del aire.

El diagrama presenta el flujo de información que seguirá el ejercicio, en donde se crearán una serie de entidades con el modelo datos de Calidad del Aire en Orion Context Broker, a su vez, éste notificará al servicio QuantumLeap que interpretará el modelo de datos y lo transformará en registros de la base de datos CrateDB. Posteriormente, la herramienta de visualización Grafana consultará los datos almacenados indexados por el tiempo en que se notificaron las entidades.

![Esquema de datos en FIWARE](./images//esquemadatos.png)

Los servicios utilizados son:
- [Orion Context Broker](https://fiware-orion.readthedocs.io/en/1.11.0/index.html)
- [QuantumLeap](https://quantumleap.readthedocs.io/en/latest/)
- [CrateDB](https://crate.io/docs/crate/reference/en/latest/)
- [Grafana](http://docs.grafana.org/)

## Requisitos de Software

A manera de facilitar el despliegue de los servicios, utilizaremos la plataforma de virtualización [Docker](https://docs.docker.com/), con la cual no es necesario preparar un entorno de trabajo instalando software adicional debido a todos los elementos utilizados ya se encuentran contenidos dentro de imágenes virtuales proveídas por el repositorio de docker [DockerHub](https://hub.docker.com/)

Adicionalmente utilizamos un cliente para realizar llamadas REST a las APIs (Application Programming Interface) de los servicios.
Entre los clientes comúnmente utilizados se encuentran [Postman](https://www.getpostman.com/) e [Insomnia](https://insomnia.rest/), en la presente guía utilizaremos éste último.

## Preparación de ambiente

### Instalación de Docker

Dependiendo del sistema operativo utilizado, realizar la instalación de Docker y docker-compose (sólo Linux)

- [Windows](https://docs.docker.com/docker-for-windows/install/)
- [MacOS](https://docs.docker.com/docker-for-mac/install/)
- [Linux](https://docs.docker.com/install/linux/docker-ce/centos/) (CentOS, Debian, Ubuntu, Fedora).

CentOS 7+
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce

sudo systemctl start docker

sudo docker run hello-world

```

Ubuntu 16.04

```
sudo apt-get update

sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-get install docker-ce

sudo docker run hello-world
```
Si la instalación se realizó correctamente, el resultado de levantar el contenedor *hello-world* debe ser el siguiente:

![Docker Hello World](./images//hello.png)

### Despliegue de servicios

Una vez finalizada la instalación de Docker, podemos realizar el despliegue de los contenedores de forma sencilla mediante la definición de un archivo en formato *yaml* que contiene la información necesaria para su configuración.

1. Crear una carpeta llamada *calidadaire*

2. Dentro de la carpeta *calidadaire*, crear un archivo llamado *docker-compose-demo.yml*, como se muestra en la estructura:

```
calidadaire
├── docker-compose-demo.yml
```

El archivo [*docker-compose-demo.yml*]((https://docs.docker.com/compose/overview/)) describe cada uno de los contenedores a desplegar así como instrucciones específicas como los puertos por donde se accederá, variables de entorno, si depende de algún otro servicio, comandos shell, etc.

- Inicialmente se indica la versión de *docker-compose* que se utilizará. La versión más actual (Julio 2018) es la versión 3.

```
version: '3'
```

- Dentro de la etiqueta *services* se describen por secciones cada uno de los contenedores que se crearán, el usuario puede indicar los nombres que deseé para cada servicio. El archivo actual crea los servicios orion, mongo, quantumleap, crate, grafana:

```
version: '3'
services:

    orion:
    mongo:
    quantumleap:
    crate:
    grafana:
```

- La etiqueta **image** dentro de cada servicio indica el nombre de la imagen seguido de una etiqueta con la versión de la imagen.

p. Ej. el contenedor *orion* se crea a partir de la imagen *fiware/orion* con la etiqueta *1.14.0*, que descrito en el archivo docker-compose es:

```
orion:
  image: fiware/orion:1.14.0
```

El repositorio de imágenes de docker se encuentra en [hub.docker](https://hub.docker.com/)

- En la etiqueta **ports** se mapea el puerto del contenedor con el puerto de la máquina host.
P. Ej. el contenedor *orion* señala que el contenedor está expuesto por el puerto *1027*, el cual mapea al puerto 1026 de la máquina host, siguiendo el orden **Puerto_Host:Puerto_Contenedor**.
```
    orion:
        ports:
          - "1026:1027"
```

- La etiqueta **command** permite ejecutar comandos de consola posterior a la creación del contenedor.
P. Ej. Después de crear el contenedor *orion*, se ejecuta el comando *-dbhost mongo* para indicar el nombre del servidor de base de datos al cual OCB se conectará.
```
    orion:
        image: fiware/orion:1.14.0
        ports:
          - "1026:1026"
        command: -dbhost mongo
```
- En la sección **depends_on** se indica si un contenedor o servicio depende de otro para poderse ejecutar.
P.Ej. En la descripción del archivo *docker-compose.yml* el contenedor *orion* requiere que el contenedor *mongo* se despliegue previamente de manera correcta para poder ejecutarse.

- La sección **environment** es muy útil, en ella se pueden definir variables de entorno para utilizar en los contenedores desplegados.
P. Ej. el contenedor *quantumleap* define la varible *CRATE_HOST* cuyo valor es la referencia al contenedor *crate*.

La definición del archivo *docker-compose-demo.yml* despliega los servicios indicados previamente:

```
version: '3'

services:

  orion:
    image: fiware/orion:1.13.0
    ports:
      - "1026:1026"
    command: -logLevel DEBUG -noCache -dbhost mongo
    depends_on:
      - mongo
    healthcheck:
      test: ["CMD", "curl", "-f", "http://0.0.0.0:1026/version"]
      interval: 1m
      timeout: 10s
      retries: 3

  mongo:
    image: mongo:3.2
    ports:
      - "27017:27017"
    volumes:
      - mongodata:/data/db

  quantumleap:
    image: smartsdk/quantumleap
    ports:
      - "8668:8668"
    depends_on:
      - mongo
      - orion
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
    volumes:
      - cratedata:/data

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    environment:
      - GF_INSTALL_PLUGINS=crate-datasource,grafana-clock-panel,grafana-worldmap-panel
    depends_on:
      - crate

volumes:
  mongodata:
  cratedata:
  redisdata:

networks:
    default:
        driver_opts:
            com.docker.network.driver.mtu: ${DOCKER_MTU:-1400}
```

***Nota: Para términos prácticos y de prueba, se utiliza docker-compose, sin embargo, para instalar los servicios en un entorno productivo (redundantes, balanceo de carga, en diferentes instancias) es necesario utilizar [docker stack deploy](https://docs.docker.com/engine/reference/commandline/stack_deploy/)***

3. En una consola o terminal (Linux, MacOS) o Windows PowerShell (Windows), acceder a la ubicación de la carpeta *calidadaire* y ejecutar el comando:
```
docker-compose -f docker-compose-demo.yml up -d
```
el cual procesa el archivo *docker-compose.yml* y levanta los servicios con las configuraciones descritas en modo detached (se lanza como proceso de fondo y puedes continuar trabajando en la consola).

4. Una vez que los servicios se despliegan correctamente, podemos verificar su funcionamiento accediendo a ellos a través de los puertos indicados en el *docker-compose* y verificando que los servicios están ejecutándose con el comando:

```
docker ps
```

![Servicios Docker desplegados](./images//dockerup.png)

- Orion Context Broker: **localhost:1026/version**
- QuantumLeap: **localhost:8668/v2/version**
- CrateDB: **localhost:4200**
- Grafana: **localhost:3000**

## Entidad de Calidad del Aire en Orion Context Broker

Utilizando el cliente para APIs REST [Insomnia](https://insomnia.rest/), crearemos y manipularemos una entidad de calidad del aire en OCB simulando una estación de monitoreo.

### Creación de entidades

Para la creación de la entidad, en Insomnia realizamos un **POST** request a OCB definiendo la entidad con los atributos del modelo de datos de FIWARE [AirQualityObserved](https://fiware-datamodels.readthedocs.io/en/latest/Environment/AirQualityObserved/doc/spec/index.html).

```
POST http://localhost:1026/v2/entities
Header: Content-Type: "application/json"

{
	"id":"AirQualityUnit01",
	"type":"AirQualityObserved",
	"dateObserved":{
		"type":"DateTime",
		"value":"2018-07-30T18:00:00-05:00"
	},
	"address": {
   	 	"type": "StructuredValue",
    	"value": {
      		"addressCountry": "MX",
					"addressLocality": "Ciudad de México",
					"streetAddress": "Centro"			
        }
   },
	"location": {
   		 "value": {
      	 	"type": "Point",
      	 	"coordinates": [-99.133167,19.434072]
   		},
    	"type": "geo:json"
  },
	"source":{
		"type":"Text",
		"value":"http://www.aire.cdmx.gob.mx/"
	},
	"temperature":{
		"type":"Number",
		"value":"0"
	},
	"relativeHumidity":{
		"type":"Number",
		"value":"0"
	},
	"CO":{
		"type":"Number",
		"value":"0"
	},
	"O3":{
		"type":"Number",
		"value":"0"
	},
	"NO2":{
		"type":"Number",
		"value":"0"
	},
	"SO2":{
		"type":"Number",
		"value":"0"
	},
	"PM10":{
		"type":"Number",
		"value":"0"
	}
}
```

![Creación de entidad en insomnia](./images//entitypost.png)

Si la llamada es correcta, se recibirá una respuesta *201 Created*

### Consulta de entidades

Utilizando el método **GET**, verificamos que nuestra entidad se encuentra definida en OCB.
Podemos filtrar la consulta por Id de la entidad, tipo de entidad, etc.

```
GET http://localhost:1026/v2/entities/AirQualityUnit01
```

### Actualización de datos y eliminación de una entidad

Realizamos operaciones básicas para actualizar **PUT**, **PATCH** y eliminar **DELETE**. Verificando los cambios en nuestra entidad.

P.Ej. Actualizando el valor de *CO*

```
PATCH http://localhost:1026/v2/entities/AirQualityUnit01/attrs
Header: Content-Type: application/json

{
  "CO": {
    "value": 25,
    "type": "Number"
  }
}
```

Y la eliminación de una entidad

```
DELETE http://localhost:1026/v2/entities/AirQualityUnit01
```

De ésta manera, nuestro servicio OCB se encuentra procesando la información de contexto que recibe manualmente a través de llamadas REST. Utilizando dispositivos de IoT, como [Cloudino](http://cloudino.io/) o [SmartSpot](http://smartcities.hopu.eu/smart-spot.html), la información se procesará de forma automática.

## Quantumleap

[QuantumLeap](https://quantumleap.readthedocs.io/en/latest/) es una API que tiene la finalidad de almacenar información del modelo NGSIv2 en una base de datos de series de tiempo; es decir, permite almacenar un historial de las actualizaciones de una entidad indexadas por el tiempo en que se realizaron.
Su funcionamiento se basa en el mecanismo de notificaciones que utiliza Orion Context Broker. Adicionalmente cuenta con métodos básicos para acceso a datos (consulta, eliminación).

Verificamos que el servicio se encuentre funcionando con la última versión 0.3.0 (Julio 2018):

```
http://localhost:8668/v2/version
```

### Suscripción de entidades de calidad del aire

El mecanismo de suscripciones de Orion Context Broker funciona de tal manera que cada vez que una entidad se actualiza, éste envía los datos definidos en la suscripción a una aplicación definida por el usuario (API, aplicación, servicio web, ...). Es decir, OCB determina cuando ha ocurrido un cambio en la información y notifica dicho cambio a una tercera aplicación de forma asíncrona.

En la siguiente suscripción se indica que cada que haya un cambio en los atributos *temperature* y *relativeHumidity* de todas las entidades (definido con el patrón *.** ) de tipo *AirQualityObserved*, se enviará una notificación al servicio *http://quantumleap:8668/v2/notify* con los atributos de la entidad *id*, *temperature*, *relativeHumidity*, *dateObserved*, *location*.

```
 {
  "description": "Suscripción AirQuality",
  "subject": {
    "entities": [
      {
        "idPattern": ".*",
        "type": "AirQualityObserved"
      }
    ],
    "condition": {
      "attrs": [
        "temperature",
        "relativeHumidity",
      ]
    }
  },
  "notification": {
    "attrs": [
      "id",
      "temperature",
      "relativeHumidity",
      "dateObserved",
      "location"
    ],
    "http": {
      "url": "http://quantumleap:8668/v2/notify"
    }
  }
}
```

1. Creamos una suscripción en OCB para las entidades de tipo *AirQualityObserved*, donde las notificaciones se envíen al endpoint de QuantumLeap ***http://localhost:8668/v2/notify***.
La suscripción señala que se envíen todos los atributos de una entidad cuando exista algún cambio en los atributos señalados en *condition.attrs*. Ésta suscripción aplica para todas las entidades de tipo *AirQualityObserved*. La sección *"metadata": ["dateCreated", "dateModified"]* sirve para incluir en la notificación el *timestamp* en que se modificaron los atributos. Éste *timestamp* es utilizado por QuantumLeap como índice de tiempo en la base de datos. En caso de que no exista, QuantumLeap utiliza la fecha del sistema para realizar la indexación de los notificaciones.

```
POST localhost:1026/v2/subscriptions
Header: Content-Type: "application/json"

{
  "description": "Suscripcion QuantumLeap AirQuality",
  "subject": {
    "entities": [
      {
        "idPattern": ".*",
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
      "location"
    ],
    "http": {
      "url": "http://quantumleap:8668/v2/notify"
    },
    "metadata": [
      "dateCreated",
      "dateModified"
    ]
  }
}
```

***NOTA: Es importante notar que la URL de QuantumLeap debe ser alcanzable por Orion, de tal forma que utilizando localhost:8668 para hacer referencia a QuantumLeap resultaría erróneo, si no que haría referencia a si mismo. En el escenario de los servicios en docker, podemos utilizar el nombre del contenedor http://quantumleap:8668/v2/ debido a que se encuentran en la misma red docker***

![Creación de suscripción a QuantumLeap en insomnia](./images//quantumsubscription.png)

2. Con la suscripción creada, cada que realicemos una actualización a una entidad de tipo *AirQualityObserved*, ésta se enviará a QuantumLeap, Podemos verificar que ahora las actualizaciones que se realizan en OCB se almacenan de forma histórica en una base de datos a través de QuantumLeap.

- Actualizamos una entidad.

```
PATCH http://localhost:1026/v2/entities/AirQualityUnit01/attrs
Header: Content-Type: application/json

{
  "CO": {
    "value": 25,
    "type": "Number"
  }
}
```

- Verificamos el registro histórico de la entidad en QuantumLeap
```
http://localhost:8668/v2/entities/AirQualityUnit01?type=AirQualityObserved
```

![Respuesta de QuantumLeap con dos notificaciones recibidas](./images//quantumget.png)

### API QuantumLeap y everificación de errores

QuantumLeap cuenta con métodos adicionales para acceso a los datos que puede ser consultados en la [API](https://app.swaggerhub.com/apis/smartsdk/ngsi-tsdb/0.1)
En caso de tener problemas podemos verificar los errores en las [FAQ](https://quantumleap.readthedocs.io/en/latest/user/troubleshooting/)

## CrateDB

CrateDB es una base de datos SQL distribuida y construida sobre una plataforma NoSQL que controla el almacenamiento, indexación e interconectividad, facilitando una mayor rapidez, flexibilidad y escalabilidad a comparación de bases de datos relacionales mientras que permite al usuario acceder a los datos a través de sentencias SQL comunes.
Debido a éstas características, QuantumLeap utiliza CrateDB como base de datos para almacenar información de series de tiempo, sirviendo como puente entre el mecanismo de notificaciones de OCB y el registro histórico y almacenamiento en la base de datos CrateDB.

### Acceso a datos en CrateDB

Podemos acceder a CrateDB a través de una línea de comandos o mediante su GUI en un navegador a través del puerto **http://localhost:4200**

1. Para verificar los datos en la GUI a través de **http://localhost:4200** ubicamos la sección *Tablas* donde observamos las tablas creadas con el prefijo **et**. P. Ej. *etairqualityobserved*.

![Sección Tablas en el menu izquierdo](./images//cratetable.png)

2. En la sección *Consola* podemos consultar los datos utilizando sentencias SQL.
P. ej. para consultar los registros de AirQualityObserved

```
select * from etairqualityobsered
```

![Resultado de consulta](./images//crateconsole.png)

### Consola de administración **crash**
Alternativamente, podemos utilizar una consola para acceder a la información de CrateDB mediante el acceso al contenedor de docker.

- Se accede a la consola del contenedor:
```
docker exec -ti crate /bin/sh
```

- Dentro del contenedor, a su vez, accedemos a la consola de administración de CrateDB, llamada *crash*.
```
crash
```
*crash* sirve de entorno para manipular la base de datos utilizando sentencias SQL.

P.Ej. Crear tablas e insertar datos:

- Creación de tablas
```
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

INSERT INTO etstation (entity_id, name, acron) values('CDMX-AmbientObserved-484150020109','Acolman','ACO');
INSERT INTO etpollutant (idpollutant, name, namefront) values(3,'co','Carbon Monoxide (CO)');
```

## Grafana

Grafana es una herramienta de visualización de datos open source comúnmente usada en monitoreo de infraestructura como sensores, automatización, condiciones ambientales y control de procesos.
Ofrece una interfaz web para la creación de componentes gráficos basadas en series tiempo utilizando distintas bases de datos con sus respectivos lenguajes y sentencias.

### Acceso a panel de administración

Accedemos al servicio desde un navegador a Grafana en **http://localhost:3000/**


![Página de inicio (última versión julio 2018)](./images//grafanainicio.png)

Las credenciales por default son:

**user :admin**
**password: admin**

Posteriormente, solicitará cambiar la contraseña de administrador e ingresaremos al panel de administración.

### Creación de Datasource

El primer paso para crear un panel de visualización es definir como se realizará la conexión a la base de datos CrateDB, de donde obtendremos la ínformación de calidad del aire.

1. En la sección *Configuration/DataSources*, agregamos un nuevo Datasource.

![Crear Datasource](./images//grafanadatasource.png)

2. Se configura el tipo y ubicación de la base de datos, el esquema y la tabla que utilizará así como la columna que indexa el valor de tiempo en los registros.

![Configuración de Datasource](./images//grafanadatasourceval.png)

Verificando que la conexión se realice correctamente con *Save & Test*.

![Conexión correcta](./images//grafanadatasourceok.png)

### Creación de Dashboard

Grafana permite crear diferentes tipos de gráficas para visualizar información temporal, mapas de calor, alertas, tablas, funciones de agregación, entre otros.
El panel realiza una conexión a la base de datos CrateDB con la definición del datasource y despliega la información a través de consultas SQL.

1. Crear un nuevo dashboard y seleccionar el tipo de panel Graph.

![Crear nuevo Dashboard](./images//dash01.png)

![Panel Graph](./images//dash02.png)

2. Editar el panel y seleccionar la sección *Metrics*, donde realizaremos las consultas a la base de datos.

![Editar Panel](./images//dash03.png)

- Identificamos el datasource de CrateDB

![Identificación de datasource AirQuality](./images//dash04.png)

- Realizamos una consulta a la base de datos cambiando el modo de edición en la sección *Toggle Edit Mode*.

```
SELECT time_index as time, CO FROM "doc"."etairqualityobserved" WHERE entity_id='AirQualityUnit01' GROUP BY time, CO ORDER BY time ASC
```

![Graficación de CO y tiempo de AirQualityUnit01](./images//dash05.png)

3. Existen distintas opciones para personalizar la visualización acorde a nuestras necesidades como el uso de variables, definir umbrales y alertas, etc.

![Calidad del aire CDMX](./images//dash06.png)

Información más detallada la podemos encontrar en [Grafana](http://docs.grafana.org/).

### Uso de Grafana a través de API

Grafana cuenta con una API que permite administrar varias opciones, incluyendo la configuración de datasources y dashboards.

1. Obtener *key* para uso de API Grafana realizando un POST request

```
POST http://admin:admin@localhost:3000/api/auth/keys
Content-Type: application/json
{"name":"apikeycurl", "role": "Admin"}
```

La respuesta es la *key* que se debe de utilizar en llamadas posteriores como un header *Authorization*
It returns a key that you'll need to send as an "Authorization" header in the following steps.
```
{
	"name": "apikeycurl",
	"key": "eyJrIjoiUDNGQlM5YldXbUdVU2JreDJiVkZDYW81aWZCTlZFSlkiLCJuIjoiYXBpa2V5Y3VybCIsImlkIjoxfQ=="
}
```

2. Se crea un datasource de tipo CrateDB indicando la URL de su ubicación dentro del payload de JSON y la *key* obtenida previamente como header.

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

3. Incluso es posible importar un dashboard generado previamente como un archivo JSON.

```
curl -X POST  -H "Accept: application/json" -H "Content-Type: application/json" -H "Authorization: Bearer eyJrIjoiUDNGQlM5YldXbUdVU2JreDJiVkZDYW81aWZCTlZFSlkiLCJuIjoiYXBpa2V5Y3VybCIsImlkIjoxfQ==" -d @rawdash.json http://0.0.0.0:3000/api/dashboards/db
```

## Instalar aplicación con docker swarm (utilizando docker-machine).

**Pre-requisitos (Linux):**

- Instalar virtualbox
```
sudo apt-get install virtualbox
```
- Instalar [docker-machine](https://docs.docker.com/machine/install-machine/#install-machine-directly)

1. Crear dos docker-machine

```
docker-machine create --driver virtualbox vm1
docker-machine create --driver virtualbox vm2
```

2. Definir nodo *main* para swarm.

```
docker-machine ls  #To see the virtual ip adress of the main node
docker-machine ssh vm1 "docker swarm init --advertise-addr 192.168.99.100"
```

El comando regresa un token para agregar nodos *workers* al swarm

3. Agrega nodo *worker* (segunda maquina) al swarm.
```
docker-machine ssh vm2 "docker swarm join --token SWMTKN-1-4kwbrascwqpye99rcs252okjgbj67eg5hpachf9dppkh6x5ff7-3dxfvrmtzd71yyezx2e0l1v7z 192.168.99.100:2377"

docker-machine ssh vm1 "docker node ls"
```
4. Cambiar al ambiente del nodo *main*

```
docker-machine env myvm1
eval $(docker-machine env myvm1)
```

5. Desplegar los servicios utilizando el archivo *docker-compose*.
```
docker stack deploy -c docker-compose.yml greenroute
docker stack ps greenroute
```

6. Eliminar el swarm.
```
docker stack rm greenroute
```

7. Cerrar el ambiente del nodo *main*.
```
eval $(docker-machine env -u)
```

## Referencias

- [Modelos de datos FIWARE](http://fiware-datamodels.readthedocs.io/en/latest/index.html)
- [Orion Context Broker](https://fiware-orion.readthedocs.io/en/1.11.0/index.html)
- [Docker](https://docs.docker.com/)
- [Docker-compose](https://docs.docker.com/compose/)
- [QuantumLeap](https://quantumleap.readthedocs.io/en/latest/)
- [Grafana](http://docs.grafana.org/)
- [CrateDB](https://crate.io/docs/crate/reference/en/latest/)
- [Cloudino](http://cloudino.io/)
- [SmartSpot](http://smartcities.hopu.eu/smart-spot.html)
