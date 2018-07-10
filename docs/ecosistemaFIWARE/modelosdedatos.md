
# Modelos de datos de FIWARE
Los modelos de datos se han armonizado para permitir la portabilidad de datos para diferentes aplicaciones, incluidas, entre otras, Smart Cities. Están destinados a ser utilizados junto con FIWARE NGSI versión 2.

## Modelo de alertas 
El objetivo del modelo es apoyar la generación de notificaciones para un usuario o desencadenar otras acciones, basadas en dichas alertas.

Esta entidad modela una alerta y podría usarse para enviar alertas relacionadas con atascos, accidentes, condiciones climáticas, alto nivel de contaminantes, etc.

Una alerta es generada por una situación específica. Las principales características de una alerta es que no es predecible y no es un dato recurrente. Eso significa que una alerta podría ser un accidente o un alto nivel en la medición de un contaminante, además, podría ser la caída de un paciente o un automóvil que conduce en dirección opuesta.

Algunos ejemplos de datos contextuales son: tipo de alerta (tráfico, clima, seguridad, contaminación, etc.), ubicación, etc.

Consulta el esquema JSON del [modelo de alertas](http://fiware-datamodels.readthedocs.io/en/latest/Alert/doc/spec/index.html)

## Modelos de datos de parques y jardines
Estos modelos de datos están diseñados para modelar parques, jardines y espacios verdes relacionados en una ciudad. Los principales tipos de entidad identificados son:
- **[Park](https://schema.org/Park)**: un parque es un área de espacio abierto proporcionado para uso recreativo, usualmente diseñado y en estado seminatural con áreas cubiertas de hierba, árboles y arbustos. Los parques son a menudo, pero no siempre, municipales. Por lo general, está abierto al público, pero puede estas vallado y puede estar cerrado temporalmente, por ejemplo durante la noche. Ver [OpenStreetMap](https://wiki.openstreetmap.org/wiki/Tag:leisure%3Dpark). 
Schema.org ya proporciona un tipo de entidad para este propósito que se puede reutilizar.

 - **[Garden](http://fiware-datamodels.readthedocs.io/en/latest/ParksAndGardens/Garden/doc/spec/index.html)**: un jardín es un espacio planificado distinguible, generalmente al aire libre, destinado a la exhibición, el cultivo y el disfrute de las plantas y otras formas de la naturaleza. Un jardín puede incorporar tanto materiales naturales como hechos por el hombre. Los jardines occidentales se basan casi universalmente en las plantas. Un jardín también puede ser parte de un parque y abierto al público. Ver [OpenStreetMap](https://wiki.openstreetmap.org/wiki/Tag:leisure%3Dgarden). Un jardín se puede dividir en varias partes más pequeñas, llamadas camas de flores.
 
- **[FlowerBed](http://fiware-datamodels.readthedocs.io/en/latest/ParksAndGardens/FlowerBed/doc/spec/index.html)**: una cama de flores es una parcela de jardín en la que se cultivan flores (u otras plantas). Por lo general, encontrará camas de flores en parques, jardines, zonas peatonales o en grandes cruces de autopistas. Ver [OpenStreetMap](https://wiki.openstreetmap.org/wiki/Proposed_features/flowerbed).

- **[GreenspaceRecord](http://fiware-datamodels.readthedocs.io/en/latest/ParksAndGardens/GreenspaceRecord/doc/spec/index.html)**: esta entidad contiene una descripción armonizada de las condiciones registradas en un área o punto particular dentro de un jardín o espacio verde relacionado. Tal registro puede asociarse a un jardín, a una cama de flores específica, etc.

## Modelos de datos del ambiente
Estos modelos de datos describen las principales entidades involucradas en aplicaciones inteligentes que se ocupan de cuestiones ambientales. Las principales entidades identificadas son:
- **[AeroAllergenObserved](http://fiware-datamodels.readthedocs.io/en/latest/Environment/AeroAllergenObserved/doc/spec/index.html)**: describe aerolerógenos observados en un lugar determinado y el riesgo de alergenos en general. 

- **[AirQualityObserved](http://fiware-datamodels.readthedocs.io/en/latest/Environment/AirQualityObserved/doc/spec/index.html)**: representa una observación de las condiciones de calidad del aire en un lugar y tiempo determinados.

- **[WaterQualityObserved](http://fiware-datamodels.readthedocs.io/en/latest/Environment/WaterQualityObserved/doc/spec/index.html)**: permite capturar todos los parámetros involucrados en los escenarios de calidad del agua, lo que permite tratar con diferentes escenarios: ríos y lagos, embalses como presas, cisternas, ubicaciones en el mar, piscinas o fuentes. 

- **[NoiseLevelObserved](http://fiware-datamodels.readthedocs.io/en/latest/Environment/NoiseLevelObserved/doc/spec/index.html)**: representa una observación de aquellos parámetros que estiman los niveles de presión de ruido en un lugar y tiempo determinados.
 
## Modelo de datos de puntos de interés
La documentación formal no está disponible todavía. Mientras tanto, verifique algunos de los [ejemplos de uso](http://fiware-datamodels.readthedocs.io/en/latest/PointOfInterest/WeatherStation/doc/spec/index.html).

## Modelos de datos de seguimiento de problemas cívicos
Estos modelos de datos permiten realizar un seguimiento de problemas cívicos. Han sido diseñados con el objetivo de permitir la interoperabilidad trivial entre FIWARE NGSI versión 2 y [Open311](http://www.open311.org/) (un modelo de colaboración y un estándar abierto para el seguimiento de problemas cívicos). Como resultado, los nombres de propiedad no se han normalizado a la sintaxis <code>camelCase</code>, permanecen tal como está especificado actualmente por [Open311](http://www.open311.org/). Ese es el razonamiento detrás de la denominación de tipos de entidad con Open311 como prefijo. Sin embargo, se agregan algunas propiedades para que las implementaciones de FIWARE NGSI versión 2 puedan almacenar y servir datos de Open 311 correctamente.
FIWARE y OASC, en el mediano plazo, podrían proponer a la Comunidad Open311 un modelo de datos armonizado alineado con el resto de los modelos de datos de FIWARE y schema.org. De hecho, al usar estos modelos de datos y una implementación de FIWARE NGSI versión 2, es trivial implementar las API propuestas por Open311. Otra opción sería el uso de JSON-LD para definir equivalencias y mapeos entre un modelo de datos de FIWARE / Schema.org y Open 311.
El modelo de datos de seguimiento de problemas cívicos de FIWARE NGSI define los siguientes tipos de entidades:
- **[Open311: ServiceType](http://fiware-datamodels.readthedocs.io/en/latest/IssueTracking/Open311_ServiceType/doc/spec/index.html)**: un tipo de servicio que un ciudadano puede solicitar. Abarca los datos de la lista de servicios GET Open 311 y la definición de servicio GET.
- **[Open311: ServiceRequest](http://fiware-datamodels.readthedocs.io/en/latest/IssueTracking/Open311_ServiceRequest/doc/spec/index.html)**: una solicitud de servicio específica (de un tipo de servicio) hecha por un ciudadano.

## Modelos de datos de alumbrado público
Las farolas, comúnmente conocidas como 'lámparas de poste', están diseñadas para hacer que las calles sean más seguras para los peatones y los conductores. Estos modelos de datos están destinados a modelar las farolas y todos sus equipos de control para lograr una iluminación urbana efectiva y eficiente desde el punto de vista energético. Abarca los siguientes tipos de entidades:
- **[Streetlight](http://fiware-datamodels.readthedocs.io/en/latest/StreetLighting/Streetlight/doc/spec/index.html)**: representa una instancia particular de una farola. Una farola está compuesta por una linterna y una lámpara. Dichos elementos están montados en una columna (poste), pared u otra estructura.
- **[StreetlightGroup](http://fiware-datamodels.readthedocs.io/en/latest/StreetLighting/Streetlight/doc/spec/index.html)**: representa un grupo de alumbrado público que forma parte del mismo circuito y está controlado por un sistema automático.
- **[StreetlightModel](http://fiware-datamodels.readthedocs.io/en/latest/StreetLighting/StreetlightModel/doc/spec/index.html)**: representa un modelo de alumbrado público compuesto por un modelo de estructura de soporte específico, un modelo de linterna y un modelo de lámpara. Una instancia de farola se basará en un determinado modelo de farola.
- **[StreetlightControlCabinet](http://fiware-datamodels.readthedocs.io/en/latest/StreetLighting/StreetlightControlCabinet/doc/spec/index.html)**: representa un equipo automatizado, generalmente en la calle, que generalmente se usa para controlar un grupo de farolas, es decir, uno o más circuitos.

## Modelos de datos de dispositivos
Estos modelos de datos permiten representar dispositivos de diferente naturaleza (IoT, móvil, usable, etc.). Está compuesto por los siguientes tipos de entidades:
- **[Dispositivo](http://fiware-datamodels.readthedocs.io/en/latest/Device/Device/doc/spec/index.html)**: un dispositivo es un aparato electrónico diseñado para realizar una tarea particular.
- **[DeviceModel](http://fiware-datamodels.readthedocs.io/en/latest/Device/DeviceModel/doc/spec/index.html)**: captura las propiedades estáticas comunes a varias instancias de un dispositivo.

## Modelos de datos armonizados de transporte
Estos modelos de datos describen las principales entidades involucradas con aplicaciones inteligentes que se ocupan de aspectos de transporte. Este conjunto de entidades está principalmente asociado con los segmentos verticales de Automóviles y Ciudad Inteligente y las aplicaciones de IoT relacionadas. Cuando es factible, se incluyen referencias a los tipos y atributos existentes de entidades de schema.org.

Estos modelos han sido diseñados para ser lo más genéricos posible, lo que permite tratar con diferentes escenarios:
- Control del flujo de tráfico
- Vehículos privados
- Vehículos públicos (autobuses, trenes, etc.)
- Vehículos municipales (camiones de recogida, unidades de limpieza, ...)
- Vehículos especiales (ambulancias, bomberos, ...)

The main entities identified are:
- **[TrafficFlowObserved](http://fiware-datamodels.readthedocs.io/en/latest/Transportation/TrafficFlowObserved/doc/spec/index.html)**: representa una observación sobre el flujo de tráfico.

- **[Road](http://fiware-datamodels.readthedocs.io/en/latest/Transportation/doc/introduction/index.html)**: contiene una descripción geográfica y contextual armonizada de un camino.

- **[RoadSegment](http://fiware-datamodels.readthedocs.io/en/latest/Transportation/RoadSegment/doc/spec/index.html)**: contiene una descripción geográfica y contextual armonizada de un segmento de carretera.

- **[Vehicle](http://fiware-datamodels.readthedocs.io/en/latest/Transportation/Vehicle/Vehicle/doc/spec/index.html)**: representa un vehículo con todas sus características individuales.

- **[VehicleModel](http://fiware-datamodels.readthedocs.io/en/latest/Transportation/Vehicle/VehicleModel/doc/spec/index.html)**: representa un modelo de vehículo y captura sus propiedades estáticas, como dimensiones, materiales o características.

## Modelo de datos de indicadores 
De acuerdo con Wikipedia, un Indicador Clave de Rendimiento (KPI) es un tipo de medición del rendimiento. Los KPI evalúan el éxito de una organización o de una actividad particular en la que participa.
El [modelo de KPI](http://fiware-datamodels.readthedocs.io/en/latest/KeyPerformanceIndicator/doc/spec/index.html) define un tipo de entidad NGSI que captura el valor y los detalles asociados de un indicador de clave de rendimiento. El modelo de datos es lo suficientemente flexible como para acomodar diferentes escenarios de uso: una entidad por cálculo de KPI o una entidad única por KPI cuyo valor evoluciona a lo largo del tiempo. Tenga en cuenta que en este último caso un módulo histórico, como el STH, debería encargarse de la evolución de KPI.

## Modelos de datos armonizados de gestión de residuos
Estos modelos de datos describen las principales entidades que típicamente están involucradas en los escenarios de gestión de residuos. De hecho, estos modelos han sido diseñados para ser lo más genéricos posible, lo que permite tratar con diferentes escenarios:

- Gestión de residuos municipales con contenedores en la calle / enterrados.
- Gestión de residuos industriales utilizando contenedores especializados.
- Contenedores utilizados ocasionalmente en la calle (contenedores de residuos de construcción, etc.)
- Las camadas se colocan en la calle o en lugares públicos donde los desechos son dejados por el público.

The main entities identified are:
- **[WasteContainerIsle](http://fiware-datamodels.readthedocs.io/en/latest/WasteManagement/WasteContainerIsle/doc/spec/index.html)**: isla que contiene uno o más contenedores. En un escenario municipal, están delimitados en las áreas de la calle.
- **[WasteContainerModel](http://fiware-datamodels.readthedocs.io/en/latest/WasteManagement/WasteContainerModel/doc/spec/index.html)**: representa un modelo de contenedor de desechos, capturando sus propiedades estáticas tales como dimensiones, materiales o características.
- **[WasteContainer](http://fiware-datamodels.readthedocs.io/en/latest/WasteManagement/WasteContainer/doc/spec/index.html)**: representa una instancia particular de contenedor de residuos colocado en una isla o lugar en particular. Todas las propiedades dinámicas de un contenedor, por ejemplo, el nivel de llenado (fillingLevel) están incluidas por esta entidad.
- **LitterModel**: Es un modelo de hojarasca, que incluye todas sus propiedades estáticas. (T.B.D.)
- **Basura**: Representa una instancia particular de basura. (T.B.D.)

## Modelos de datos armonizados de estacionamiento
Estos modelos de datos están destinados a modelar entidades relevantes para casos de uso de estacionamiento en escenarios de ciudades inteligentes. Cuando es factible, estos modelos reutilizan tipos, propiedades y enumeraciones de DATEX II versión 2.3. Se puede encontrar un diccionario de datos para los términos de DATEX II en http://datexbrowser.tamtamresearch.com/.

No obstante, estos modelos de datos están destinados a sistemas basados en NGSI y se han realizado muchas simplificaciones con respecto a DATEX II versión 2.3.
Los principales tipos de entidad identificados son:
- **[OffStreetParking](http://fiware-datamodels.readthedocs.io/en/latest/Parking/OffStreetParking/doc/spec/index.html)**: un sitio de estacionamiento con entradas y salidas explícitas.
- **[ParkingAccess](http://fiware-datamodels.readthedocs.io/en/latest/Parking/ParkingAccess/doc/spec/index.html)**. un punto de acceso a un sitio de estacionamiento.
- **[OnStreetParking](http://fiware-datamodels.readthedocs.io/en/latest/Parking/OnStreetParking/doc/spec/index.html)**: una zona de estacionamiento en la calle, de entrada libre (que puede ser medida) que contiene al menos uno o más lugares de estacionamiento adyacentes.
- **[ParkingGroup](http://fiware-datamodels.readthedocs.io/en/latest/Parking/ParkingGroup/doc/spec/index.html)**: un grupo de lugares de estacionamiento. El nivel de granularidad puede variar. Puede ser un piso en un estacionamiento, un área específica que pertenece a un estacionamiento grande, etc. o solo un grupo de lugares, diferenciados para un propósito específico (uso, restricciones, etc.).
- **[ParkingSpot](http://fiware-datamodels.readthedocs.io/en/latest/Parking/ParkingSpot/doc/spec/index.html)**: Un lugar de estacionamiento individual, generalmente monitoreado.

## Modelo de datos del clima 
El [modelo de datos del clima](http://fiware-datamodels.readthedocs.io/en/latest/Weather/WeatherObserved/doc/spec/index.html) mide una observación de las condiciones climáticas en un lugar y tiempo determinados. Este modelo de datos ha sido desarrollado en cooperación con operadores móviles y la GSMA.
