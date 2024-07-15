![](img/switch-1.png)

# Servicios diversos de IP

Este capítulo cubre los siguientes temas del examen:

- Conectividad IP
	  - Describir el propósito del protocolo de redundancia del primer salto.
- Servicios de infraestructura
	  - Explicar la función de SNMP en las operaciones de red.
	  - Describir las capacidades y función de TFTP/FTP en la red.

El capítulo comienza con los protocolos de redundancia de primer salto (FHRP), que proporcionan redundancia para la función del enrutador predeterminado en cualquier subred. El término _FHRP_ se refiere a una clase de soluciones, con tres opciones y con ejemplos que muestran la opción más popular, el Protocolo de enrutador Hot Standby (HSRP).

El Protocolo simple de administración de red (SNMP) sigue en la segunda sección principal. Según el tema del examen asociado, esta sección se centra en los conceptos de SNMP en lugar de la configuración, incluida cómo los sistemas de administración de red (clientes SNMP) pueden interrogar los dispositivos administrados (agentes SNMP) para encontrar el estado actual de cada dispositivo.

El Protocolo de transferencia de archivos (FTP) y el Protocolo trivial de transferencia de archivos (TFTP) protagonizan la tercera sección principal. La primera rama de esta sección se centra en algunos usos prácticos de TFTP y FTP, específicamente cómo utilizar estos protocolos en enrutadores Cisco para actualizar el IOS. Armado con ese conocimiento práctico, verá los detalles del protocolo tanto de FTP como de TFTP en el resto de la sección.
### First Hop Redundancy Protocol
Cuando las redes utilizan un diseño que incluye enrutadores, conmutadores, enlaces LAN y enlaces WAN redundantes, en algunos casos se requieren otros protocolos para aprovechar esa redundancia y evitar los problemas causados ​​por ella.

Por ejemplo, imagine una WAN con muchas sucursales remotas. Si cada sucursal remota tiene dos enlaces WAN que la conectan con el resto de la red, esos enrutadores pueden usar un protocolo de enrutamiento IP para elegir las mejores rutas. El protocolo de enrutamiento aprende rutas a través de ambos enlaces WAN y agrega la mejor ruta a la tabla de enrutamiento. Cuando falla el mejor enlace WAN, el protocolo de enrutamiento agrega la ruta alternativa a la tabla de enrutamiento IP, aprovechando la redundancia.
  
Como otro ejemplo, considere una LAN con enlaces y conmutadores redundantes. Esas LAN tienen problemas a menos que los conmutadores utilicen Spanning Tree Protocol (STP) o Rapid STP (RSTP). STP/RSTP evita los problemas creados por las tramas que recorren esas rutas redundantes adicionales en la LAN.

Esta sección examina otro tipo de protocolo que ayuda cuando una red utiliza cierta redundancia, esta vez con enrutadores predeterminados redundantes. Cuando dos o más enrutadores se conectan a la misma subred LAN, todos esos enrutadores podrían usarse como enrutador predeterminado para los hosts de la subred. Sin embargo, para aprovechar al máximo los enrutadores predeterminados redundantes, se necesita otro protocolo. El término _Protocolo de redundancia de primer salto_ (FHRP) se refiere a la categoría de protocolos que se pueden utilizar para que los hosts aprovechen los enrutadores redundantes en una subred.

Esta primera sección importante del capítulo analiza los conceptos principales detrás de cómo funcionan los diferentes FHRP. Esta sección comienza analizando la necesidad de redundancia de una red en general y la necesidad de enrutadores predeterminados redundantes. Luego muestra cómo las tres opciones FHRP disponibles pueden resolver los problemas que ocurren cuando se utilizan enrutadores predeterminados redundantes.
### La necesidad de redundancia en las redes
Las redes necesitan enlaces redundantes para mejorar la disponibilidad de esas redes. Con el tiempo, algo en una red fallará. La fuente de alimentación de un enrutador podría fallar, un cable podría romperse o un interruptor podría perder energía. Y esos enlaces WAN, que se muestran como líneas simples en la mayoría de los dibujos de este libro, son en realidad las partes físicas más complicadas de la red, con muchas partes individuales que también pueden fallar.

Dependiendo del diseño de la red, la falla de un solo componente podría significar una interrupción que afecte al menos a una parte de la población de usuarios. Los ingenieros de redes se refieren a cualquier componente que, si falla, derriba esa parte de la red como un "punto único de falla". Por ejemplo, en la Figura 12-1, las LAN parecen tener cierta redundancia, mientras que la WAN no. Si la mayor parte del tráfico fluye entre sitios, existen muchos puntos únicos de falla, como se muestra en la figura.

![](img/12.1.png)

La figura señala varios componentes como un único punto de falla. Si alguna de las partes notadas de la red falla, los paquetes no pueden fluir desde el lado izquierdo de la red hacia la derecha.

En términos generales, para mejorar la disponibilidad, el ingeniero de redes primero analiza un diseño y encuentra los puntos únicos de falla. Luego, el ingeniero elige dónde agregarlo a la red para que uno (o más) puntos únicos de falla ahora tenga opciones redundantes, lo que aumenta la disponibilidad. En particular, el ingeniero
- Agrega dispositivos y enlaces redundantes
- Implementa cualquier función necesaria que aproveche el dispositivo o enlace redundante.

Por ejemplo, de todos los puntos únicos de falla en la Figura 12-1, el más costoso a largo plazo probablemente sería el enlace WAN debido al cargo mensual continuo. Sin embargo, estadísticamente, los enlaces WAN son el componente con mayor probabilidad de fallar. Por lo tanto, una actualización razonable de la red de la Figura 12-1 sería agregar un enlace WAN y posiblemente incluso conectarse a otro enrutador en el lado derecho de la red, como se muestra en la Figura 12-2.

![](img/12.2.png)

Many real enterprise networks follow designs like Figure 12-2, with one router at each remote site, two WAN links connecting back to the main site, and redundant routers at the main site (on the right side of the figure). Compared to Figure 12-1, the design in Figure 12-2 has fewer single points of failure. Of the remaining single points of failure, a risk remains, but it is a calculated risk. For many outages, a reload of the router solves the problem, and the outage is short. But the risk still exists that the switch or router hardware fails completely and requires time to deliver a replacement device on-site before that site can work again.

For enterprises that can justify more expense, the next step in higher availability for that remote site is to protect against those catastrophic router and switch failures. In this particular design, adding one router on the left side of the network in Figure 12-2 removes all the single points of failure that had been noted earlier. Figure 12-3 shows the design with a second router, which connects to a different LAN switch so that SW1 is also no longer a single point of failure.

![](img/12.3.png)

### La necesidad de un protocolo de redundancia del primer salto
De los diseños mostrados hasta ahora en este capítulo, solo el diseño de la Figura 12-3 tiene dos enrutadores para admitir la LAN en el lado izquierdo de la figura, específicamente la misma VLAN y subred. Si bien es útil tener enrutadores redundantes en la misma subred, la red necesita usar un FHRP cuando existen estos enrutadores redundantes.

Para ver la necesidad y el beneficio de usar un FHRP, primero piense en cómo los hosts en la VLAN 10/subred 10.1.1.0/24 podrían usar estos enrutadores redundantes como enrutadores predeterminados, como se muestra en la Figura 12-4. La lógica del host permanecerá sin cambios, por lo que cada host tiene una única configuración de enrutador predeterminada. Entonces, algunas opciones de diseño para la configuración predeterminada del enrutador incluyen las siguientes:
- Todos los hosts en la subred usan R1 (10.1.1.9) como su enrutador predeterminado y reconfiguran estáticamente su configuración de enrutador predeterminado a 10.1.1.129 de R2 si falla R1.
- Todos los hosts en la subred usan R2 (10.1.1.129) como su enrutador predeterminado y reconfiguran estáticamente su configuración de enrutador predeterminado a 10.1.1.9 de R1 si falla R2.
- La mitad de los hosts usa R1 y la otra mitad usa R2 como enrutador predeterminado y, si cualquiera de los enrutadores falla, la mitad de los usuarios reconfigura estáticamente su configuración de enrutador predeterminada.

Para asegurarse de que el concepto quede claro, la Figura 12-4 muestra esta tercera opción, con la mitad de los hosts usando R1 y la otra mitad usando R2. La figura elimina todos los conmutadores LAN solo para ordenar la figura. Los hosts A y B usan R1 como enrutador predeterminado y los hosts C y D usan R2 como enrutador predeterminado.

![](img/12.4.png)

Todas estas opciones tienen un problema: los usuarios tienen que actuar. Tienen que saber que ocurrió un corte. Deben saber cómo reconfigurar la configuración predeterminada de su enrutador. Y tienen que saber cuándo volver a cambiarlo a la configuración original.

Los FHRP hacen que este diseño funcione mejor. Los dos enrutadores parecen ser un único enrutador predeterminado. Los usuarios nunca tienen que hacer nada: la configuración predeterminada de su enrutador sigue siendo la misma e incluso su tabla ARP sigue siendo la misma.

Para permitir que los hosts permanezcan sin cambios, los enrutadores deben trabajar un poco más, según lo define uno de los protocolos FHRP. Genéricamente, cada FHRP hace que suceda lo siguiente:

**1.** Todos los hosts actúan como siempre lo han hecho, con una configuración de enrutador predeterminada que nunca tiene que cambiar.
**2.** Los enrutadores predeterminados comparten una dirección IP virtual en la subred, definida por FHRP.
**3.** Los hosts utilizan la dirección IP virtual FHRP como dirección de enrutador predeterminada.
**4.**  Los enrutadores intercambian mensajes del protocolo FHRP para que ambos acuerden qué enrutador hace qué trabajo en cualquier momento.
**5.** Cuando un enrutador falla o tiene algún otro problema, los enrutadores usan el FHRP para elegir qué enrutador asume las responsabilidades del enrutador fallido.
### Las tres soluciones para la redundancia del primer salto
El término _Protocolo de redundancia de primer salto_ no nombra ningún protocolo en particular. En cambio, nombra una familia de protocolos que cumplen la misma función. Para una red determinada, como la del lado izquierdo de la Figura 12-4, el ingeniero elegiría uno de los protocolos de la familia FHRP.

La Tabla 12-2 enumera los tres protocolos FHRP en orden cronológico, según cuándo se utilizaron por primera vez. Cisco introdujo por primera vez el protocolo propietario Hot Standby Router Protocol (HSRP) y funcionó bien para muchos de sus clientes. Posteriormente, el IETF desarrolló un RFC para un protocolo similar, el Protocolo de redundancia de enrutador virtual (VRRP). Finalmente, Cisco desarrolló una opción más sólida, el Protocolo de equilibrio de carga de puerta de enlace (GLBP).

| **Acronym** | **Full Name**                      | **Origin** | **Redundancy Approach** | **Load Balancing Per…** |
| ----------- | ---------------------------------- | ---------- | ----------------------- | ----------------------- |
| HSRP        | Hot Standby Router Protocol        | Cisco      | active/standby          | subnet                  |
| VRRP        | Virtual Router Redundancy Protocol | RFC 5798   | active/standby          | subnet                  |
| GLBP        | Gateway Load Balancing Protocol    | Cisco      | active/active           | host                    |

This chapter focuses on HSRP and does not discuss VRRP and GLBP other than this brief mention. HSRP, the first of the three FHRP protocols to enter the market, remains a popular option in many networks. The current CCNA 200-301 exam requires you to know the functions of an FHRP, so the example of HSRP meets that need, with the next few pages walking through the concepts of how HSRP works. (Note that Appendix D, “Topics from Previous Editions,” contains a section with more depth about GLBP, copied from an earlier edition of the book, as well as a section on HSRP configuration if you are interested in reading more that goes beyond the current exam’s topics.)
### Conceptos HSRP
HSRP opera con un modelo activo/en espera (también llamado más generalmente _activo/pasivo_). HSRP permite que dos (o más) enrutadores cooperen, todos dispuestos a actuar como enrutador predeterminado. Sin embargo, en un momento dado, sólo un enrutador admite activamente el tráfico del usuario final. Los paquetes enviados por los hosts a su enrutador predeterminado fluyen hacia ese enrutador activo. Luego, los otros enrutadores, con un estado de espera HSRP, se sientan allí esperando pacientemente para tomar el control en caso de que el enrutador HSRP activo tenga un problema.

El enrutador activo HSRP implementa una dirección IP virtual y una dirección MAC virtual coincidente. Esta dirección IP virtual existe como parte de la configuración HSRP, que es un elemento de configuración adicional en comparación con el subcomando de interfaz **dirección IP** habitual. Esta dirección IP virtual está en la misma subred que la dirección IP de la interfaz, pero es una dirección IP diferente. Luego, el enrutador crea automáticamente la dirección MAC virtual. Todos los enrutadores HSRP que cooperan conocen estas direcciones virtuales, pero solo el enrutador activo HSRP utiliza estas direcciones en un momento determinado.

Los hosts se refieren a la dirección IP virtual como la dirección de su enrutador predeterminado, en lugar de la dirección IP de la interfaz de cualquier enrutador. Por ejemplo, en la Figura 12-5, R1 y R2 usan HSRP. La dirección IP virtual de HSRP es 10.1.1.1, y la dirección MAC virtual se denomina VMAC1 en aras de la simplicidad.

![](img/12.5.png)

### Conmutación por error HSRP
HSRP en cada enrutador tiene algo de trabajo por hacer para que la red funcione como se muestra en la Figura 12-5. Los dos enrutadores necesitan configuración HSRP, incluida la dirección IP virtual. Los dos enrutadores se envían mensajes HSRP entre sí para negociar y decidir qué enrutador debe estar activo actualmente y cuál debe estar en espera. Luego, los dos enrutadores continúan enviándose mensajes entre sí para que el enrutador en espera sepa cuándo falla el enrutador activo y pueda asumir el control como el nuevo enrutador activo.

La Figura 12-6 muestra el resultado cuando falla R1, el enrutador activo HSRP en la Figura 12-5. R1 deja de usar la dirección IP y MAC virtual, mientras que R2, el nuevo enrutador activo, comienza a usar estas direcciones. Los hosts no necesitan cambiar la configuración predeterminada del enrutador en absoluto, y el tráfico ahora fluye hacia R2 en lugar de R1.

![](img/12.6.png)

Cuando ocurre la conmutación por error, se producen algunos cambios, pero ninguno de esos cambios ocurre en los hosts. El host mantiene la misma configuración predeterminada del enrutador, configurada en la dirección IP virtual (10.1.1.1 en este caso). La tabla ARP del host tampoco tiene que cambiar, y la MAC virtual HSRP aparece como la dirección MAC del enrutador virtual.

Cuando se produce la conmutación por error, se producen cambios tanto en los enrutadores como en los conmutadores LAN. Claramente, el nuevo enrutador activo debe estar listo para recibir paquetes (encapsulados dentro de tramas) utilizando las direcciones IP y MAC virtuales. Sin embargo, los conmutadores LAN, ocultos en las últimas figuras, anteriormente enviaban tramas destinadas a VMAC1 al enrutador R1. Ahora los conmutadores deben saber cómo enviar las tramas al nuevo enrutador activo, R2.

Para que los conmutadores cambien sus entradas de la tabla de direcciones MAC para VMAC1, R2 envía una trama Ethernet con VMAC1 como dirección MAC de origen. Los conmutadores, como es normal, aprenden la dirección MAC de origen (VMAC1), pero con nuevos puertos que apuntan hacia R2. La trama también es una transmisión LAN, por lo que todos los conmutadores aprenden una entrada de la tabla MAC para VMAC1 que conduce hacia R2. (Por cierto, esta trama Ethernet contiene un mensaje de respuesta ARP, llamado ARP gratuito, porque el enrutador lo envía sin recibir primero una solicitud ARP).
### HSRP Load Balancing
El modelo activo/en espera de HSRP significa que en una subred todos los hosts envían sus paquetes fuera de la subred a través de un solo enrutador. En otras palabras, los enrutadores no comparten la carga de trabajo, y un enrutador maneja todos los paquetes. Por ejemplo, en la Figura 12-5, R1 era el enrutador activo, por lo que todos los hosts de la subred enviaron sus paquetes a través de R1 y ninguno de los hosts de la subred envió sus paquetes a través de R2.

HSRP admite el equilibrio de carga al preferir que diferentes enrutadores sean el enrutador activo en diferentes subredes. La mayoría de los sitios que requieren un segundo enrutador para redundancia también son lo suficientemente grandes como para usar varias VLAN y subredes en el sitio. Es probable que los dos enrutadores se conecten a todas las VLAN, actuando como el enrutador predeterminado en cada VLAN. Luego, HSRP se puede configurar para preferir un enrutador como activo en una VLAN y otro enrutador como activo en otra VLAN, equilibrando el tráfico. O puede configurar varias instancias de HSRP en la misma subred (llamadas grupos HSRP múltiples), prefiriendo que un enrutador esté activo en un grupo y que el otro enrutador esté activo en otro.

Por ejemplo, la Figura 12-7 muestra una LAN rediseñada, ahora con dos hosts en la VLAN 1 y dos hosts en la VLAN 2. Tanto el R1 como el R2 se conectan a la LAN y ambos usan una VLAN troncal y un enrutador en un dispositivo ( configuración ROAS). Ambos enrutadores utilizan HSRP en cada una de las dos subredes, apoyándose mutuamente. Sin embargo, a propósito, el R1 se configuró para que gane la negociación para convertirse en HSRP activo en la VLAN 1, y el R2 se configuró para ganar en la VLAN 2.

![](img/12.7.png)

Tenga en cuenta que al hacer que cada enrutador actúe como enrutador activo HSRP en algunas subredes, el diseño utiliza ambos enrutadores y ambos enlaces WAN.

Los FHRP son necesarios en cualquier dispositivo que actúe como enrutador predeterminado, lo que por supuesto incluye tanto enrutadores tradicionales como conmutadores de capa 3. HSRP se puede configurar en enrutadores y conmutadores de capa 3 en interfaces que tienen direcciones IP configuradas. Sin embargo, en la mayoría de los casos, HSRP se utiliza en interfaces de subredes que tienen hosts que necesitan utilizar un enrutador predeterminado. Esas interfaces incluyen interfaces físicas del enrutador, subinterfaces troncales del enrutador e interfaces virtuales conmutadas (SVI) de capa 3.
### Simple Network Management Protocol
En 1988, se publicó el RFC 1065, “Estructura e identificación de la información de gestión para Internet basadas en TCP/IP”. La idea detrás de este documento era el hecho de que la información sobre los dispositivos en una red basada en TCP/IP (ajustes de configuración, información de estado, contadores, etc.) podría dividirse en una base de datos de variables. Luego, esas variables podrían recopilarse mediante software de gestión para monitorear y administrar la red basada en IP. Después de todo, los elementos de cualquier máquina basada en IP tendrían puntos en común. Por ejemplo, una PC, una impresora de red y un enrutador tendrían puntos en común, como interfaces, direcciones IP y buffers. ¿Por qué no crear una base de datos estandarizada de estas variables y un sistema simple para monitorearlas y gestionarlas? Esta idea fue brillante, tuvo éxito y se convirtió en lo que hoy conocemos como _Protocolo simple de administración de red_ (SNMP).

Esta segunda de las tres secciones principales del capítulo centra ahora nuestra atención en SNMP, analizando los conceptos principales junto con las dos versiones comunes que se utilizan en la actualidad: SNMPv2c y SNMPv3.

SNMP es un protocolo de capa de aplicación que proporciona un formato de mensaje para la comunicación entre los denominados _administradores_ y _agentes_. Un administrador SNMP es una aplicación de administración de red que se ejecuta en una PC o servidor, y ese host generalmente se denomina estación de administración de red (NMS). Existen muchos agentes SNMP en la red, uno por dispositivo administrado. El agente SNMP es un software que se ejecuta dentro de cada dispositivo (enrutador, conmutador, etc.), con conocimiento de todas las variables de ese dispositivo que describen la configuración, el estado y los contadores del dispositivo. El administrador SNMP utiliza protocolos SNMP para comunicarse con cada agente SNMP.

Cada agente mantiene una base de datos de variables que conforman los parámetros, estados y contadores de las operaciones del dispositivo. Esta base de datos, llamada Base de información de gestión (MIB), tiene algunos elementos centrales en común en la mayoría de los dispositivos de red. También tiene una gran cantidad de variables exclusivas de ese tipo de dispositivo; por ejemplo, las MIB del enrutador incluirán variables que no son necesarias en las MIB del conmutador, y viceversa. (Para tener una perspectiva, hice una verificación rápida en un enrutador al escribir esta sección y encontré un poco más de 7000 variables MIB en un enrutador).

La figura 12-8 conecta algunas de estas ideas y términos. En primer lugar, muchas empresas venden productos de gestión SNMP; por ejemplo, la serie de productos de gestión Cisco Prime [(www.cisco.com/go/prime)](http://www.cisco.com/go/prime) utiliza SNMP ( y otros protocolos) para gestionar redes. El IOS en enrutadores y conmutadores incluye un agente SNMP, con MIB incorporado, que se puede habilitar con la configuración que se muestra más adelante en este capítulo.

![](img/12.8.png)

### Lectura y escritura de variables SNMP: obtención y configuración de SNMP

El NMS normalmente sondea al agente SNMP en cada dispositivo. El NMS puede notificar al usuario humano frente a la PC o enviar correos electrónicos, mensajes de texto, etc. para notificar al personal de operaciones de la red sobre cualquier problema identificado por los datos encontrados al sondear los dispositivos. Incluso puede reconfigurar el dispositivo a través de estas variables SNMP en la MIB si permite este nivel de control.

Específicamente, el NMS utiliza los mensajes SNMP Get, GetNext y GetBulk (denominados juntos simplemente mensajes Get) para solicitar información a un agente. El NMS envía un mensaje SNMP Set para escribir variables en el agente SNMP como medio para cambiar la configuración del dispositivo. Estos mensajes vienen en pares: por ejemplo, una solicitud de obtención solicita al agente el contenido de una variable y la respuesta de obtención proporciona esa información. La Figura 12-9 muestra un ejemplo de un flujo típico, en el que el NMS utiliza un Get SNMP para solicitar la variable MIB que describe el estado de una interfaz de enrutador en particular.

![](img/12.9.png)

SNMP permite mucha flexibilidad en la forma de monitorear las variables en la MIB. Lo más habitual es que un administrador de red recopile y almacene estadísticas a lo largo del tiempo utilizando el NMS. El NMS, con los datos almacenados, puede analizar diversos hechos estadísticos, como promedios, mínimos y máximos. Para ser proactivos, los administradores pueden establecer umbrales para ciertas variables clave, indicando al NMS que envíe una notificación (correo electrónico, mensaje de texto, etc.) cuando se supere un umbral.
### Notificaciones SNMP: capturas e informes
Además de solicitar información con los comandos Obtener y configurar variables en los agentes con el comando `set`, los agentes SNMP pueden iniciar comunicaciones con el NMS. Estos mensajes, generalmente llamados notificaciones, utilizan dos mensajes SNMP específicos: `Trap` e `Inform`. Los agentes SNMP envían un mensaje SNMP `Trap` o `Inform` al NMS para enumerar el estado de ciertas variables MIB cuando esas variables alcanzan un estado determinado.

Como ejemplo de una trampa, supongamos que la interfaz G0/0 del enrutador 1 falla, como se muestra en el paso 1 de la figura 12-10. Con las trampas configuradas, el enrutador enviaría un mensaje de captura SNMP al NMS, y ese mensaje de captura indicaría el estado inactivo de la interfaz G0/0. Luego, el software NMS puede enviar un mensaje de texto al personal de soporte de la red, abrir una ventana emergente en la pantalla del NMS, cambiar el color del icono del enrutador correcto a rojo en la interfaz gráfica, etc.

![](img/12.10.png)

Los mensajes SNMP `Traps` e `Inform` tienen exactamente el mismo propósito pero difieren en los mecanismos del protocolo. Las capturas SNMP, disponibles desde la primera versión de SNMP de finales de la década de 1980 (SNMP Versión 1 o SNMPv1), utilizan un proceso de disparar y olvidar. El agente SNMP envía la captura a la dirección IP del NMS, con UDP como protocolo de transporte como ocurre con todos los mensajes SNMP y sin recuperación de errores en la capa de aplicación. Si llega el Trap, genial; si se pierde en tránsito, se pierde.

Los mensajes de información son como los mensajes traps pero con mayor confiabilidad. Agregado al protocolo con SNMP Versión 2 (SNMPv2), Informs todavía usa UDP pero agrega confiabilidad a la capa de aplicación.

El NMS debe acusar recibo del informe con un mensaje de respuesta SNMP, o el agente SNMP expirará y reenviará el informe.

Tenga en cuenta que tanto los Traps como los inform tienen una función útil en la actualidad y los Traps todavía se utilizan con frecuencia. Ambos informan al NMS. Los Traps utilizan menos gastos generales para el agente, mientras que los inform mejoran la confiabilidad de los mensajes pero requieren un poco más de esfuerzo general.
### La base de información de gestión
Cada agente SNMP tiene su propia Base de Información de Gestión. La MIB define variables cuyos valores son establecidos y actualizados por el agente. Las variables MIB en los dispositivos de la red permiten que el software de gestión monitoree/controle el dispositivo de red.

Más formalmente, el MIB define cada variable como un _ID de objeto_ (OID). En la mayoría de los dispositivos, la MIB organiza los OID basándose en parte en los estándares RFC y en parte con variables propiedad del proveedor. La MIB organiza todas las variables en una jerarquía de OID, que normalmente se muestra como un árbol. Cada nodo del árbol se puede describir según la secuencia de la estructura del árbol, ya sea por nombre o por número. La Figura 12-11 muestra una pequeña parte de la estructura de árbol de una MIB que resulta ser parte de la parte de la MIB propiedad de Cisco.

![](img/12.11.png)

Trabajar directamente con una MIB, con nombres y números de variables largos, puede ser un desafío, por lo que el software NMS generalmente oculta la complejidad de la numeración y los nombres de las variables MIB. Sin embargo, para tener una idea de los nombres de las variables, la Figura 12-11 muestra la estructura de árbol para dos variables, siendo los nombres de las variables la larga cadena de números que se muestra en la parte inferior de la figura. Trabajar con esos números y la estructura de árbol puede resultar, en el mejor de los casos, difícil. Como resultado, la mayoría de las personas administran sus redes mediante un NMS como Cisco Prime. Para tener una perspectiva, puede usar un administrador SNMP y escribir la variable MIB 1.3.6.1.4.1.9.2.1.58.0 y hacer clic en un botón para obtener esa variable y ver el porcentaje de uso actual de la CPU de un enrutador Cisco. Sin embargo, la mayoría de los usuarios de un NMS preferirían ignorar esos detalles y tener una interfaz gráfica simple para solicitar la misma información, sin tener que saber nunca que 1.3.6.1.4.9.2.1.58.0 representa la variable MIB de utilización de CPU del enrutador.
### Proteger SNMP
SNMP admite algunos mecanismos de seguridad, dependiendo en parte de la versión particular. Esta sección funciona a través de las opciones.

En primer lugar, un método eficaz para proteger SNMP es utilizar ACL para limitar los mensajes SNMP a aquellos provenientes únicamente de servidores conocidos. Los agentes SNMP en los enrutadores y conmutadores Cisco admiten mensajes SNMP que fluyen en paquetes IPv4 e IPv6. El agente SNMP puede configurar una ACL IPv4 para filtrar mensajes SNMP entrantes que llegan en paquetes IPv4 y una ACL IPv6 para filtrar mensajes SNMP que llegan en paquetes IPv6.

Usar una ACL IPv4 e IPv6 para proteger a un agente tiene sentido. Los únicos hosts que deberían enviar mensajes SNMP al agente SNMP en un enrutador o conmutador son los hosts NMS. Esos hosts NMS rara vez se mueven y el personal de redes debe conocer bien sus direcciones IP. Tiene sentido configurar una ACL que permita paquetes provenientes de las direcciones IP de todos los hosts NMS, pero no de otros.

En cuanto a los mensajes del protocolo SNMP, todas las versiones de SNMP admiten un mecanismo básico de contraseña de texto sin cifrar, aunque ninguna de esas versiones se refiere al mecanismo como el uso de una contraseña. SNMP Versión 3 (SNMPv3) también agrega seguridad más moderna.

SNMPv1 definió contraseñas de texto sin cifrar llamadas SNMP _communities_. Básicamente, tanto el agente SNMP como el administrador SNMP necesitan conocimiento previo del mismo valor de comunidad SNMP (llamado _cadena de comunidad_). Los mensajes Obtener SNMP y el mensaje Establecer incluyen el valor de cadena de comunidad apropiado, en texto claro. Si el NMS envía un Get o Set con la cadena de comunidad correcta, según lo configurado en el agente SNMP, el agente procesa el mensaje.

SNMPv1 define tanto una comunidad de solo lectura como una comunidad de lectura y escritura. La _comunidad de solo lectura (RO)_ permite obtener mensajes, y la _comunidad de lectura y escritura (RW)_ permite lecturas y escrituras (obtenciones y conjuntos). La Figura 12-12 muestra los conceptos. En los pasos 1 y 2, el agente se configura con cadenas de comunidad RO y RW particulares, y el NMS configura los valores coincidentes. En el paso 3, la obtención SNMP puede fluir con cualquiera de las comunidades, pero en el paso 4, la solicitud de configuración debe utilizar la comunidad RW.

![](img/12.12.png)

SNMPv2 y la versión 2 de SNMP basada en la comunidad (SNMPv2c) relacionada agregaron un cambio en el nombre, pero básicamente mantuvieron la misma característica de seguridad de la comunidad que SNMPv1 una vez que se completó el proceso de estándares. Las especificaciones originales para SNMPv2 no incluían comunidades SNMPv1; sin embargo, el mercado todavía quería comunidades, por lo que un RFC adicional agregó el mecanismo de comunidades SNMPv1 a SNMPv2. Este RFC actualizado, “SNMPv2 basado en la comunidad”, pasó a conocerse simplemente como SNMPv2c. Los proveedores (incluido Cisco) implementaron SNMPv2c; sin embargo, la seguridad todavía era relativamente débil.

SNMPv3 llegó con mucha celebración entre los administradores de red. Por fin había llegado la seguridad con el potente protocolo de gestión de red. SNMPv3 elimina las comunidades y las reemplaza con las siguientes características:

- **Integridad del mensaje:** Este mecanismo, aplicado a todos los mensajes SNMPv3, confirma si cada mensaje ha sido modificado o no durante el tránsito.
- **Autenticación:** Esta característica opcional agrega autenticación con un nombre de usuario y contraseña, y la contraseña nunca se envía como texto sin cifrar. En cambio, utiliza un método de hash como muchos otros procesos de autenticación modernos.
- **Cifrado (privacidad):** Esta característica opcional cifra el contenido de los mensajes SNMPv3 para que los atacantes que intercepten los mensajes no puedan leer su contenido.
### FTP and TFTP
Esta final de tres secciones principales del capítulo se centra en dos temas: Protocolo de transferencia de archivos (FTP) y Protocolo trivial de transferencia de archivos (TFTP). Ambos existen como protocolos TCP/IP definidos en RFC. Ambos utilizan un modelo de cliente y servidor, en el que el cliente se conecta a un servidor y luego el cliente puede copiar archivos al servidor o desde el servidor. Ambos existen como una gran cantidad de implementaciones de código de cliente y de servidor, desde clientes de línea de comandos hasta aplicaciones con interfaces gráficas, utilizando los respectivos protocolos FTP o TFTP detrás de escena.

Esta sección analiza FTP y TFTP con dos ramas. La primera sección ofrece una visión práctica del uso más común de TFTP y FTP por parte de los ingenieros de redes mientras trabajan: la tarea de actualizar imágenes de IOS. El proceso puede utilizar TFTP y FTP, por lo que esta sección proporciona los conceptos básicos. La segunda rama de esta sección principal final pasa a hablar sobre FTP y TFTP en un sentido mucho más amplio, con detalles sobre cada protocolo, sus capacidades y las capacidades que cada uno proporciona a cualquier usuario.
### Gestión de imágenes de Cisco IOS con FTP/TFTP

IOS existe como un archivo (un único archivo) que luego el enrutador carga en la RAM para usarlo como sistema operativo. Para comprender mejor el proceso, debes comprender algunos detalles más sobre cómo funciona IOS. En particular, es necesario comprender el sistema de archivos IO (IFS), que define cómo IOS almacena los archivos (incluido el archivo IOS). El proceso de actualización de la imagen de IOS se produce copiando nuevos archivos de IOS en el enrutador y luego iniciando el enrutador con ese nuevo IOS.
### El sistema de archivos IOS
Cada sistema operativo crea sistemas de archivos para almacenar archivos. Una computadora necesita algún tipo de almacenamiento permanente, pero necesita algo más que un lugar para almacenar bytes. El sistema operativo organiza el almacenamiento en un sistema de archivos, que incluye directorios, estructura y nombres de archivos, con las reglas asociadas. Al utilizar un sistema de archivos, el sistema operativo puede mantener los datos organizados para que el usuario y las aplicaciones puedan encontrarlos más tarde.

Cada sistema operativo define sus propias convenciones de sistema de archivos. Los sistemas operativos Windows, por ejemplo, utilizan una barra inclinada hacia la izquierda (\) en estructuras de directorios, como \Desktop\Applications. Linux y macOS utilizan una barra inclinada hacia la derecha, por ejemplo, /Desktop. Cada sistema operativo también se refiere a los discos físicos de manera ligeramente diferente, y IOS no es diferente.

En cuanto al almacenamiento físico, los enrutadores Cisco suelen utilizar memoria flash, sin unidad de disco duro. La memoria flash es un almacenamiento permanente regrabable, ideal para almacenar archivos que deben conservarse cuando el enrutador se queda sin energía. Cisco utiliza deliberadamente memoria flash en lugar de unidades de disco duro en sus productos porque no hay partes móviles en la memoria flash, por lo que hay menos posibilidades de falla en comparación con las unidades de disco. Algunos enrutadores tienen memoria flash en la placa base. Otros tienen ranuras para memoria flash que permiten extraer y reemplazar fácilmente la tarjeta flash, pero con la intención de que la tarjeta permanezca en el dispositivo la mayor parte del tiempo. Además, muchos dispositivos tienen ranuras USB que admiten unidades flash USB.

Para cada dispositivo de memoria física en el enrutador, IOS crea un sistema de archivos IOS simple y le da un nombre a ese dispositivo. El ejemplo 12-1 muestra una lista sorprendentemente larga de sistemas de archivos IOS. Tenga en cuenta que las entradas de tipo _disk_ y _usbflash_ son los dispositivos de almacenamiento físico en ese enrutador.

En este caso, el enrutador tiene una de las dos ranuras flash compactas del 2901 con una tarjeta flash de 256 MB y una de las dos ranuras flash USB con una unidad flash USB de 8 GB. Mire la columna de tamaño y la columna de prefijos en el resultado para encontrar estos dispositivos, según sus tipos como _disco_ y _usbflash_.

```
R2# show file systems 
File Systems:

			Size(b)        Free(b)       Type   Flags   Prefixes
			  -               -        opaque      rw    archive:
			  -               -        opaque      rw     system:
			  -               -        opaque      rw     tmpsys:
			  -               -        opaque      rw       null: 
			                  -       network      rw       tftp:
*        256487424        49238016       disk      rw    flash0: flash:#
              -               -          disk      rw     flash1:
            262136         253220       nvram      rw      nvram:
			  -               -        opaque      wo     syslog:
			  -               -        opaque      rw     xmodem:
			  -               -        opaque      rw     ymodem:
			  -               -       network      rw        rcp:
			  -               -       network      rw       pram:
			  -               -       network      rw       http:
			  -               -       network      rw        ftp:
			  -               -       network      rw        scp:
			  -               -        opaque      ro        tar:
			  -               -       network      rw      https:
			  -               -        opaque      ro        cns:
	      7794737152     7483719680  usbflash      rw   usbflash0:
74503236 bytes copied in 187.876 secs (396555 bytes/sec)
```

En este caso, el ejemplo enumera 20 sistemas de archivos IOS diferentes, pero el enrutador no tiene 20 dispositivos de almacenamiento físico diferentes. En cambio, IOS también utiliza estos sistemas de archivos para otros fines, con estos tipos:

- **Opaque:**  Para representar sistemas de archivos internos lógicos para facilitar las funciones y comandos internos.
- **Network:** para representar sistemas de archivos externos que se encuentran en diferentes tipos de servidores para facilitar la referencia en diferentes comandos de IOS.
- **Disk:** Para flash
- **Flash USB:**  Para flash USB
- **NVRAM:** Un tipo especial para la memoria NVRAM, la ubicación predeterminada del archivo de configuración de inicio

Muchos comandos de IOS hacen referencia a archivos en un IFS, pero sólo algunos comandos hacen referencia directamente a los archivos por sus nombres formales. Los nombres formales utilizan el prefijo como se ve en la columna del extremo derecho del ejemplo 12-1. Por ejemplo, el comando `more flash0:/wotemp/fred` mostraría el contenido del archivo `fred` en el directorio `/wotemp` en la primera ranura de memoria flash del enrutador. (El comando `more` muestra el contenido de un archivo). Sin embargo, muchos comandos usan una palabra clave que se refiere indirectamente a un nombre de archivo formal, para reducir la escritura. Por ejemplo:

- Comando **show running-config**: Se refiere al sistema de `archivos:running-config`
- Comando **show startup-config**: Se refiere al archivo `nvram:startup-config`
- Comando **show flash**: Se refiere al flash IFS predeterminado (generalmente flash0:)
### Actualización de imágenes de IOS
Uno de los primeros pasos para actualizar el IOS de un enrutador a una nueva versión es obtener la nueva imagen del IOS y colocarla en la ubicación correcta. Normalmente, los enrutadores Cisco tienen su IOS en uno de los sistemas de archivos físicos locales, generalmente en memoria flash permanente. El único requisito es que el IOS esté en algún sistema de archivos accesible, incluso si el archivo se encuentra en un servidor externo y el dispositivo carga el sistema operativo a través de la red. Sin embargo, la mejor práctica es almacenar el archivo IOS de cada dispositivo en una memoria flash que permanecerá en el dispositivo de forma permanente.

La Figura 12-13 ilustra el proceso para actualizar una imagen de IOS a la memoria flash mediante los siguientes pasos:

| **Step 1.** | Obtenga la imagen de IOS de Cisco, generalmente descargándola de [Cisco.com](http://Cisco.com/) mediante HTTP o FTP.                                                                                                                                      |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Step 2.** | Coloque la imagen de IOS en algún lugar al que pueda llegar el enrutador. Las ubicaciones incluyen servidores TFTP o FTP en la red o una unidad flash USB que luego se inserta en el enrutador.                                                           |
| **Step 3.** | Emita el comando **copiar** desde el enrutador, copiando el archivo en la memoria flash que generalmente permanece en el enrutador de forma permanente. (Los enrutadores generalmente no pueden arrancar desde la imagen de IOS en una unidad flash USB). |

![](img/12.13.png)

El Ejemplo 12-2 proporciona un ejemplo del paso 3 de la Figura 12-13, copiando la imagen del IOS en la memoria flash. En este caso, el enrutador R2, un 2901, copia una imagen de IOS desde un servidor TFTP en la dirección IP 2.2.2.1.

```
R2# copy tftp flash
Address or name of remote host []? 2.2.2.1
Source filename []? c2900-universalk9-mz.SPA.152-4.M1.bin 
Destination filename [c2900-universalk9-mz.SPA.152-4.M1.bin ]?
Accessing tftp://2.2.2.1/c2900-universalk9-mz.SPA.152-4.M1.bin ...
Loading c2900-universalk9-mz.SPA.152-4.M1.bin from 2.2.2.1 (via GigabitEthernet0/1):
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
[OK - 97794040 bytes]

97794040 bytes copied in 187.876 secs (396555 bytes/sec)

R2#
```

El comando **copiar** realiza una tarea sencilla (copiar un archivo), pero también tiene varios elementos pequeños que comprobar. Necesita algunos datos del usuario, por lo que el comando le solicita esa información mostrándole algo de texto y esperando su entrada. Los elementos del ejemplo muestran la entrada del usuario. Luego, el enrutador debe verificar que la copia funcione. El comando funciona a través de este tipo de preguntas:

**1.** ¿Cuál es la dirección IP o el nombre de host del servidor TFTP?
**2.** ¿Cuál es el nombre del archivo?
**3.** Pídale al servidor que conozca el tamaño del archivo y luego verifique la memoria flash del enrutador local para preguntar si hay suficiente espacio disponible para este archivo en la memoria flash.
**4.** ¿El servidor realmente tiene un archivo con ese nombre?
**5.** ¿Quieres que el enrutador borre los archivos antiguos en flash?

El enrutador le solicitará respuestas a algunas de estas preguntas, según sea necesario. Para cada pregunta, debe escribir una respuesta o presionar **Entrar** si la respuesta predeterminada (que se muestra entre corchetes al final de la pregunta) es aceptable. Luego, el enrutador borra la memoria flash si se le indica, copia el archivo y luego verifica que la suma de verificación del archivo muestre que no se produjeron errores en la transmisión.

Puede ver el contenido del sistema de archivos flash para ver el archivo IOS que acaba de copiar usando un par de comandos. El comando  `show flash` muestra los archivos en el sistema de archivos flash predeterminado (flash0:), como se ve en la parte superior del Ejemplo 12-3. Debajo, el comando más general `dir flash0:` enumera el contenido de ese mismo sistema de archivos, con información similar. (Puede utilizar el comando `dir` para mostrar el contenido de cualquier IFS local).

```
R4# show flash

-#- --length-- -----date/time------ path 
1    104193476  Jul 21 2015 13:38:06 +00:00  c2900-universalk9-mz.SPA.154-3.M3.bin
3      3000320  Jul 10 2012 00:05:44 +00:00  cpexpress.tar
4         1038  Jul 10 2012 00:05:52 +00:00  home.shtml
5       122880  Jul 10 2012 00:06:02 +00:00  home.tar
6      1697952  Jul 10 2012 00:06:16 +00:00  securedesktop-ios-3.1.1.45-k9.pkg
7       415956  Jul 10 2012 00:06:28 +00:00  sslclient-win-1.1.4.176.pkg
8         1153  Aug 16 2012 18:20:56 +00:00  wo-lic-1
9     97794040  Oct 10 2014 21:06:38 +00:00  c2900-universalk9-mz.SPA.152-4.M1.bin

49238016 bytes available (207249408 bytes used)

R4# dir flash0:
Directory of flash0:/

   1  -rw-   104193476  Jul 21 2015 13:38:06 +00:00  c2900-universalk9-mz.SPA.154-3.
M3.bin
   3         -rw-     3000320  Jul 10 2012 00:05:44 +00:00  cpexpress.ta
   4         -rw-        1038  Jul 10 2012 00:05:52 +00:00  home.shtml
   5         -rw-      122880  Jul 10 2012 00:06:02 +00:00  home.tar
   6         -rw-     1697952  Jul 10 2012 00:06:16 +00:00  securedesktop-ios-3.1.1.45-k9. pkg
   7         -rw-      415956  Jul 10 2012 00:06:28 +00:00  sslclient-win-1.1.4.176.pkg
   8         -rw-        1153  Aug 16 2012 18:20:56 +00:00  wo-lic-1
   9         -rw-    97794040  Oct 10 2014 21:06:38 +00:00  c2900-universalk9-mz.SPA.152-4.M1.bin

256487424 bytes total (49238016 bytes free)
```

Preste mucha atención al uso de memoria por archivo y para IFS como se muestra en el ejemplo. El resultado enumera el tamaño en bytes de cada archivo. Tenga en cuenta que el archivo IOS tiene aproximadamente 104 MB.

Tenga en cuenta que el tamaño del archivo IOS coincide con el tamaño mostrado anteriormente en la transferencia TFTP en el Ejemplo 12-2. El final de cada uno de los comandos luego enumera la cantidad de espacio disponible para agregar nuevos archivos a la memoria flash (uno lo enumera como "bytes disponibles"; el otro como "bytes libres"). Sin embargo, esa misma línea final de cada comando muestra información ligeramente diferente sobre el uso: `show flash` enumera los bytes utilizados, mientras que el comando `dir` enumera el total de bytes (bytes utilizados más bytes libres). Experimente con los números de este ejemplo para asegurarse de saber qué comando enumera qué total en particular.
### Verificación de la integridad del código IOS con MD5
Descarga el IOS de Cisco, lo copia a su enrutador y lo ejecuta. ¿Es realmente el código de Cisco? ¿O algún atacante infame de alguna manera consiguió que descargaras un IOS falso que tiene un virus?

Cisco proporciona un medio para verificar la integridad del archivo IOS para evitar este tipo de problema. La Figura 12-14 muestra la mecánica básica del proceso. Primero, cuando Cisco crea una nueva imagen de IOS, calcula y publica un valor hash MD5 para ese archivo de IOS específico. Es decir, Cisco utiliza como entrada el propio archivo IOS, ejecuta el algoritmo matemático MD5 contra ese archivo, produciendo un código hexadecimal. Cisco coloca ese código en el sitio de descarga para que todos lo vean. Luego, ejecuta los mismos cálculos MD5 en su enrutador contra el archivo IOS en el enrutador, usando el comando IOS **verify**. Ese comando enumerará el hash MD5 recalculado en su enrutador. Si ambos hash MD5 son iguales, el archivo no ha cambiado.

![](img/12.14.png)

El comando  `verify /md5`  genera el hash MD5 en su enrutador, como se muestra en el ejemplo 12-4. Tenga en cuenta que puede incluir el valor hash calculado por Cisco como último parámetro (como se muestra en el ejemplo) o dejarlo fuera. Si lo incluye, IOS le dirá si el valor calculado localmente coincide con lo que copió en el comando. Si lo omite, el comando `verify` enumera el hash MD5 calculado localmente y usted mismo debe realizar la verificación exigente de los valores carácter por carácter.

```
R2#  verify /md5 flash0:c2900-universalk9-mz.SPA.154-3.M3.bin a79e325e6c498b70829d4d b0afba5041** .....................................................................................
.....................................................................................
…..MD5 of flash0:c2900-universalk9-mz.SPA.154-3.M3.bin Done!

Verified (flash0:c2900-universalk9-mz.SPA.154-3.M3.bin) = a79e325e6c498b70829d4d b0afba5041
```
### Copiar imágenes con FTP
El mundo de las redes tiene muchas opciones para la transferencia de archivos, varias de las cuales IOS admite para la transferencia de archivos dentro y fuera de los sistemas de archivos IOS que residen en el enrutador. TFTP y FTP han sido compatibles durante más tiempo, y más recientemente se agregó soporte para protocolos como el Protocolo de copia segura (SCP), que utiliza el Protocolo de transferencia de archivos SSH (SFTP). La Tabla 12-3 enumera algunos de los nombres de protocolos de transferencia de archivos que puede encontrar al trabajar con enrutadores.

| **Method** | **Method (Full Name)**         | **Encrypted?** |
| ---------- | ------------------------------ | -------------- |
| TFTP       | Trivial File Transfer Protocol | No             |
| FTP        | File Transfer Protocol         | No             |
| SCP        | Secure Copy Protocol           | Yes            |

Para copiar archivos con FTP, sigue el mismo tipo de proceso que usa con TFTP (consulte el Ejemplo 12-5). Puedes seguir las indicaciones interactivas después de usar un comando EXEC como  `copy ftp flash`. Sin embargo, el comando `copy` te permite usar un URI para el origen y/o el destino, lo que te permite colocar la mayor parte o toda la información en la propia línea de comando. Cada URI hace referencia al nombre formal de un archivo en el IFS.

```
R1# copy ftp://wendell:odom@192.168.1.170/c2900-universalk9-mz.SPA.155-2.T1.bin flash Destination filename [c2900-universalk9-mz.SPA.155-2.T1.bin]?
Accessing ftp://192.168.1.170/c2900-universalk9-mz.SPA.155-2.T1.bin...
Loading c2900-universalk9-mz.SPA.155-2.T1.bin!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
[OK - 107410736/4096 bytes]

107410736 bytes copied in 119.604 secs (898053 bytes/sec)
```

Primero, observe de cerca el URI largo del comando que comienza con "ftp". La parte “ftp” identifica el protocolo, por supuesto. Después de //, el texto hace referencia al nombre de usuario (wendell) y la contraseña (odom), así como a la dirección IP del servidor FTP. Después del sencillo / viene el nombre del archivo en el servidor.

Aunque el comando es largo, solo tiene dos parámetros, con el primer parámetro largo y la palabra clave corta `flash` como segundo parámetro. El comando `copy` enumera la ubicación de origen como primer parámetro y el destino como segundo. El destino en este caso, `flash`, es una palabra clave que hace referencia al flash predeterminado, normalmente `flash0:`, pero no identifica un nombre de archivo específico. Como resultado, IOS solicita al usuario un nombre de archivo de destino específico, con un valor predeterminado (entre paréntesis) para mantener el nombre del archivo de origen. En este caso, el usuario simplemente presionó Enter para aceptar el valor predeterminado. Para evitar que se le solicite, el comando podría haber incluido `flash:c2900-universalk9-mz.SPA.155-2.T1.bin` como ese segundo parámetro, definiendo completamente el archivo de destino.

Finalmente, con otro giro, puede configurar el nombre de usuario y la contraseña de FTP en el enrutador para no tener que incluirlos en el comando `copy`. Por ejemplo, los comandos de configuración global `ip ftp username wendell` y `ip ftp password odom` habrían configurado esos valores. Entonces, el comando `copy` habría comenzado con `copy ftp://192.168.1.170/...`, omitiendo el `username:password` en el comando, sin necesidad de solicitar luego al usuario el nombre de usuario y la contraseña.

Esto completa los ejemplos que muestran cómo copiar archivos IOS en un enrutador usando TFTP y FTP. Los temas del examen mencionan TFTP y FTP, pero no el proceso de actualización de IOS, por lo que el texto ahora deja de lado el proceso de actualización de IOS para centrarse más en TFTP y FTP. Sin embargo, hay algunos pasos más que completar para actualizar IOS, como configurar el comando de inicio del sistema y recargar el enrutador. 

Sin embargo, para completar el proceso de actualización de IOS, debe realizar algunos pasos más.
### Los protocolos FTP y TFTP
El comando `copy` de IOS, cuando se utiliza la palabra clave **tftp** o **ftp**, hace que el comando actúe como un cliente. El cliente se conecta a un servidor TFTP o FTP y luego intenta transferir el archivo. En los ejemplos del IOS, ese comando `copy` copió el archivo del servidor al dispositivo cliente (un enrutador).

El resto de esta sección examina lo que sucede detrás de escena en ese proceso, con una mirada más cercana a FTP y TFTP como protocolos y herramientas.
### Conceptos básicos del protocolo FTP
FTP ha sido durante mucho tiempo un protocolo central de Internet y ha servido como protocolo principal de transferencia de archivos durante varias décadas. RFC 959, que estandariza FTP, se remonta a 1985. FTP utiliza TCP como protocolo de transporte, confiando en TCP para proporcionar una entrega de datos en orden y sin errores, de modo que la aplicación FTP sepa que cada transferencia de archivo crea una copia exacta de el expediente sin omisiones. FTP utiliza el conocido puerto TCP 21 y, en algunos casos, también el conocido puerto 20.

En cuanto al funcionamiento normal, FTP utiliza un modelo cliente/servidor para la transferencia de archivos, como se muestra en el ejemplo de la Figura 12-15. La figura muestra los pasos principales, pero no todos los mensajes. Por ejemplo, el paso 1 muestra al host A creando una conexión TCP con el servidor (que recibe los tres mensajes TCP habituales). El paso 2 representa el intercambio que permite al servidor autenticar al cliente. El paso 3 muestra la idea de que, una vez autenticados, el cliente y el servidor pueden enviar comandos FTP a través de la conexión para indicarle al otro dispositivo qué hacer.

![](img/12.15.png)

Los comandos que fluyen a través de esta conexión TCP inicial, denominada conexión de control FTP, definen los tipos de funciones admitidas por FTP. Esos comandos permiten al cliente navegar por las estructuras de directorios del servidor, enumerar archivos y luego transferir archivos desde el servidor (FTP GET) o al servidor (FTP PUT). A continuación se muestra un resumen de algunas de las acciones de FTP:

- **Navegar por directorios:** enumera el directorio actual, cambia el directorio actual a un nuevo directorio, vuelve al directorio de inicio, todo tanto en el lado del servidor como del cliente de la conexión.
- **Agregar/eliminar directorios:** Cree nuevos directorios y elimine los directorios existentes tanto en el cliente como en el servidor.
- **Listar archivos:** Listar archivos tanto en el cliente como en el servidor.
- **Transferencia de archivos:** Obtener (el cliente obtiene una copia del archivo del servidor), Poner (el cliente toma un archivo que existe en el cliente y coloca una copia del servidor FTP).

Si bien muchos sistemas operativos admiten clientes FTP de línea de comandos, que requieren que usted aprenda los distintos comandos FTP y los use desde la línea de comandos, la mayoría de los usuarios usan una aplicación de cliente FTP que emite los comandos FTP detrás de escena. Los clientes suelen mostrar archivos en el sistema local y en el servidor con una interfaz de usuario similar a un explorador de archivos típico en un sistema operativo de escritorio (por ejemplo, Windows Explorer, macOS Finder). La Figura 12-16 muestra una interfaz de usuario de muestra del cliente FTP Filezilla [(Filezilla-project.org).](http://Filezilla-project.org/)

![](img/12.16.png)

La aplicación cliente en la Figura 12-16 enumera el sistema de archivos local de la computadora cliente a la izquierda y el sistema de archivos del servidor FTP a la derecha. El usuario puede hacer clic en el botón derecho para cambiar de directorio, como si utilizara cualquier aplicación que explora un sistema de archivos, con FTP ejecutando los comandos detrás de escena. El usuario también puede arrastrar y soltar archivos de izquierda a derecha para colocar un archivo en el servidor, o viceversa para obtener un archivo del servidor.

El servidor FTP puede ser una aplicación de servidor instalada y administrada por otros, o usted puede instalar o habilitar un servidor FTP para su propio uso. Por ejemplo, un ingeniero de redes podría instalar una aplicación de servidor FTP en su computadora portátil para usarla en la actualización de archivos IOS, mientras que el personal de TI puede mantener un servidor FTP disponible las 24 horas del día, los 7 días de la semana para que lo utilicen todos los empleados de la empresa. Una simple búsqueda en Internet puede mostrar una variedad de aplicaciones de servidor FTP que se ejecutan en los sistemas operativos de escritorio comunes. Además, tanto Windows 10 como macOS vienen con una opción de servidor FTP o FTPS (FTP Secure) integrada en el sistema operativo; todo lo que tienes que hacer es habilitarlo. (Todas las distribuciones de Linux tienen servidores FTP disponibles mediante descargas simples).

Una vez instalado, el servidor se puede configurar con una variedad de configuraciones. Por ejemplo, el servidor necesita especificar qué usuarios pueden acceder al servidor, de modo que pueda usar las mismas credenciales de inicio de sesión permitidas para el host donde reside o especificar otras credenciales. Puede especificar los directorios a los que puede acceder cada usuario y si el usuario tiene acceso de solo lectura o de lectura y escritura.
### Modos FTP activo y pasivo
FTP puede funcionar en modo activo o pasivo. La elección del modo puede afectar si el cliente TCP puede o no conectarse al servidor y realizar funciones normales. El usuario del cliente FTP puede elegir qué modo usar, por lo que esta sección analiza los detalles subyacentes para explicar por qué el modo pasivo FTP puede ser la opción más probable para funcionar.

Primero, tenga en cuenta que FTP utiliza dos tipos de conexiones TCP:
- **Conexión de control:** Se utiliza para intercambiar comandos FTP
- **Conexión de datos:** Se utiliza para enviar y recibir datos, tanto para transferencias de archivos como para salida para mostrar a un usuario.

Dadas las dos funciones, cuando un cliente se conecta a un servidor FTP, el cliente primero crea la conexión de control FTP como se muestra en la Figura 12-17. El servidor escucha nuevas conexiones de control en su conocido puerto 21; el cliente asigna cualquier puerto dinámico nuevo (49222 en este caso) y crea una conexión TCP con el servidor.

![](img/12.17.png)

Después de crear la conexión TCP, el usuario se autentica en el servidor FTP y realiza algunas acciones. Algunas de esas acciones requieren solo la conexión de control, pero eventualmente el usuario realizará una acción (como obtener un archivo) que requiere una conexión de datos. Cuando eso sucede, para crear la conexión de datos FTP, el cliente utilizará el modo activo o el modo pasivo, como se muestra en los dos ejemplos siguientes.

La Figura 12-18 muestra un ejemplo de lo que sucede en modo activo. Siguiendo los pasos de la figura:

**1.** El cliente FTP asigna un puerto dinámico no utilizado actualmente y comienza a escuchar en ese puerto.
**2.** El cliente identifica ese puerto (y su dirección IP) en el servidor FTP enviando un comando FTP **PORT** al servidor.
**3.** El servidor, debido a que también opera en modo activo, espera el comando **PORT**; el servidor reacciona e inicia la conexión de datos FTP a la dirección del cliente (192.168.1.102) y al puerto (49333).

![](img/12.18.png)

El modo activo funciona bien tanto con el cliente como con el servidor FTP ubicados dentro de la misma red empresarial. Cuando se encuentra dentro de la misma red, normalmente no hay ninguna función NAT ni ningún firewall entre los dos. Sin embargo, si el cliente FTP se encuentra en una red empresarial y el servidor FTP reside en algún lugar de Internet, una conexión en modo activo normalmente falla. La mayoría de los firewalls no permiten que los hosts basados ​​en Internet inicien conexiones TCP a hosts dentro de la empresa sin una regla de firewall específica que permita conexiones a un puerto conocido y, en este caso, el cliente FTP asigna cualquier número de puerto disponible. Por ejemplo, en la Figura 12-18, un firewall descartaría la conexión TCP (paso 3).

El modo pasivo ayuda a resolver las restricciones del firewall al hacer que el cliente FTP inicie la conexión de datos FTP al servidor. Sin embargo, el modo pasivo no hace que el cliente FTP simplemente se conecte a un puerto conocido en el servidor; requiere más intercambios de números de puerto para usar entre el servidor y el cliente, como se muestra en la Figura 12-19, con estos pasos:

**1.** El cliente FTP cambia para usar el modo pasivo FTP, notificando al servidor mediante el comando `FTP PASV`.
**2.** El servidor elige un puerto para escuchar la próxima nueva conexión TCP, en este caso el puerto TCP 49444.
**3.** El FTP notifica al cliente FTP su dirección IP y el puerto elegido con el comando `FTP PORT`.
**4.** El cliente FTP abre la conexión de datos TCP a la dirección IP y al puerto aprendidos en el paso anterior.

![](img/12.19.png)
### FTP sobre TLS (FTP seguro)
FTP, definido en RFC 959 en 1985, tiene algunas deficiencias en materia de seguridad. Tal como se definió originalmente, incluye la capacidad de utilizar nombres de usuario y contraseñas para autenticación y autorización; sin embargo, el nombre de usuario/contraseña fluye como texto sin cifrar. Además, todas las transferencias de datos fluyen como texto claro.

A lo largo de los años, varios RFC definieron mejoras de seguridad para FTP. Esas nuevas características incluyen el uso de certificados digitales para la autenticación, así como el uso de Transport Layer Security (TLS) para cifrar todos los datos (incluidos nombres de usuarios y contraseñas). Avance rápido hasta el día de hoy y muchas de esas características convergen en lo que la mayoría de los clientes y servidores FTP admiten como FTP sobre TLS o FTP seguro (FTPS).

Con FTPS, el cliente y el servidor siguen utilizando comandos FTP y siguen utilizando una conexión de control y de datos. Sin embargo, FTPS cifra tanto las conexiones de control como las de datos con TLS, incluido el intercambio de nombres de usuario y contraseñas. FTPS incluye algunas variaciones, incluido el proceso de modo explícito de FTPS que se muestra en la Figura 12-20:

**1.** El cliente crea la conexión TCP de control FTP al puerto conocido 21 del servidor.
**2.** El cliente inicia el uso de TLS en la conexión de control con el comando `FTP AUTH`.
**3.** Cuando el usuario realiza una acción que requiere una conexión de datos FTP, el cliente crea una conexión TCP de datos FTP al puerto conocido 21 del servidor.
**4.** El cliente inicia el uso de TLS en la conexión de datos con el comando `FTP AUTH`.

![](img/12.20.png)

Por el contrario, el proceso en modo implícito comienza con una conexión TLS requerida, sin necesidad de un comando `FTP AUTH`, utilizando los puertos conocidos 990 (para la conexión de control) y 989 (para la conexión de datos).
### Conceptos básicos del protocolo TFTP
FTP tiene la función de herramienta general de transferencia de archivos para cualquier usuario, con una buena cantidad de opciones de aplicaciones cliente FTP disponibles. TFTP desempeña un papel mucho menor como herramienta para el usuario medio, pero desempeña un papel más útil para el personal de soporte de TI.

Para lo básico, el Protocolo Trivial de Transferencia de Archivos utiliza el conocido puerto 69 de UDP. Debido a que usa UDP, TFTP agrega una función para verificar cada archivo en busca de errores de transmisión mediante el uso de un proceso de suma de verificación en cada archivo una vez que se completa la transferencia.

La palabra _trivial_ en el nombre se refiere a su número relativamente pequeño de características, lo que pretende ser una ventaja al hacer que la herramienta sea liviana. Por ejemplo, admite muchos menos comandos que FTP (menos funciones), lo que significa que el código requiere menos espacio para su instalación, lo que puede resultar útil para dispositivos con memoria limitada. TFTP puede obtener y colocar archivos, pero no incluye comandos para cambiar directorios, crear/eliminar directorios o incluso enumerar archivos en el servidor. TFTP no admite ni siquiera una simple autenticación de texto sin cifrar. De hecho, si se está ejecutando un servidor TFTP, debería aceptar solicitudes de cualquier cliente TFTP.

Idealmente, TFTP tiene su mejor uso como herramienta temporal para transferencias rápidas de archivos en un entorno controlado, particularmente cuando los datos en sí no tienen que estar seguros. Por ejemplo, imagine este escenario:

**1.** Un ingeniero de redes guarda todas las imágenes de IOS de enrutadores y conmutadores en una carpeta.
**2.** La ingeniera habilita un servidor TFTP en su computadora portátil según sea necesario; de lo contrario, el servidor TFTP permanece deshabilitado.
**3.** La ingeniera conecta su computadora portátil a una LAN y habilita el servidor TFTP el tiempo suficiente para transferir imágenes de IOS hacia o desde algunos dispositivos.
**4.** Si el ingeniero olvida desactivar TFTP, el único riesgo es que alguien pueda copiar un IOS
### Referencias de comandos

| **Command**                                                     | **Description**                                                                                                         |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **boot system flash** [_flash-fs_:] [_filename_]                | Global command that identifies the location of an IOS image in flash memory                                             |
| **boot system** {**tftp** \| **ftp**} _filename_ [_ip-address_] | Global command that identifies an external server, protocol, and filename to use to load an IOS from an external server |
| **ip ftp username** _name_                                      | Global command to define the username used when referencing the **ftp:** IOS file system but not supplying a username   |
| **ip ftp password** _pass_                                      | Global command to define the password used when referencing the **ftp:** IOS file system but not supplying a password   |

| **copy** _from-location to-location_                               | Enable mode EXEC command that copies files from one file location to another. Locations include the startup-config and running-config files, files on TFTP and RPC servers, and flash memory. |
| ------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show flash**                                                     | Lists the names and size of the files in flash memory, and notes the amount of flash memory consumed and available.                                                                           |
| **dir** _filesystem:_<br><br>**dir**<br><br>_filesystem:directory_ | Lists the files in the referenced file system or file system directory.                                                                                                                       |
| **verify /md5** _filesystem:name [MD5-hash]_                       | Performs an MD5 hash of the referenced file and displays the results. If listed, the command compares the MD5 hash in the command with the results of performing MD5 on the local file.       |
