## ¿Qué aprenderé en este módulo?

**Características de la capa de red**

La capa de red (Capa OSI 3) proporciona servicios para permitir que los dispositivos finales intercambien datos a través de las redes. IPv4 e IPv6 son los principales protocolos de comunicación de la capa de red. La capa de red también incluye el protocolo de enrutamiento OSPF y protocolos de mensajería como ICMP. Los protocolos de capa de red realizan cuatro operaciones básicas: direccionamiento de dispositivos finales, encapsulación, enrutamiento y desencapsulación. IPv4 e IPv6 especifican la estructura de paquetes y el procesamiento utilizado para transportar los datos de un host a otro. IP encapsula el segmento de la capa de transporte agregando un encabezado IP, que se utiliza para entregar el paquete al host de destino. El encabezado IP es examinado por los dispositivos de Capa 3 (es decir, routers) a medida que viaja a través de una red a su destino. Las características de la IP son que es sin conexión, el mejor esfuerzo e independiente de los medios de comunicación. IP no tiene conexión, lo que significa que IP no crea una conexión de extremo a extremo dedicada antes de enviar los datos. El protocolo IP no garantiza que todos los paquetes que se envían, de hecho, se reciban. Esta es la definición de la característica poco confiable, o mejor esfuerzo. IP funciona independientemente de los medios que transportan los datos en las capas más bajas de la pila de protocolos.

**Paquete IPv4**

Un encabezado de paquete IPv4 consta de campos que contienen información sobre el paquete. Estos campos tienen números binarios que examinan el proceso de capa 3. Los valores binarios de cada campo identifican diversos parámetros de configuración del paquete IP. Los campos significativos del encabezado IPv6 incluyen: versión, DS, suma de comprobación de encabezado, TTL, protocolo y direcciones IPv4 de origen y destino.

**Paquete IPv6**

IPv6 está diseñado para superar las limitaciones de IPv4, entre ellas: agotamiento de direcciones IPv4, falta de conectividad de extremo a extremo y mayor complejidad de la red. IPv6 aumenta el espacio de direcciones disponible, mejora el manejo de paquetes y elimina la necesidad de NAT. Los campos en el encabezado del paquete IPv6 incluyen: versión, clase de tráfico, etiqueta de flujo, longitud de la carga útil, siguiente encabezado, límite de salto y las direcciones IPv6 de origen y destino.

**Cómo arma las rutas un host**

Un host puede enviar un paquete a sí mismo, a otro host local y a un host remoto. En IPv4, el dispositivo de origen utiliza su propia máscara de subred junto con su propia dirección IPv4 y la dirección IPv4 de destino para determinar si el host de destino está en la misma red. En IPv6, el router local anuncia la dirección de red local (prefijo) a todos los dispositivos de la red, para realizar esta determinación. La puerta de enlace predeterminada es el dispositivo de red (es decir, el router) que puede enrutar el tráfico a otras redes. En una red, una puerta de enlace predeterminada suele ser un router que tiene una dirección IP local en el mismo rango de direcciones que otros hosts de la red local, puede aceptar datos en la red local y reenviar datos fuera de la red local, y enrutar el tráfico a otras redes. Una tabla de enrutamiento de host generalmente incluirá una puerta de enlace predeterminada. En IPv4, el host recibe la dirección IPv4 de la puerta de enlace predeterminada de forma dinámica a través de DHCP o se configura manualmente. En IPv6, el router anuncia la dirección de la puerta de enlace predeterminada o el host se puede configurar manualmente. En un host de Windows, el comando **route print** o **netstat -r** se puede usar para mostrar la tabla de enrutamiento del host.

**Introducción al enrutamiento**

Cuando un host envía un paquete a otro host, consulta su tabla de enrutamiento para determinar dónde enviar el paquete. Si el host de destino está en una red remota, el paquete se reenvía a la puerta de enlace predeterminada, que generalmente es el router local. ¿Qué sucede cuando llega un paquete a la interfaz de un enrutador? El router examina la dirección IP de destino del paquete y busca en su tabla de enrutamiento para determinar dónde reenviar el paquete. La tabla de enrutamiento contiene una lista de todas las direcciones de red conocidas (prefijos) y a dónde reenviar el paquete. Estas entradas se conocen como entradas de ruta o rutas. El router reenviará el paquete utilizando la mejor entrada de ruta que coincida (más larga). La tabla de enrutamiento de un router almacena tres tipos de entradas de ruta: redes conectadas directamente, redes remotas y una ruta predeterminada. Los routers aprenden sobre redes remotas de forma manual o dinámica utilizando un protocolo de enrutamiento dinámico. Las rutas estáticas son entradas de ruta que se configuran manualmente. Las rutas estáticas incluyen la dirección de red remota y la dirección IP del router de salto siguiente. OSPF y EIGRP son dos protocolos de enrutamiento dinámico. El comando EXEC mode **show ip route** privilegiado se utiliza para ver la tabla de enrutamiento IPv4 en un router Cisco IOS. Al principio de una tabla de enrutamiento IPv4 hay un código que se utiliza para identificar el tipo de ruta o cómo se aprendió la ruta. Las fuentes de ruta comunes (códigos) incluyen:

**L** - Dirección IP de interfaz local conectada directamente

**C** - Red conectada directamente

**S** - La ruta estática fue configurada manualmente por un administrador

**O** - Open Shortest Path First (OSPF)

**D** - Enhanced Interior Gateway Routing Protocol (EIGRP)