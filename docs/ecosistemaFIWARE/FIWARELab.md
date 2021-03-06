# La nube de FIWARE (FIWARE Lab)
La nube de FIWARE (FIWARE Lab) es un entorno de pruebas no comercial donde toma lugar la innovación y experimentación basada en las tecnologías FIWARE. Los usuarios pueden probar la tecnología y sus aplicaciones haciendo uso de los recursos proporcionados por FIWARE Lab: la infraestructura de cómputo como servicio (máquinas virtuales, espacio de almacenamiento, servicios de red), los componentes de software de la [Plataforma FIWARE](./plataformaFIWARE.md) y los datos abiertos publicados por ciudades y otras organizaciones. 
FIWARE Lab se conforma de un conjunto de nodos federados distribuidos alrededor del mundo. Puedes consultar la [infografía en tiempo real de las capacidades de los nodos de FIWARE Lab](http://infographic.lab.fiware.org/).

  ![FIWARELab](./images//FL-00.jpg)
  
Para tener acceso a FIWARE Lab y contar con recursos para realizar el despliegue de una infraestructura virtual para experimentar con las tecnologías de FIWARE, es necesario solicitar una cuenta de usuario.

Consulta los términos de privacidad [Términos de privacidad](https://goo.gl/kIjJhA) y los [Términos y condiciones de uso de FIWARE Lab](https://goo.gl/cVSeNq), así como las [Políticas para obtener una cuenta Community](https://goo.gl/CeWN9b) en el nodo FIWARE Lab de México.

## Tipos de cuenta 
**"Trial"**: Los usuarios "Trial" pueden acceder a FIWARE Lab con recursos limitados para probar las tecnologías FIWARE durante un período corto de tiempo (14 días). Una vez que expira el período de prueba, la cuenta se deshabilita y se liberan los recursos asociados. Si el usuario desea continuar utilizando los recursos a largo plazo, deberán solicitar (y obtener) y una cuenta "Community" previo a la expiración del periodo de prueba.

**"Community"**: Los usuarios "Community" son usuarios que han solicitado formalmente un entorno a largo plazo para trabajar en el desarrollo de aplicaciones basadas en tecnologías FIWARE. Para hacer la solicitud, deberá llenar un formulario en el cual describirá la aplicación que está desarrollando y el uso planificado de los recursos de FIWARE. La solicitud pasa por un proceso de evaluación, si da como resultado una respuesta positiva, se les asignarán recursos en un nodo FIWARE Lab predeterminado y tendrán acceso a él durante un período de 9 meses, con la posibilidad de renovar el proceso de solicitud. 


## Solicita una cuenta "Trial" en FIWARE Lab
1. Ingrese a la página de FIWARE Lab: <https://cloud.lab.fiware.org>
2. Seleccione la opción "Create Account".
  ![CrearcuentaTrial](./images//FL-01.jpg)
  
3. Complete los datos para solicitar una Cuenta "Trial":
    - "Name": ingrese un nombre de usuario para crear su cuenta.
    - "Email": ingrese el correo electrónico que usará para acceder a FIWARE Lab.
    - "FIWARE Lab region": seleccione el nodo de FIWARE en el que desea crear su cuenta. 
    - "I accept the FIWARE Lab Terms & Conditions": acepte los  términos y condiciones de uso de FIWARE Lab. Puede consultar la versión en españól en [Términos y condiciones de uso de FIWARE Lab](https://goo.gl/cVSeNq).

4.	Haga clic en el botón “Submit” para que el formulario de solicitud de cuenta sea enviado. Recibirá un correo electrónico de confirmación desde <communityaccount@fiware.org>. Si no recibe el correo, revise su bandeja de SPAM o contacte  al centro de soporte <fiware-lab-help@lists.fiware.org>.

  ![Crearcuenta](./images//FL-02.jpg)
  
  
## Solicita una cuenta "Community" en FIWARE Lab
Para solicitar una cuenta Community, deberá llenar un formulario de solicitud desde el sitio web de
[FIWARE Lab](https://cloud.lab.fiware.org), donde se le solicita describir de manera breve su proyecto y listar los componentes
de FIWARE que utilizará. Revise las [Políticas para obtener una cuenta Community](https://goo.gl/CeWN9b) en el nodo FIWARE Lab de México para mayor información.

Las instrucciones para solicitar una cuenta Community se describen a continuación. 

1. Ingrese a la página de FIWARE Lab: <https://cloud.lab.fiware.org>
2. Seleccione la opción "Request Community Account".
  ![CrearcuentaCommunity](./images//FL-03.jpg)
  
3.	Complete los datos para solicitar una Cuenta Community:
    - "User full name": ingrese su nombre completo.
    - "User account email":ingrese el correo electrónico que usará para acceder a FIWARE Lab (si ya tiene una cuenta, ingrese el correo electrónico registrado al crear su cuenta).
    - "Are you already registrered in FIWARE Lab? YES – NO": confirme que ha creado una cuenta básica en FIWARE Lab. 
    - "Company": indique la compañía / institución a la que pertenece. 
    - "Department": indique el departamento al que pertenece. 
    - "Number of developers": indique el número de desarrolladores involucrados en su proyecto. 
    - "Startup / Project name": indique el nombre de su startup o proyecto.
    - "Accelerator programe name": seleccione la opción (México).
    - "Accelerator submission name": deje este espacio en blanco. 
    - "Startup / Project description": describa su proyecto incluyendo el objetivo, áreas de aplicación (turismo, salud, seguridad, etc.), descripción de la aplicación que desea desarrollar y sus beneficios, el tiempo estimado de desarrollo y los recursos de FIWARE que impactan a su proyecto.
    - "Preferred FIWARE Lab Node": Seleccione México
    - "Proof of concept URL": (en caso de tenerla) indique la URL donde se pueda acceder a la prueba de concepto de su proyecto (demo).
    - "Number of VMs": indique el número de máquinas virtuales (MVs) que requiere para su proyecto.
    - "Total # vCPUs": indique el número total de CPUs virtuales que utilizarán sus máquinas virtuales. Por ejemplo: si requiere 1 CPU por cada máquina virtual y en total son 3 máquinas virtuales entonces requerirá 3 CPUs. 
    - "Total RAM": indique el total de RAM que requiere en su proyecto (en Gb). Por ejemplo: si requiere 2 Gb por cada máquina virtual y en total son 3 máquinas virtuales entonces requerirá 6 Gb de RAM.  
    - "Total harddisk": indique cuanto espacio de almacenamiento requiere para su proyecto (en Gb). Por ejemplo: si requiere 30 Gb por cada máquina virtual y en total son 3 máquinas virtuales entonces requerirá 90 Gb de espacio de almacenamiento.  
    - "# public IPs": indique el número de IPs públicas que requiere en su proyecto. Por favor considere que se trata de un recurso escaso y costoso. El uso de soluciones como proxies, le permitirá desarrollar sus aplicaciones utilizando solamente una IP. 
    - "Object storage": si requiere un Object Storage (arquitectura de almacenamiento que gestiona datos como objetos), indique cuanto espacio necesita (en Gb). Tenga en cuenta que no todos los nodos cuentan con el servicio de Object Storage. 
    - "Additional comments /requirements / essential questions": indique si tiene algún comentario adicional, requerimiento o pregunta. Por ejemplo: una máquina virtual con más recursos de los máximos establecidos por defecto.  
    - "Name": indique su nombre (opcional).
    - "Email": indique un correo electrónico (opcional).

4.	Haga clic en el botón “Submit” para que el formulario de solicitud de cuenta Community sea enviado. Recibirá un correo electrónico de confirmación desde <communityaccount@fiware.org>. Si no recibe el correo, revise su bandeja de SPAM o contacte  al centro de soporte <fiware-lab-help@lists.fiware.org>.

  ![Crearcuenta](./images//FL-04.jpg)
