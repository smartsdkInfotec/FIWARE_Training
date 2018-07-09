
# Modelos de datos de FIWARE
Los modelos de datos se han armonizado para permitir la portabilidad de datos para diferentes aplicaciones, incluidas, entre otras, Smart Cities. Están destinados a ser utilizados junto con FIWARE NGSI versión 2.

## Modelo de alertas 
El objetivo del modelo es apoyar la generación de notificaciones para un usuario o desencadenar otras acciones, basadas en dichas alertas.

Esta entidad modela una alerta y podría usarse para enviar alertas relacionadas con atascos, accidentes, condiciones climáticas, alto nivel de contaminantes, etc.

Una alerta es generada por una situación específica. Las principales características de una alerta es que no es predecible y no es un dato recurrente. Eso significa que una alerta podría ser un accidente o un alto nivel en la medición de un contaminante, además, podría ser la caída de un paciente o un automóvil que conduce en dirección opuesta.

Algunos ejemplos de datos contextuales son: tipo de alerta (tráfico, clima, seguridad, contaminación, etc.), ubicación, etc.

Consulta el esquema JSON del [modelo de alertas](http://fiware-datamodels.readthedocs.io/en/latest/Alert/doc/spec/index.html)

## Modelo de datos de parques y jardines
Estos modelos de datos están diseñados para modelar parques, jardines y espacios verdes relacionados en una ciudad. Los principales tipos de entidad identificados son:
- **[Parque](https://schema.org/Park)**: un parque es un área de espacio abierto proporcionado para uso recreativo, usualmente diseñado y en estado seminatural con áreas cubiertas de hierba, árboles y arbustos. Los parques son a menudo, pero no siempre, municipales. Por lo general, está abierto al público, pero puede estas vallado y puede estar cerrado temporalmente, por ejemplo durante la noche. Ver [OpenStreetMap](https://wiki.openstreetmap.org/wiki/Tag:leisure%3Dpark). 
Schema.org ya proporciona un tipo de entidad para este propósito que se puede reutilizar.

 - **[Jardín](http://fiware-datamodels.readthedocs.io/en/latest/ParksAndGardens/Garden/doc/spec/index.html)**: un jardín es un espacio planificado distinguible, generalmente al aire libre, destinado a la exhibición, el cultivo y el disfrute de las plantas y otras formas de la naturaleza. Un jardín puede incorporar tanto materiales naturales como hechos por el hombre. Los jardines occidentales se basan casi universalmente en las plantas. Un jardín también puede ser parte de un parque y abierto al público. Ver [OpenStreetMap](https://wiki.openstreetmap.org/wiki/Tag:leisure%3Dgarden). Un jardín se puede dividir en varias partes más pequeñas, llamadas camas de flores.
 
- **[Cama de flores](http://fiware-datamodels.readthedocs.io/en/latest/ParksAndGardens/FlowerBed/doc/spec/index.html)**: una cama de flores es una parcela de jardín en la que se cultivan flores (u otras plantas). Por lo general, encontrará camas de flores en parques, jardines, zonas peatonales o en grandes cruces de autopistas. Ver [OpenStreetMap](https://wiki.openstreetmap.org/wiki/Proposed_features/flowerbed).

- **[GreenspaceRecord](http://fiware-datamodels.readthedocs.io/en/latest/ParksAndGardens/GreenspaceRecord/doc/spec/index.html)**: esta entidad contiene una descripción armonizada de las condiciones registradas en un área o punto particular dentro de un jardín o espacio verde relacionado. Tal registro puede asociarse a un jardín, a una cama de flores específica, etc.

## Modelo de datos del ambiente
 
![catálogo](./images//catalogo.png)
