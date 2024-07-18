# Comprender a Ansible, Puppet y Chef

**Este capítulo cubre los siguientes temas del examen:**

- Automatización y Programabilidad
  - Reconocer las capacidades de los mecanismos de configuración Puppet, Chef y Ansible.

Hasta ahora, ha visto cómo utilizar la CLI de IOS para configurar enrutadores y conmutadores. Para configurar usando la CLI, ingresa al modo de configuración, emite comandos de configuración (que cambian el archivo de configuración en ejecución) y, finalmente, abandona el modo de configuración. Si decide conservar esos cambios, guarde la configuración en el archivo startup-config usando el comando `copy running-config startup-config`. La próxima vez que se inicie el enrutador o conmutador, el dispositivo carga el archivo de configuración de inicio en la RAM como configuración en ejecución. Suficientemente simple.

En este capítulo se analizan herramientas para la gestión de la configuración que reemplazan el proceso de configuración por dispositivo. Para siquiera imaginar qué hacen estas herramientas primero, es necesario dar un salto de imaginación al mundo cotidiano de un ingeniero de redes en una empresa mediana y grande. En una red en funcionamiento real, gestionar la configuración de muchos dispositivos de red plantea desafíos. Esos desafíos se pueden abordar utilizando el mismo proceso antiguo de “usar el modo de configuración en cada dispositivo”, además de trabajo duro, atención al detalle y buenas prácticas operativas. Sin embargo, ese proceso manual por dispositivo se vuelve cada vez más difícil por diversas razones, por lo que, en algún momento, las empresas recurren a herramientas automatizadas de gestión de configuración para proporcionar mejores resultados.

La primera sección de este capítulo ofrece una mirada generalizada a los problemas de la gestión de la configuración a escala junto con algunas de las soluciones a esos problemas. La segunda sección principal detalla cada una de las tres herramientas de administración de configuración (Ansible, Puppet y Chef) para definir algunas de las características y términos utilizados con cada una. Al final del capítulo, debería poder ver algunas de las razones por las que estas herramientas automatizadas de administración de configuración tienen un papel en las redes modernas y suficiente contexto para comprenderlas al elegir una e investigarla para leer más.
### Desafíos y soluciones de configuración de dispositivos
Piense en cualquier red de producción. ¿Qué define la configuración exacta prevista para cada dispositivo en una red de producción? ¿Es la configuración en ejecución tal como existe ahora o la configuración de inicio antes de que se realizaran cambios recientes o la configuración de inicio del mes pasado? ¿Podría un ingeniero cambiar la configuración del dispositivo para que se aleje de ese ideal, sin que el resto del personal lo sepa? ¿Qué proceso, si lo hay, podría descubrir la desviación de la configuración? E incluso con los cambios acordados por todos, ¿cómo saber quién cambió la configuración, cuándo y específicamente qué cambió?

Tradicionalmente, CCNA nos enseña cómo configurar un dispositivo usando el comando **configure** **terminal** para alcanzar el modo de configuración, que cambia el archivo de configuración en ejecución y cómo guardar ese archivo de configuración en ejecución en la configuración de inicio. archivo. Ese proceso manual no proporciona ningún medio para responder a ninguna de las preguntas legítimas planteadas en el primer párrafo; sin embargo, para muchas empresas, esas preguntas (y otras) necesitan respuestas, tanto consistentes como precisas.

No todas las empresas alcanzan el tamaño para querer hacer algo más con la gestión de la configuración. A las empresas con un solo ingeniero de redes les podría ir bastante bien administrando las configuraciones de los dispositivos, especialmente si las configuraciones de los dispositivos de red no cambian con frecuencia. Sin embargo, a medida que una empresa recurre a múltiples ingenieros de redes y aumenta la cantidad y los tipos de dispositivos, con tasas más altas de cambio de configuración, la gestión manual de la configuración tiene problemas.

Esta sección comienza analizando algunos de estos tipos de problemas de administración de configuración para que comience a comprender por qué las empresas necesitan más que buenas personas y buenas prácticas para abordar la configuración de dispositivos. El resto de la sección detalla algunas de las características que puede encontrar en las herramientas de administración de configuración automatizadas.
### Deriva de configuración
Considere la historia de una empresa de un tamaño que necesita dos ingenieros de redes, Alice y Bob. Ambos tienen experiencia y trabajan bien juntos. Pero las configuraciones de red han crecido más allá de lo que cualquier persona puede saber de memoria, y con dos ingenieros de red, pueden recordar detalles diferentes o incluso estar en desacuerdo en ocasiones.

Una noche, a la 1 de la madrugada, Bob recibe una llamada sobre un problema. Ingresa a la red desde su computadora portátil y resuelve el problema con un pequeño cambio de configuración en el enrutador BR22 de la sucursal. Alice, la ingeniera senior, recibe una llamada diferente a las 4 a. m. sobre otro problema y realiza un cambio en el enrutador BR33 de la sucursal.

El día siguiente está muy ocupado y ni Alice ni Bob mencionan los cambios que hicieron. Ambos siguen procedimientos y documentan los cambios en su sistema de gestión de cambios, que enumera los detalles de cada cambio. Pero ambos están ocupados, el tema nunca surge y ninguno menciona los cambios de cada uno durante meses.

La historia muestra cómo puede ocurrir una desviación de la configuración: un efecto en el que la configuración se aleja de la configuración prevista con el tiempo. Alice y Bob probablemente estén de acuerdo con cómo debería ser la configuración estándar de un enrutador de sucursal, pero ambos hicieron una excepción a esa configuración para solucionar un problema, lo que provocó una desviación de la configuración. La Figura 19-1 muestra la idea básica, con esos dos enrutadores de sucursal ahora con configuraciones ligeramente diferentes a las de los otros enrutadores de sucursal.

**Figure 19-1**        _Configuration Drift in Branch Routers BR22 and BR33_

La desviación de la configuración se convierte en un problema mucho mayor si se utilizan únicamente herramientas de configuración manual tradicionales. Por ejemplo:
- El proceso de configuración manual en el dispositivo no rastrea el historial de cambios: qué líneas cambiaron, qué cambió en cada línea, qué configuración anterior se eliminó, quién cambió la configuración, cuándo se realizó cada cambio.
- Los sistemas externos utilizados por buenos procesos de gestión de sistemas, como el software de gestión de cambios y generación de tickets de problemas, pueden registrar detalles. Sin embargo, estos se encuentran fuera de la configuración y requieren un análisis para descubrir qué cambió. También dependen de los humanos para seguir los procesos operativos de manera consistente y correcta; de lo contrario, un ingeniero no podrá encontrar el historial completo de cambios en una configuración.
- Hacer referencia a datos históricos en los sistemas de gestión de cambios no funciona bien si un dispositivo ha pasado por múltiples cambios de configuración durante un período de tiempo.
### Archivos de configuración centralizados y control de versiones
El modelo de configuración manual por dispositivo tiene mucho sentido para que una persona administre un dispositivo. Con ese modelo, un ingeniero de red puede usar la configuración de inicio en el dispositivo como la configuración ideal prevista. Si es necesario un cambio, el ingeniero ingresa al modo de configuración y actualiza la configuración en ejecución hasta que esté satisfecho con el cambio. Luego, el ingeniero guarda una copia en startup-config como la configuración ideal actual para el dispositivo.

El modelo de configuración manual por dispositivo no funciona tan bien para redes más grandes, con cientos o incluso miles de dispositivos de red, con varios ingenieros de red. Por ejemplo, si el equipo piensa que la configuración de inicio de cada dispositivo es la configuración ideal, si un miembro del equipo cambia la configuración (como lo hicieron Alice y Bob en la historia anterior), no existen registros sobre el cambio. El archivo de configuración no muestra qué cambió, cuándo cambió ni quién lo cambió, y el proceso no notifica a todo el equipo sobre el cambio.

Como primer paso hacia una mejor gestión de la configuración, muchas empresas medianas y grandes almacenan las configuraciones en una ubicación central. Al principio, almacenar archivos de forma centralizada puede ser un simple esfuerzo para mantener copias de seguridad de la configuración de cada dispositivo. Por supuesto, colocarían los archivos en una carpeta compartida accesible para todo el equipo de la red, como se muestra en la Figura 19-2.

**Figure 19-2**          _Copying Device Configurations to a Central Location_

¿Qué archivo de configuración es la única fuente de verdad en este modelo? Los archivos de configuración todavía existen en cada dispositivo, pero ahora también existen en un servidor centralizado, y los ingenieros pueden cambiar la configuración del dispositivo, así como los archivos de texto en el servidor. Por ejemplo, si la copia de la configuración de BR21 en el dispositivo difiere del archivo en el servidor centralizado, lo que debería considerarse correcto, ideal, ¿la verdad sobre lo que el equipo pretende para este dispositivo?

En la práctica, las empresas adoptan ambos enfoques. En algunos casos, las empresas continúan utilizando los archivos de configuración del dispositivo como fuente de verdad, y los archivos de configuración centralizados se tratan como copias de seguridad en caso de que el dispositivo falle y deba ser reemplazado. Sin embargo, otras empresas hacen la transición para tratar los archivos del servidor como la única fuente de verdad sobre la configuración de cada dispositivo. Al utilizar el archivo centralizado como fuente de verdad, los ingenieros pueden aprovechar muchas herramientas de administración de configuración y, de hecho, administrar las configuraciones más fácilmente y con mayor precisión.

Por ejemplo, las herramientas de gestión de configuración utilizan software de control de versiones para rastrear los cambios en los archivos de configuración centralizados, observando quién cambia un archivo, qué líneas y caracteres específicos cambiaron, cuándo ocurrió el cambio, etc. Las herramientas también le permiten comparar las diferencias entre las versiones de los archivos a lo largo del tiempo, como se muestra en la Figura 19-3.

**Figure 19-3**             _Showing File Differences in GitHub_

La figura muestra un ejemplo de una comparación entre dos versiones de un archivo de configuración. Las dos líneas resaltadas superiores, con los signos menos, muestran las líneas que fueron modificadas, mientras que las dos líneas resaltadas inferiores, con los signos más, muestran las nuevas versiones de cada línea.

El software de control de versiones resuelve muchos de los problemas relacionados con la falta de seguimiento de cambios dentro de los propios dispositivos. La Figura 19-3 muestra el resultado de un sitio popular de software como servicio llamado GitHub [(www.github.com)](http://www.github.com/). GitHub ofrece cuentas gratuitas y de pago y utiliza software de código abierto (Git) para realizar las funciones de control de versiones.
### Monitoreo y aplicación de la configuración
Con un sistema de control de versiones y una convención de almacenar los archivos de configuración en una ubicación central, un equipo de red puede hacer un trabajo mucho mejor rastreando los cambios y respondiendo quién, qué y cuándo para saber qué cambió en la configuración de cada dispositivo. Sin embargo, el uso de ese modelo introduce otros desafíos, desafíos que se pueden resolver mejor utilizando también una herramienta de gestión de configuración automatizada.

Con este nuevo modelo, los ingenieros deberían realizar cambios editando los archivos de configuración asociados en el repositorio centralizado. Luego se puede indicar a la herramienta de administración de configuración que copie o aplique la configuración al dispositivo, como se muestra en la Figura 19-4. Una vez que se completa el proceso, el archivo de configuración central y la configuración de ejecución (y la configuración de inicio) del dispositivo deben ser idénticos.

**Figure 19-4**               _Pushing Centralized Configuration to a Remote Device_

Usar el modelo mostrado en la Figura 19-4 todavía presenta peligros. Por ejemplo, los ingenieros de red deben realizar cambios utilizando las herramientas de administración de configuración, pero aún tienen la capacidad de iniciar sesión en cada dispositivo y realizar cambios manuales en cada dispositivo. Entonces, si bien la idea de utilizar una herramienta de administración de configuración con un repositorio centralizado de archivos de configuración suena atractiva, eventualmente alguien cambiará los dispositivos directamente. Los cambios de configuración correctos anteriores podrían sobrescribirse y volverse incorrectos con cambios futuros. En otras palabras, eventualmente puede ocurrir cierta desviación de la configuración.

Las herramientas de administración de configuración pueden monitorear las configuraciones del dispositivo para descubrir cuándo la configuración del dispositivo difiere de la configuración ideal prevista y luego reconfigurar el dispositivo o notificar al personal de ingeniería de redes para realizar el cambio. Esta característica podría denominarse _monitoreo de configuración_ o _cumplimiento de configuración_, particularmente si la herramienta cambia automáticamente la configuración del dispositivo.

La Figura 19-5 muestra la idea general detrás del monitoreo de configuración. El software de administración de configuración automatizada solicita una copia del archivo de configuración en ejecución del dispositivo, como se muestra en los pasos 1 y 2. En el paso 3, el software de administración de configuración compara el archivo de configuración ideal con el archivo de configuración en ejecución que acaba de llegar para verificar si tienen alguna diferencia (derivación de configuración). Según la configuración de la herramienta, corrige la configuración o notifica al personal sobre la variación de la configuración.

**Figure 19-5**        _Configuration Monitoring_
### Configuración de aprovisionamiento
El aprovisionamiento de configuración se refiere a cómo aprovisionar o implementar cambios en la configuración una vez realizados cambiando archivos en el sistema de gestión de configuración. Como una de las funciones principales de una herramienta de administración de configuración, probablemente verá características como estas:

- La función principal para implementar cambios de configuración en un dispositivo después de que alguien haya editado el archivo de configuración centralizado del dispositivo.
- La capacidad de elegir qué subconjunto de dispositivos configurar: todos los dispositivos, tipos con un atributo determinado (como los de una función particular) o solo un dispositivo, según los atributos y
### lógica
- La capacidad de determinar si cada cambio fue aceptado o rechazado, y de utilizar la lógica para reaccionar de manera diferente en cada caso dependiendo del resultado.
- Para cada cambio, la capacidad de volver a la configuración original si se rechaza incluso un comando de configuración en un dispositivo
- La capacidad de validar el cambio ahora (sin realizarlo realmente) para determinar si el cambio funcionará o no cuando se intente.
- La capacidad de verificar la configuración una vez completado el proceso para confirmar que la configuración prevista de la herramienta de administración de configuración coincide con la configuración del dispositivo.
- La capacidad de usar la lógica para elegir si guardar la configuración en ejecución en la configuración de inicio o no.
- La capacidad de representar archivos de configuración como plantillas y variables para que dispositivos con roles similares puedan usar la misma plantilla pero con valores diferentes
- La capacidad de almacenar los pasos lógicos en un archivo, programado para ejecutarse, de modo que la herramienta de automatización pueda implementar los cambios sin que el ingeniero esté presente.

La lista podría ir más allá, pero estas características describen algunas de las características principales incluidas en todas las herramientas de administración de configuración analizadas en este capítulo. La mayoría de los elementos de la lista giran en torno a la edición del archivo de configuración central de un dispositivo. Sin embargo, las herramientas tienen muchas más funciones, por lo que usted tiene más trabajo que hacer para planificar e implementar su funcionamiento. Las próximas páginas se centran en dar algunos detalles más sobre los dos últimos elementos de la lista.
### Plantillas de configuración y variables
Piense en las funciones que desempeñan los dispositivos de red en una empresa. Centrándonos en los enrutadores por un momento, los enrutadores a menudo se conectan tanto a la WAN como a una o más LAN. Es posible que tenga una pequeña cantidad de enrutadores más grandes conectados a la WAN en sitios grandes, con suficiente potencia para manejar velocidades de paquetes más grandes. Los sitios más pequeños, como las sucursales, pueden tener enrutadores pequeños, tal vez con una única interfaz WAN y una única interfaz LAN; sin embargo, es posible que tenga una gran cantidad de esos enrutadores de sucursales pequeñas en la red.

Para cualquier conjunto de dispositivos con la misma función, las configuraciones probablemente sean similares. Por ejemplo, un conjunto de enrutadores de sucursales puede tener exactamente la misma configuración para algunos servicios IP, como NTP o SNMP. Si se utiliza la configuración de interfaz OSPF, los enrutadores en la misma área OSPF y con ID de interfaz idénticas podrían tener una configuración OSPF idéntica.

Por ejemplo, el Ejemplo 19-1 muestra un extracto de configuración de un enrutador de sucursal, con las partes únicas de la configuración resaltadas. Todas las partes no resaltadas podrían ser iguales en todos los demás enrutadores de sucursales del mismo modelo (con los mismos números de interfaz). Una empresa puede tener docenas o cientos de enrutadores para sucursales con una configuración casi idéntica.

**Example 19-1**                  _Router BR1 Configuration, with Unique Values Highlighted_

```
hostname BR1
!
interface GigabitEthernet0/0  ip address 10.1.1.1 255.255.255.0  ip ospf 1 area 11
!
interface GigabitEthernet0/1 ! interface GigabitEthernet0/1/0  ip address 10.1.12.1 255.255.255.0  ip ospf 1 area 11 !
router ospf 1  router-id 1.1.1.1
```

Las herramientas de gestión de configuración pueden separar los componentes de una configuración en las partes comunes a todos los dispositivos en esa función (la plantilla) versus las partes únicas de cualquier dispositivo (las variables). Luego, los ingenieros pueden editar el archivo de plantilla estándar para una función de dispositivo como un archivo separado del archivo variable de cada dispositivo. Luego, la herramienta de administración de configuración puede procesar la plantilla y las variables para crear el archivo de configuración ideal para cada dispositivo, como se muestra en la Figura 19-6, que muestra los archivos de configuración que se están creando para los enrutadores de sucursal BR21, BR22 y BR23.

**Figure 19-6**          _Concept: Configuration Templates and Variables_

Para brindar un poco más de información, el Ejemplo 19-2 muestra un archivo de plantilla que Ansible podría usar para la configuración que se muestra en el Ejemplo 19-1. Cada herramienta especifica qué idioma usar para cada tipo de archivo, y Ansible usa el lenguaje Jinja2 para las plantillas. La plantilla imita la configuración del Ejemplo 19-1, excepto que coloca los nombres de las variables dentro de conjuntos de llaves dobles.

```
hostname {{hostname}}
!
interface GigabitEthernet0/0  ip address {{address1}} {{mask1}}  ip ospf {{OSPF_PID}} area {{area}} !

interface GigabitEthernet0/1 !

interface GigabitEthernet0/1/0
 ip address {{address2}} {{mask2}}  ip ospf {{OSPF_PID}} area {{area}} ! router ospf {{OSPF_PID}}  router-id {{RID}}
```

Para proporcionar los valores para un dispositivo, Ansible solicita definir archivos de variables usando YAML, como se muestra en el Ejemplo 19-3. El archivo muestra la sintaxis para definir las variables que se muestran en la configuración completa en el Ejemplo 19-1, pero ahora definidas como variables.

```
--hostname: BR1 address1: 10.1.1.1 mask1: 255.255.255.0 address2: 10.1.12.1 mask2: 255.255.255.0 RID: 1.1.1.1 area: '11'

OSPF_PID: '1'
```

El sistema de gestión de configuración procesa una plantilla más todas las variables relacionadas para producir la configuración prevista para un dispositivo. Por ejemplo, el ingeniero crearía y editaría un archivo de plantilla similar al Ejemplo 19-2 y luego crearía y editaría un archivo variable como el Ejemplo 19-3 para cada enrutador de sucursal. Ansible procesaría los archivos para crear archivos de configuración completos como el texto que se muestra en el Ejemplo 19-1.

Puede parecer un trabajo extra separar las configuraciones en una plantilla y variables, pero el uso de plantillas tiene grandes ventajas. En particular:

- Las plantillas aumentan el enfoque en tener una configuración estándar para cada función de dispositivo, lo que ayuda a evitar copos de nieve (dispositivos configurados de forma única).
- Se pueden implementar fácilmente nuevos dispositivos con una función existente simplemente copiando un archivo de variables existente por dispositivo y cambiando los valores.
- Las plantillas permiten una resolución de problemas más sencilla porque la resolución de problemas con una plantilla estándar debería encontrar y solucionar problemas con todos los dispositivos que usan la misma plantilla.
- El seguimiento de las versiones de los archivos de la plantilla frente a los archivos de variables también permite una solución de problemas más sencilla. Los problemas con un dispositivo se pueden investigar para encontrar cambios en la configuración del dispositivo por separado de la plantilla de configuración estándar.
### Archivos que controlan la automatización de la configuración
Las herramientas de gestión de configuración también proporcionan diferentes métodos para definir la lógica y los procesos que le indican a la herramienta qué cambios realizar, en qué dispositivos y cuándo. Por ejemplo, un ingeniero podría ordenar a una herramienta que realice cambios durante un período de cambio de fin de semana. Esa misma lógica podría especificar un subconjunto de dispositivos. También podría detallar los pasos para verificar el cambio antes y después de intentarlo, y cómo notificar a los ingenieros si ocurre un problema.

Curiosamente, puedes hacer gran parte de la lógica sin saber programar. Cada herramienta utiliza un lenguaje de algún tipo que los ingenieros utilizan para definir los pasos de acción, a menudo un lenguaje definido por esa empresa (un lenguaje de dominio específico). Pero hacen que los lenguajes sean sencillos y, en general, son mucho más fáciles de aprender que los lenguajes de programación. Las herramientas de gestión de configuración también le permiten ampliar los pasos de acción más allá de lo que se puede hacer en el conjunto de herramientas mediante el uso de un lenguaje de programación general. La Figura 19-7 resume los archivos que puede ver en cualquiera de las herramientas de administración de configuración.
### Conceptos básicos de Ansible, Puppet y Chef
Este capítulo se centra en un tema de examen que pregunta sobre las capacidades de tres herramientas de administración de configuración: Ansible, Puppet y Chef. La primera sección principal del capítulo describe las capacidades de las tres (y otras) herramientas de gestión de configuración. Esta segunda sección principal examina algunas de las características de cada herramienta, centrándose en la terminología y las capacidades principales.

Ansible, Puppet y Chef son paquetes de software. Puede comprar cada herramienta, con variaciones según el paquete. Sin embargo, todos también tienen diferentes opciones gratuitas que le permiten descargar y conocer las herramientas, aunque es posible que necesite ejecutar un invitado de Linux porque algunas de las herramientas no se ejecutan en un sistema operativo Windows.

En cuanto a los nombres, la mayoría de la gente usa las palabras _Ansible_, _Puppet_ y _Chef_ para referirse a las empresas, así como a sus principales productos de gestión de configuración. Los tres surgieron como parte de la transición de servidores basados ​​en hardware a servidores virtualizados, lo que aumentó considerablemente la cantidad de servidores y creó la necesidad de automatización de software para crear, configurar y eliminar máquinas virtuales. Los tres producen uno o más productos de software de gestión de configuración que se han convertido en sinónimo de sus empresas en muchos sentidos. (Este capítulo sigue esa convención, ignorando en su mayor parte los nombres exactos de los productos y refiriéndose a los productos y al software simplemente como Ansible, Puppet y Chef). A continuación, pasemos a algunos detalles sobre cada uno.
### Ansible
Para utilizar Ansible [(www.ansible.com)](http://www.ansible.com/), debe instalar Ansible en alguna computadora: Mac, Linux o una máquina virtual Linux en un host de Windows. Puede utilizar la versión gratuita de código abierto o utilizar la versión paga del servidor Ansible Tower.

Una vez instalado, crea varios archivos de texto, como los siguientes:
- **Playbooks:** Estos archivos proporcionan acciones y lógica sobre lo que debería hacer Ansible.
- **Inventario:** Estos archivos proporcionan nombres de host de dispositivos junto con información sobre cada dispositivo, como roles de dispositivo, para que Ansible pueda realizar funciones para subconjuntos del inventario.
- **Plantillas:** Usando el lenguaje Jinja2, las plantillas representan la configuración de un dispositivo pero con variables (ver Ejemplo 19-2).
- **Variables:** Usando YAML, un archivo puede enumerar variables que Ansible sustituirá en plantillas (ver Ejemplo 19-3).

En cuanto a cómo funciona Ansible para administrar dispositivos de red, utiliza una arquitectura sin agentes. Eso significa que Ansible no depende de ningún código (agente) que se ejecute en el dispositivo de red. En cambio, Ansible se basa en características típicas de los dispositivos de red, a saber, SSH y/o NETCONF, para realizar cambios y extraer información. Cuando se usa SSH, el nodo de control de Ansible en realidad realiza cambios en el dispositivo como lo haría cualquier otro usuario de SSH, pero trabajando con código de Ansible, en lugar de con un humano.

Se puede describir a Ansible como el uso de un modelo push (según la Figura 19-8) en lugar de un modelo pull (como Puppet y Chef). Después de instalar Ansible, un ingeniero necesita crear y editar todos los distintos archivos de Ansible, incluido un libro de estrategias de Ansible. Luego, el ingeniero ejecuta el libro de jugadas, que le indica a Ansible que realice los pasos. Esos pasos pueden incluir la configuración de uno o más dispositivos según los distintos archivos (paso 3), y se considera que el nodo de control envía la configuración al dispositivo.

**Figure 19-8**           _Ansible Push Model_

Al igual que con todas las herramientas, Ansible puede realizar tanto el aprovisionamiento de configuración (configurar dispositivos después de realizar cambios en los archivos) como el monitoreo de configuración (verificar si la configuración del dispositivo coincide con la configuración ideal en el nodo de control). Sin embargo, la arquitectura de Ansible encaja de forma más natural con el aprovisionamiento de configuración, como se ve en la figura. Para realizar el monitoreo de la configuración, Ansible utiliza módulos lógicos que detectan y enumeran las diferencias de configuración, después de lo cual el libro de jugadas define qué acción tomar (reconfigurar o notificar).
### marioneta
Para utilizar Puppet [(www.puppet.com)](http://www.puppet.com/), como Ansible, comience instalando Puppet en un host Linux. Puede instalarlo en su propio host Linux, pero para fines de producción, normalmente lo instalará en un servidor Linux llamado Puppet master. Al igual que con Ansible, puede utilizar una versión gratuita de código abierto con versiones pagas disponibles. Puede comenzar a aprender Puppet sin un servidor separado para aprender y probar.

Una vez instalado, Puppet también utiliza varios archivos de texto importantes con diferentes componentes, como los siguientes:
- **Manifiesto:** Este es un archivo de texto legible por humanos en Puppet Master, que utiliza un lenguaje definido por Puppet y se utiliza para definir el estado de configuración deseado de un dispositivo.
- **Recurso, Clase, Módulo:** Estos términos se refieren a componentes del manifiesto, donde el componente más grande (módulo) está compuesto por clases más pequeñas, que a su vez están compuestas por recursos.
- **Plantillas:** Al utilizar un lenguaje específico del dominio de Puppet, estos archivos le permiten a Puppet generar manifiestos (y módulos, clases y recursos) sustituyendo variables en la plantilla.

Una forma de pensar en las diferencias entre el enfoque de Ansible y el de Puppet es que los manuales de Ansible utilizan un lenguaje imperativo, mientras que Puppet utiliza un lenguaje declarativo. Por ejemplo, con Ansible, el manual enumerará tareas y opciones basadas en esos resultados, como "Configure todos los enrutadores de sucursales en estas ubicaciones y, si ocurren errores en algún dispositivo, realice estas tareas adicionales para ese dispositivo". En cambio, los manifiestos títeres declaran el estado final que debe tener un dispositivo: "Este enrutador de sucursal debe tener la configuración en este archivo al final del proceso". El manifiesto, creado por el ingeniero, define el estado final, y Puppet tiene la tarea de hacer que el dispositivo tenga esa configuración, sin que se le indique el conjunto específico de pasos a seguir.

Puppet suele utilizar una arquitectura basada en agentes para admitir dispositivos de red. Algunos dispositivos de red habilitan la compatibilidad con Puppet a través de un agente en el dispositivo; considérelo como otra característica configurable en el dispositivo. Sin embargo, no todos los sistemas operativos de Cisco admiten agentes Puppet, por lo que Puppet resuelve ese problema utilizando un agente proxy que se ejecuta en algún host externo (lo que se denomina operación sin agente). Luego, el agente externo usa SSH para comunicarse con el dispositivo de red, como se muestra en la Figura 19-9.

**Figure 19-9**          _Agent-based and Agentless Operation for Puppet_

Armado con un manifiesto que declara algo como "Este dispositivo debe tener este estado de configuración", Puppet utiliza un modelo de extracción para que esa configuración aparezca en el dispositivo, como se muestra en la Figura 19-10. Una vez instalado, se siguen estos pasos:

| **Step 1.** | The engineer creates and edits all the files on the Puppet server.                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Step 2.** | The engineer configures and enables the on-device agent or a proxy agent for each device.                                                                                       |
| **Step 3.** | The agent pulls manifest details from the server, which tells the agent what its configuration should be.                                                                       |
| **Step 4.** | If the agent device’s configuration should be updated, the Puppet agent performs additional pulls to get all required detail, with the agent updating the device configuration. |

**Figure 19-10**            _Pull Model with Puppet_
### Chef
Chef [(www.chef.io)](http://www.chef.io/), al igual que Ansible y Puppet, existe como paquetes de software que usted instala y ejecuta. Chef (la empresa) ofrece varios productos, siendo Chef Automate el producto al que la mayoría de la gente se refiere simplemente como Chef. Al igual que con Puppet, en producción probablemente ejecute Chef como un servidor (llamado modo servidor-cliente), con múltiples estaciones de trabajo Chef utilizadas por el personal de ingeniería para crear archivos Chef que se almacenan en el servidor Chef. Sin embargo, también puede ejecutar Chef en modo independiente (llamado Chef Zero), lo cual resulta útil cuando recién está comenzando y aprendiendo en el laboratorio.

Una vez instalado Chef, crea varios archivos de texto con diferentes componentes, como los siguientes:
- **Recurso:** Los objetos de configuración cuyo estado es administrado por Chef; por ejemplo, un conjunto de comandos de configuración para un dispositivo de red, análogo a los ingredientes de una receta en un libro de cocina.
- **Receta:** La lógica del Chef aplicada a los recursos para determinar cuándo, cómo y si actuar contra los recursos, análoga a una receta en un libro de cocina.
- **Libros de cocina:** un conjunto de recetas sobre los mismos tipos de trabajo, agrupadas para facilitar su administración y uso compartido.
- **Lista de ejecución:** una lista ordenada de recetas que deben ejecutarse en un dispositivo determinado

Chef utiliza una arquitectura similar a Puppet. Para los dispositivos de red, cada dispositivo administrado (llamado nodo Chef o cliente Chef) ejecuta un agente. El agente realiza un monitoreo de la configuración en el sentido de que el cliente extrae recetas y recursos del servidor Chef y luego ajusta su configuración para permanecer sincronizada con los detalles de esas recetas y listas de ejecución. Sin embargo, tenga en cuenta que Chef requiere un código de cliente Chef en el dispositivo y muchos dispositivos Cisco no admiten un cliente Chef, por lo que probablemente verá un mayor uso de Ansible y Puppet para la administración de la configuración de dispositivos Cisco.
### Resumen de herramientas de gestión de configuración
Las tres herramientas de gestión de configuración enumeradas aquí tienen una buena base de usuarios y diferentes puntos fuertes. En cuanto a su uso para administrar la configuración de dispositivos de red, Ansible parece tener el mayor interés, luego Puppet y luego Chef. La arquitectura sin agentes de Ansible y el uso de SSH brindan soporte para una amplia gama de dispositivos Cisco. El modelo sin agentes de Puppet también crea un amplio soporte para dispositivos Cisco.

La Tabla 19-2 resume algunas de las ideas más comunes sobre cada una de las tres herramientas de gestión de configuración automatizada. Tenga en cuenta que la columna de Puppet supone un agente en el dispositivo.

| **Action**                           | **Ansible**  | **Puppet**  | **Chef**        |
| ------------------------------------ | ------------ | ----------- | --------------- |
| Term for the file that lists actions | Playbook     | Manifest    | Recipe, Runlist |
| Protocol to network device           | SSH, NETCONF | HTTP (REST) | HTTP (REST)     |
| Uses agent or agentless model        | Agentless    | Agent*      | Agent           |
| Push or pull model                   | Push         | Pull        | Pull            |