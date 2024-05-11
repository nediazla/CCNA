## ¿Qué aprenderé en este módulo?

**Las reglas**

Todos los métodos de comunicación tienen tres elementos en común: origen del mensaje (remitente), destino del mensaje (receptor) y canal. El envío de un mensaje se rige por reglas denominadas _protocols_. Los protocolos deben incluir: un remitente y receptor identificado, lenguaje y gramática comunes, velocidad y tiempo de entrega, y requisitos de confirmación o acuse de recibo. Los protocolos de red determinan la codificación, el formato, la encapsulación, el tamaño, la distribución y las opciones de entrega del mensaje. La codificación es el proceso mediante el cual la información se convierte en otra forma aceptable para la transmisión. La decodificación revierte este proceso para interpretar la idea. Los formatos de los mensajes dependen del tipo de mensaje y el canal que se utilice para entregar el mensaje. Sincronización: incluye el método de acceso, control del flujo y tiempo de espera de respuesta. Las opciones de entrega de mensajes incluyen unidifusión, multidifusión y difusión.

**Protocolos**

Los protocolos son implementados por dispositivos finales y dispositivos intermediarios en software, hardware o ambos. Un mensaje enviado a través de una red informática normalmente requiere el uso de varios protocolos, cada uno con sus propias funciones y formato. Cada protocolo de red tiene su propia función, formato y reglas para las comunicaciones. La familia de protocolos Ethernet incluye IP, TCP, HTTP y muchos más. Los protocolos protegen los datos para proporcionar autenticación, integridad de los datos y cifrado de datos: SSH, SSL y TLS. Los protocolos permiten a los routeres intercambiar información de ruta, comparar información de ruta y, a continuación, seleccionar la mejor ruta de acceso a la red de destino: OSPF y BGP. Los protocolos se utilizan para la detección automática de dispositivos o servicios: DHCP y DNS. Los equipos y dispositivos de red utilizan protocolos acordados que proporcionan las siguientes funciones: direccionamiento, confiabilidad, control de flujo, secuenciación, detección de errores e interfaz de aplicación.

**Suite de Protocolos**

Un grupo de protocolos interrelacionados que son necesarios para realizar una función de comunicación se denomina suite de protocolos. Una pila de protocolos muestra la forma en que los protocolos individuales se implementan dentro de una suite. Desde la década de 1970 ha habido varios conjuntos de protocolos diferentes, algunos desarrollados por una organización de estándares y otros desarrollados por varios proveedores. Los protocolos TCP/IP son específicos de las capas Aplicación, Transporte e Internet. TCP/IP es el conjunto de protocolos utilizado por las redes e Internet actuales. TCP/IP ofrece dos aspectos importantes a proveedores y fabricantes: conjunto de protocolos estándar abierto y conjunto de protocolos basado en estándares. El proceso de comunicación del conjunto de protocolos TCP/IP permite procesos tales como un servidor web encapsular y enviar una página web a un cliente, así como el cliente desencapsular la página web para mostrarla en un explorador web.

**Organizaciones de estandarización**

Los estándares abiertos fomentan la interoperabilidad, la competencia y la innovación. Las organizaciones de estandarización generalmente son organizaciones sin fines de lucro y neutrales en lo que respecta a proveedores, que se establecen para desarrollar y promover el concepto de estándares abiertos. Varias organizaciones tienen diferentes responsabilidades para promover y crear estándares para Internet, incluyendo: ISOC, IAB, IETF e IRTF. Las organizaciones de estándares que desarrollan y soportan TCP/IP incluyen: ICANN e IANA. Las organizaciones de estándares electrónicos y de comunicaciones incluyen: IEEE, EIA, TIA y ITU-T.

**Modelos de referencia**

Los dos modelos de referencia que se utilizan para describir las operaciones de red son OSI y TCP/IP. El modelo de referencia OSI tiene siete capas:

7 - Aplicación

6 - Presentación

5 - Sesión

4 - Transporte

3 - Red

2 - Enlacede datos

1 - Física

El modelo TCP/IP incluye cuatro capas.

4 - Aplicación

3 - Transporte

2 - Internet

1 - Acceso ala red

**Encapsulación de datos**

La segmentación de mensajes tiene dos beneficios principales.

- Al enviar partes individuales más pequeñas del origen al destino, se pueden intercalar muchas conversaciones diferentes en la red. Este proceso se denomina multiplexación.
- La segmentación puede aumentar la eficiencia de las comunicaciones de red. Si parte del mensaje no logra llegar al destino, solo deben retransmitirse las partes faltantes.

TCP es responsable de secuenciar los segmentos individuales. La manera que adopta una porción de datos en cualquier capa se denomina unidad de datos del protocolo (PDU). Durante el encapsulamiento, cada capa encapsula las PDU que recibe de la capa inferior de acuerdo con el protocolo que se utiliza. Cuando se envían mensajes en una red, el proceso de encapsulamiento opera desde las capas superiores hacia las capas inferiores. Este proceso se invierte en el host receptor, y se conoce como desencapsulamiento. El desencapsulamiento es el proceso que utilizan los dispositivos receptores para eliminar uno o más de los encabezados de protocolo. Los datos se desencapsulan mientras suben por la pila hacia la aplicación del usuario final.

**Acceso a los datos**

La capa de red y la capa de enlace de datos son responsables de enviar los datos desde el dispositivo de origen o emisor hasta el dispositivo de destino o receptor. Los protocolos de las dos capas contienen las direcciones de origen y de destino, pero sus direcciones tienen objetivos distintos.

- **Direcciones de origen y de destino de la capa de red**: son responsables de enviar el paquete IP desde el dispositivo de origen hasta el dispositivo final, ya sea en la misma red o a una red remota.
- **Direcciones de origen y de destino de la capa de enlace de datos**: son responsables de enviar la trama de enlace de datos desde una tarjeta de interfaz de red (NIC) a otra en la misma red.

Las direcciones de la capa de red, o direcciones IP, indican el origen y el destino final. Una dirección IP contiene dos partes: la parte de red (IPv4) o Prefijo (IPv6) y la parte de host (IPv4) o el ID de interfaz (IPv6). Cuando el emisor y el receptor del paquete IP están en la misma red, la trama de enlace de datos se envía directamente al dispositivo receptor. En una red Ethernet, las direcciones de enlace de datos se conocen como direcciones MAC de Ethernet. Cuando el emisor del paquete se encuentra en una red distinta de la del receptor, las direcciones IP de origen y de destino representan los hosts en redes diferentes. La trama de Ethernet se debe enviar a otro dispositivo conocido como router o gateway predeterminado.