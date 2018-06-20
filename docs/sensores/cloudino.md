# Creación de una unidad de monitoreo de calidad del aire con Cloudino

## Introducción 

Esta guía proporciona la información necesaria para crear una unidad de monitoreo de calidad del aire utilizando Cloudino, un agente de Internet de las Cosas, que permite monitorear sensores de manera remota y enviar datos a la plataforma FIWARE. 
El conector WiFi de Cloudino (Cloudino WiFi Connector) puede ser utilizado como un dispositivo independiente para comunicar objetos de la vida real directamente a internet o como un microcontrolador adicional dedicado a la capa de red, trabajando en paralelo con soluciones de microcontroladores actuales como Arduino.


## Qué se necesita

Utilizando Arduino, el conector WiFi de Cloudino funciona como otro procesador en paralelo dedicado solamente a la capa de red, incluyendo los protocolos de Internet de las Cosas, dejando al Arduino dedicado a la conectividad con sensores y actuadores y permitiendo reprogramar al Arduino vía WiFi o a través de la Nube. 
De esta manera, podemos conectar cualquier sensor compatible con Arduino y procesar los datos a través de Cloudino y la plataforma FIWARE. 
Para el propósito de esta guía, utilizaremos los siguientes componentes:

* [Cloudino WiFi Connector (CWC)](http://cloudino.io).
* [Arduino UNO](https://store.arduino.cc/usa/arduino-uno-rev3).
* [PPD42NS](https://www.mouser.com/ds/2/744/Seeed_101020012-1217636.pdf). Sensor PM10.
* [DHT11](http://www.micropik.com/PDF/dht11.pdf). Sensor de temperatura / Humedad.
* [MQ-131](https://www.compel.ru/item-pdf/cf02de305c8369719f27e4149395c6da/pn/winsen~mq131-high-concentration-ozone-gas-sensor.pdf).
  Sensor de Ozono.
* [Grove Multichannel](http://cdn-reichelt.de/documents/datenblatt/A300/101020088_01.pdf).
  Sensor de gas (NO2, CO).

Estos componentes se pueden adquirir en alguna tienda de electrónica local o en línea. A continuación, presentamos algunas opciones: 

* <https://www.robotshop.com/>
* <https://www.seeedstudio.com/>
* <https://www.sainsmart.com/>
* <https://store.arduino.cc/usa/arduino-uno-rev3>
* <https://www.sparkfun.com/products/13678>

El conector WiFi de Cloudino puede ser adquirido directamente contactando a su desarrollador 
<javier.solis@infotec.mx>, <victor.hernandez@infotec.mx>
o puede construir su propio conector utilizando un dispositivo [ESP8266](https://www.sparkfun.com/products/13678)
y siguiendo las instrucciones de la [documentación de Cloudino](https://github.com/Cloudino/Cloudino-Doc/wiki/Make-your-first-Cloudino).

## Modelo de conexión de la unidad de monitoreo de calidad del aire 

Fig. 1 muestra el modelo de conexión para una unidad de monitoreo de calidad del aire donde la placa de Arduino Uno conecta cuatro sensores, los cuales miden: temperatura, humedad, monóxido de carbono (CO), óxido de nitrógeno (NO2) y partículas de polvo (PM10), además conecta el conector WiFi de Cloudino que permite enviar las mediciones de los sensores a la nube utilizando el portal
[cloudino.io](http://cloudino.io/).

  ![Air monitoring unit diagram](./images//cdn_01.png)
  *Fig. 1. Diagrama de la unidad de monitoreo de calidad del aire*

El esquema es propuesto con fines educativos y sus componentes son fáciles de conseguir. Sin embargo, es posible integrar algún componente electrónico adicional que se desee a la placa de Arduino para crear sistemas más complejos y de esta manera publicar sus datos en las plataformas de Cloudino y FIWARE. 

## Instalación y configuración

### Crear un ambiente de desarrollo en Cloudino.io

Para utilizar la plataforma de Cloudino, es necesario ingresar al portal
[cloudino.io](http://cloudino.io/) donde se encuentran las herramientas que nos permitirán conectar cualquier dispositivo al Internet de las Cosas. 

  ![Cloudino.io Portal](./images//cdn_02.png)
  *Fig. 2. Portal cloudino.io*

En primer lugar, se requiere crear una cuenta e iniciar sesión en la plataforma, donde encontrará una sección de configuración para gestionar los dispositivos Cloudino, conexiones al [FIWARE Orion Context Broker (OCB)](https://fiware-orion.readthedocs.io), así como una guía de inicio. 

  ![Login page](./images//cdn_03.png)
  *Fig. 3. Página de inicio de sesión*

  ![Cloudino.io User's Main page](./images//cdn_04.png)
  *Fig. 4. Página de inicio de cloudino.io después de iniciar sesión*

El conector WiFi de Cloudino debe configurarse desde el portal para conectar su unidad de monitoreo de calidad del aire a la plataforma de Cloudino. Para ello registraremos un dispositivo en el portal. 

  ![Cloudino Wifi Connector](./images//cdn_05.png)
  *Fig. 5. Conector WiFi de Cloudino*

Del lado izquierdo del panel, seleccione el menú "Devices" y haga clic en la opción "Add Device". 
Introduzca un nombre, descripción y tipo de hardware. En este caso, utilizaremos un Arduino Uno. Haga clic en "Submit" para dar de alta el dispositivo. Se puede registrar la cantidad de dispositivos que se desee. 

  ![CWC device registed](./images//cdn_06.png)
  *Fig. 6. Registro del conector WiFi de Cloudino en cloudino.io*

Después del registro del dispositivo, se mostrarán varias secciones que permitirán configurarlo, controlarlo y programarlo. Así como asociarlo a nuestro conector WiFi de Cloudino, como se ve en la Fig. 7.

  ![CWC General Information](./images//cdn_07.png)
  *Fig. 7. Información general del conector WiFi de Cloudino*

La vista "General" muestra el identificador del dispositivo (id), el token de autenticación (Auth Token), nombre (name), descripción (description), tipo (type) y el nivel de seguridad público o privado (public/private). Esta información será utilizada posteriormente para asociar nuestro conector WiFi de Cloudino al dispositivo creado en el portal. 

### Conectar un conector WiFi de Cloudino al portal de cloudino.io

Una vez que se ha registrado un dispositivo en el portal de cloudino.io, se requiere asociarlo con el conector WiFi de Cloudino adjuntando el token de autenticación generado en el paso anterior. De esta manera, nuestra unidad de calidad del aire se conectará a la plataforma Cloudino. 
Para llevar a cabo esta acción, conecte el conector WiFi de Cloudino a una fuente de alimentación (5 volts). Al conectarlo, se creará una red WiFi a la cual debemos conectarnos para acceder al panel de configuración del conector WiFi de Cloudino. **El SSID de la red WiFi es el número de serie del conector WiFi de Cloudino y se crea sin contraseña**. 
Por ejemplo, Fig. 8 muestra la red Cloudino_FAAE37 que es la que utilizaremos para configurar nuestro conector WiFi de Cloudino, el cual tiene el número de serie FAAE37. 

  ![WiFi network generated by any CWC](./images//cdn_08.png)
  *Fig. 8. Red WiFi creada por un conector WiFi de Cloudino*

Una vez que nos conectamos a la red WiFi de nuestro conector (en este caso Cloudino_FAAE37), nos aparecerá un panel de configuración. En caso de que esto no suceda, puede acceder al panel utilizando un navegador web con la dirección IP **192.168.4.1**. Fig. 9 muestra la página web de un conector WiFi de Cloudino donde se realiza su configuración.

  ![CWC Configuration Panel on 192.168.4.1](./images//cdn_09.png)
  *Fig. 9. Panel de configuración del conector WiFi de Cloudino en la IP 192.168.4.1*

La asociación se realiza accediendo a la opción "Cloudino Cloud Configuration" que se encuentra en el submenú "Server Configuration" / "Cloudino Server". Para ello se requiere copiar y pegar el token de autenticación (Auth Token) en el campo "Auth Token" y seleccionar la opción "True" en el campo "Active". Posteriormente haga clic en el botón "Save", como se muestra en la  Fig 10.

  ![Cloudino Cloud Configuration](./images//cdn_10.png)
  *Fig. 10. Configuración del conector WiFi de Cloudino*

Por último, se debe configurar el conector WiFi de Cloudino para tener acceso a internet a través de una red WiFi para que envíe datos a la plataforma Cloudino. Para ello, vaya al menú "WiFi Configuration", escanee las redes WiFi disponibles y conéctese a la que desee. Puede verificar si la conexión es correcta cuando el estatus (Status) cambie a **"CONNECTED"**, como se muestra en la  Fig 11. 

  ![WiFi configuration Web page](./images//cdn_11.png)
  *Fig. 11. Configuración de la red WiFi.*

La conexión entre su conector WiFi de Cloudino y [cloudino.io](http://cloudino.io/) se establece cuando al campo “Active” se le asigna el valor “True” como se especificó en pasos anteriores. 

### Verificar la conexión entre el conector WiFi de Cloudino y cloudino.io

Para comprobar si el conector WiFi de Cloudino está conectado con la plataforma de Cloudino, se requiere que inicie sesión nuevamente en el portal de cloudino.io y seleccionar el menú "Devices". 

Si la conexión se estableció adecuadamente, podrá observar la leyenda **"online"** en color verde a un lado su dispositivo creado previamente, como se aprecia en la Fig. 12.
***Tenga en cuenta que debió desconectar su computadora de la red WiFi de su conector WiFi de Cloudino y conectarse a una red WiFi de acceso a internet.***

En este punto, su unidad de monitoreo de calidad del aire está lista para ser programada y controlada por medio de las herramientas de [cloudino.io](http://cloudino.io/).

  ![Cloudino device online and ready to be used](./images//cdn_12.png)
  *Fig. 12. El dispositivo está en línea y listo para usarse.*

### Define your application's logic through the development tools from cloudino.io

Once a CWC is registered in [cloudino.io](http://cloudino.io/), we can proceed
to program it along with the associated hardware "Arduino UNO" and sensors showed
in Fig. 1. to define the behaviour of the IoT air quality monitoring application.

At first, we need to create a sketch –project- which has the same context and
meaning of the Arduino technology. You must select the **"Arduino"** section
from the dropdown menu indicated with **</>** icon and go to **"Sketchers"**,
where you can create or edit projects to be used on Arduino boards with the
Cloudino technology. Select the **"Add Sketcher"** option as seen on Fig. 13.

  ![Add Sketcher option in the Arduino menu](./images//cdn_13.png)
  *Fig. 13. Add Sketcher option in the Arduino menu*

Then, you must enter a name for your Sketch (a program used to control an
Arduino board), and press the "Submit" button, it will display a Sketcher
area as shown in Figs. 14, 15.

  ![Define the name of your sketcher](./images//cdn_14.png)
  *Fig. 14. Define the name of your sketcher*

  ![Sketcher area: compile, save and delete your code](./images//cdn_15.png)
  *Fig. 15. Sketcher area: compile, save and delete your code*

In Sketcher area, see Fig. 15, you can write the code to control Arduino,
CWC and sensors for remote monitoring of your air quality unit. The
code syntax is based on the Cloudino API (Application Programming Interface)
which is a variant from Arduino, making it easier to be adopted by former
and new arduino users. A broad description of the Cloudino API can be found
at <https://github.com/Cloudino/Cloudino-Doc>.

Below we can see the arduino code to be used in the development of our air
quality monitoring unit. It sends the raw values captured from the sensors
showed in the diagram from Fig. 1 to the Cloudino platform using a post method.

```c++
#include <Cloudino.h>
#include <dht11.h>
#define DHT11PIN 7

Cloudino cdino;
dht11 DHT11;

const int analogPinO3 = A0;
const int analogPinCO = A3;
const int analogPinNO2 = A4;

void setup()
{
  cdino.setInterval(10000,getSensors);
  cdino.begin();
}

void getSensors()
{
   int chk = DHT11.read(DHT11PIN);
   cdino.post("temperature",String((float)DHT11.temperature,2));
   cdino.post("relativeHumidity",String((float)DHT11.humidity,2));
   cdino.post("O3",String(analogRead(analogPinO3)));
   cdino.post("CO",String(analogRead(analogPinCO)));
   cdino.post("NO2",String(analogRead(analogPinNO2)));
   cdino.post("PM10",String(digitalRead(8)));
}

void loop()
{
   cdino.loop();
}
```

***The previous code captures the raw data from sensors except the DHT11
which makes use of its own library to calculate the correct temperature and
relative humidity. To properly represent the measurements from many sensors,
additional libraries might be needed***

Once the code for the sketch has been written, it must be saved and compiled to
verify that it is consistent with the Cloudino API, this process is performed
by clicking the "Compile" button as shown in Fig. 16. The blue box (Console)
shows the compilation status, here you can be aware of any compilation errors
and success. To solve any compilation problems you should consult the Cloudino
API on the [Cloudino website](https://github.com/Cloudino/Cloudino-Doc).

  ![A sketch compiled with success](./images//cdn_16.png)
  *Fig 16. A sketch compiled with success*

Now, the created sketch must be loaded to the actual device. This is done by
using the "Upload Sketch" section within the selected device that is going to
be configured.

  ![“Upload Sketch” section from the selected device](./images//cdn_17.png)
  *Fig. 17. “Upload Sketch” section from the selected device*

A list of sketches developed by the user and samples is shown. We need to select
the recently created sketch named "AirQualityMonitorUnit_sketch" and press the
"Upload Sketcher" button in order to flash the compiled code in the CWC memory
and the Arduino board to perform the developed functionality. The application
logic is stored after flashing concludes succesfully Figures 18 and 19 show the
process described above.

  ![Selecting a sketch for the device](./images//cdn_18.png)
  *Fig. 18. Selecting a sketch for the device*

  ![Upload process of the sketch inside the CWC](./images//cdn_19.png)
  *Fig. 19. Upload process of the sketch inside the CWC.*

At this point, the functionality of the system according to the Fig. 1 is working.
While the monitoring station is powered and connected to internet it will be
able to send measurements to the cloudino.io server; with that said, data can
be observed and used remotely.
The developed code periodically sends the values reported by the sensors each
second and displays them in the **"Messages"** section of the cloudino.io platform,
as shown in Fig. 20. The console is updated in real time indefinitely while
Cloudino is operating.

  ![Real-time measurements from your AirQuality monitoring unit](./images//cdn_20.png)
  *Fig. 20. Real-time measurements from your AirQuality monitoring unit*

## Your Cloudino as IoT Agent

The Cloudino itself operates as an IoT Agent able to work with FIWARE.
Its Cloudino Platform lets us connect to the FIWARE Orion Context Broker following
a simple series of steps.

To share the data of your developed air quality station in Cloudino with the
FIWARE platform, you must first enter the **"FIWARE OCB"** menu and select the
**"Add Entity"** option; this section helps you to define a link with the FIWARE
platform. As shown on Fig. 21, it is neccesary to define a name for the link,
a description and select a device (CWC) to be associated with the platform.
In this manner, the data collected by Cloudino can be shared with the FIWARE
platform.

  ![Creating a link with the FIWARE platform](./images//cdn_21.png)
  *Fig. 21. Creating a link with the FIWARE platform*

Once a link has been created, a configuration panel is displayed (Fig 22)
where you can activate/deactivate the link, define the entity (data set) which
will be shared in the FIWARE platform as well as the OCB server address that
will be used to send the data and, when appropriate, the authentication data
required to connect to the server. Fig. 22 shows the definition of the entity
"AirQualityMonitorUnit:Entity" that includes all the data obtained from the
previously developed air quality monitoring unit.

  ![Configuration panel of the FIWARE OCB Link and entity definition](./images//cdn_22.png)
  *Fig. 22. Configuration panel of the FIWARE OCB Link and entity definition*

The following JSON code describes the entity “AirQualityMonitorUnit:Entity”
created in the previus step (Fig. 22):

```json
{
  "id": "AirQualityMonitorUnit:Entity",
  "type": "AirQualityObserved",
  "address": {
    "type": "StructuredValue",
    "value": {
      "addressCountry": "MX",
      "addressLocality": "Ciudad de México",
      "streetAddress": "San Fernando"
      }
    },
    "dataSource": {
      "type": "text",
      "value": "Cloudino"
    },
    "dateObserved": {
      "type": "DateTime",
      "value": "2018-02-01T17:00:00-05:00"
    },
    "location": {
      "value": {
        "type": "Point",
        "coordinates": [-99.163309, 19.291001]
      },
      "type": "geo:json"
    },
    "temperature": {
      "type": "text",
      "value": "12.2"
    },
    "relativeHumidity": {
      "type": "text",
      "value": "0.54"
    },
    "O3": {
      "type": "number",
      "value": "1.6"
    },
    "CO": {
      "type": "number",
      "value": "1.7"
    },
    "NO2": {
      "type": "number",
      "value": "1.9"
    },
    "PM10": {
      "type": "number",
      "value": "3.5"
    }
  }
```

***It is important to notice that the names of the variables used in cloudino must
be equal to the names defined in the attributes of the FIWARE entity model.***

After configuring the link, you must switch the "Active" option to **"ON"** and
define the entity (previous JSON code) to be shared with FIWARE, then click the
"Submit" button. It will automatically start to submit the data from the air
quality monitoring unit to the FIWARE OCB.

To verify the operation, we can access the associated OCB server defined in the
previous figure, i.e. <http://207.249.127.132:1026/v2> without any authentication.
In this way, Fig. 23. shows how the data defined in the entity and associated
with the Cloudino is displayed and updated in real time on the FIWARE OCB server.

  ![Data of air quality monitoring unit in the FIWARe OCB](./images//cdn_23.png)
  *Fig. 23. Data of air quality monitoring unit in the FIWARe OCB*

As can be seen on Fig. 23, the data of the air quality monitoring unit can
be accessed with the following REST call:
<http://207.249.127.132:1026/v2/entities/AirQualityMonitorUnit:Entity>,

where we can see the real time information of the air quality monitoring
sensors controlled through the Cloudino platform. The information can now be
used by any kind of IoT application.

Further details to developed your IoT applications with Cloudino and integrate
with the FIWARE platform can be found here: <https://github.com/Cloudino/Cloudino-Doc>

