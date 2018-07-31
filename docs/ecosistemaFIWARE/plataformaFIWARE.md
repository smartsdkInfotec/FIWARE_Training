## Plataforma FIWARE
La [plataforma FIWARE](https://www.fiware.org/developers/catalogue/) es un marco de componentes de plataforma de código abierto que se puede ensamblar junto con otros componentes de plataforma de terceros para acelerar el desarrollo de soluciones inteligentes. Estos componentes de plataforma proporcionan APIs (interfaces de programación de aplicaciones) cuyas especificaciones son públicas y libres de regalías. Además, tienen disponible públicamente una implementación de referencia de código abierto para que los proveedores de FIWARE puedan salir rápidamente al mercado, con propuestas de bajo costo. 

El principal y único componente obligatorio de cualquier plataforma o solución "Powered by FIWARE" es el Context Broker (su implementación de referencia es llamada Orion Context Broker), que aporta una función fundamental en cualquier solución inteligente: la necesidad de administrar la información de contexto, lo que permite realizar actualizaciones y acceder al contexto. El Context Broker está rodeado por una suite de componentes de plataforma adicionales, que permiten suministrar datos de contexto de diversas fuentes y brindan soporte para el procesamiento, análisis y visualización de datos, así como para control de acceso a datos, publicación o monetización. Además, ofrece una serie de herramientas que facilitan la implementación y configuración de FIWARE o componentes de terceros y su integración con el Context Broker.

![model](./images//FGE-02.jpg) 


## Catálogo de componentes de la plataforma FIWARE
Las siguientes secciones describen la lista actual de componentes de la plataforma FIWARE (Generic Enablers) estructurados en capítulos. 

## Gestión de Contexto
EL [Orion Context Broker](./ocb.md) es el componente central y obligatorio de cualquier plataforma o solución "Powered by FIWARE". Permite administrar la información de contexto de una manera altamente descentralizada y a gran escala. Proporciona la [API FIWARE NGSIv2](./ocb.md), la cual es una API Restful simple y muy potente que permite realizar actualizaciones, consultas o suscripciones a cambios en la información de contexto. El [Orion Context Broker](./ocb.md) mantiene la información del contexto actual. Sin embargo, la información de contexto evoluciona con el tiempo, creando un historial de contexto. Para apoyar el almacenamiento del historial de contexto existen los siguientes componentes: 

El [STH Comet](https://catalogue-server.fiware.org/enablers/sth-comet) proporciona los medios para almacenar un historial de datos de contexto a corto plazo (generalmente meses) en MongoDB.

El [Cygnus](https://catalogue-server.fiware.org/enablers/cygnus) brinda los medios para administrar el historial de contexto que se crea como una secuencia de datos que se puede inyectar en múltiples receptores de datos, incluidas algunas bases de datos populares como PostgreSQL, MySQL, MongoDB o AWS DynamoDB, así como plataformas BigData como Hadoop, Storm, Spark or Flink.

El [QuantumLeap](https://quantumleap.readthedocs.io/en/latest/) permite el almacenamiento de datos de la API FIWARE NGSIv2 a una base de datos de series de tiempo, conocida como ngsi-tsdb. Cuenta con un traductor de CrateDB con lo que se brindan las siguientes ventajas:
- Escalabilidad fácil con clúster de base de datos en contenedores
- Soporte de Geo-consultas
- Lenguaje de consulta tipo SQL
- Integración soportada con herramientas de visualización como Grafana

## IoT, Robots y sistemas de terceros
Existe una serie de componentes que facilitan la interfaz con el Internet de las cosas, robots y sistemas de terceros con el fin de recopilar información de contexto valiosa o desencadenar actuaciones en respuesta a actualizaciones de contexto:
- [IDAS](https://catalogue-server.fiware.org/enablers/backend-device-management-idas) ofrece una amplia gama de Agentes de IoT que facilita la interfaz con dispositivos que utilizan los protocolos de IoT más utilizados (LWM2M sobre CoaP, JSON o UltraLight sobre HTTP / MQTT u OPC-UA), así como un esqueleto para desarrollar su propio Agente IoT. 

## Procesamiento, análisis y visualización
Existe una serie de componentes que facilitan el proceso, el análisis o la visualización de la información de contexto con el fin de implementar el "comportamiento inteligente" esperado en cualquier aplicación:

## Gestión de acceso, publicación y monetización


Consulta el [catálogo de FIWARE](https://www.fiware.org/developers/catalogue/) 
