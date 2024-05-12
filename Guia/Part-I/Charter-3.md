# Listas avanzadas de control de acceso IPv4

**En este capítulo se tratan los siguientes temas del examen:**

**Fundamentos de seguridad 5.0**

- Configurar y verificar las listas de control de acceso

Las ACL IPv4 son ACL estándar o extendidas, con ACL estándar que coinciden solo con la dirección IP de origen y que coinciden con una variedad de campos de encabezado de paquete. Al mismo tiempo, las ACL IP están numeradas o nombradas. La figura 3-1 muestra las categorías y las características principales de cada una de ellas tal como se presentaron en el capítulo anterior.

![](img/3.1.png)

Este capítulo analiza las otras tres categorías de ACL más allá de las ACL IP numeradas estándar y termina con algunas funciones misceláneas para proteger los routers y switches de Cisco.
### Listas de control de acceso IP numeradas ampliadas

Las listas de acceso IP extendidas tienen muchas similitudes en comparación con las ACL IP numeradas estándar discutidas en el capítulo anterior. Al igual que las ACL IP estándar, se habilitan listas de acceso extendidas en las interfaces para los paquetes que entran o salen de la interfaz. IOS busca en la lista secuencialmente. Las ACL extendidas también utilizan la lógica de primera coincidencia, ya que el router detiene la búsqueda a través de la lista tan pronto como se coincide con la primera instrucción, realizando la acción definida en la primera instrucción coincidente. Todas estas características también se aplican a las listas de acceso numeradas estándar (y a las ACL con nombre).

Las ACL extendidas difieren de las ACL estándar principalmente debido a la mayor variedad de campos de encabezado de paquete que se pueden usar para hacer coincidir un paquete. Una ACE extendida (instrucción ACL) puede examinar varias partes de los encabezados de paquete, lo que requiere que todos los parámetros coincidan correctamente para que coincidan con esa ACE. Esa poderosa lógica de coincidencia hace que las listas de acceso extendidas sean más útiles y más complejas que las ACL IP estándar.
### Coincidencia del protocolo, la IP de origen y la IP de destino

Al igual que las ACL de IP numeradas estándar, las ACL de IP numeradas extendidas también utilizan el  `comando global access-list`  . La sintaxis es idéntica, al menos hasta la  palabra clave `permit` o `deny`. En ese momento, el comando enumera los parámetros coincidentes, y estos difieren, por supuesto. En particular, el comando extended ACL `access-list` requiere tres parámetros coincidentes: el tipo de protocolo IP, la dirección IP de origen y la dirección IP de destino.

El campo Protocolo del encabezado IP identifica el encabezado que sigue al encabezado IP. La Figura 3-2 muestra la ubicación del campo Protocolo IP, el concepto del mismo apunta al tipo de encabezado que sigue, junto con algunos detalles del encabezado IP como referencia.

![](img/3.2.png)

IOS requiere que configure los parámetros para las tres partes resaltadas de la Figura 3-2. Para el tipo de protocolo, simplemente use una palabra clave, como **tcp**, **udp** o **icmp**, que coincida con los paquetes IP que tienen un encabezado TCP, UDP o ICMP, respectivamente, después del encabezado IP. O puede usar la palabra clave **ip**, que significa "todos los paquetes IPv4". También debe configurar algunos valores para los campos de dirección IP de origen y destino que siguen; estos campos utilizan la misma sintaxis y las mismas opciones para hacer coincidir las direcciones IP que se describen en el Capítulo 2, "Listas básicas de control de acceso IPv4". La figura 3-3 muestra la sintaxis.

![](img/3.3.png)

En la tabla 3-2 se enumeran varios ejemplos de comandos `access-list` que utilizan solo los parámetros coincidentes necesarios. Siéntase libre de cubrir el lado derecho y usar la tabla para un ejercicio, o simplemente revise las explicaciones para tener una idea de la lógica en algunos comandos de ejemplo:

| **access-list Statement**                              | **What It Matches**                                                                                                 |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **access-list 101 deny tcp any any**                   | Any IP packet that has a TCP header                                                                                 |
| **access-list 101 deny udp any any**                   | Any IP packet that has a UDP header                                                                                 |
| **access-list 101 deny icmp any any**                  | Any IP packet that has an ICMP header                                                                               |
| **access-list 101 deny ip host 1.1.1.1 host 2.2.2.2**  | All IP packets from host 1.1.1.1 going to host 2.2.2.2, regardless of the header after the IP header                |
| **access-list 101 deny udp 1.1.1.0** **0.0.0.255 any** | All IP packets that have a UDP header following the IP header, from subnet 1.1.1.0/24, and going to any destination |

La última entrada de la Tabla 3-2 ayuda a hacer un punto importante sobre cómo IOS procesa las ACL extendidas:

En un comando `access-list` de ACL extendido, todos los parámetros coincidentes deben coincidir con el paquete para que el paquete coincida con el comando.

Por ejemplo, en el último ejemplo de la Tabla 3-2, el comando comprueba si hay UDP, un origen

Dirección IP de la subred 1.1.1.0/24 y cualquier dirección IP de destino. Si se examinara un paquete con la dirección IP de origen 1.1.1.1, coincidiría con la comprobación de la dirección IP de origen, pero si tuviera un encabezado TCP en lugar de UDP, no coincidiría con este comando `access-list`. Todos los parámetros deben coincidir.
### Coincidencia de números de puerto TCP y UDP

Las ACL extendidas también pueden examinar partes de los encabezados TCP y UDP, en particular los campos de número de puerto de origen y destino. Los números de puerto identifican la aplicación que envía o recibe los datos.

Los puertos más útiles para comprobar son los puertos conocidos utilizados por los servidores. Por ejemplo, los servidores web utilizan el conocido puerto 80 de forma predeterminada. La Figura 3-4 muestra la ubicación de los números de puerto en el encabezado TCP, después del encabezado IP.

![](img/3.4.png)

Cuando un comando de ACL extendido incluye la  palabra clave **tcp** o **udp**, ese comando puede hacer referencia opcionalmente al puerto de origen y/o destino. Para realizar estas comparaciones, la sintaxis utiliza palabras clave para igual, no igual, menor que, mayor que y para un rango de números de puerto. Además, el comando puede usar los números de puerto decimales literales o palabras clave más convenientes para algunos puertos de aplicación conocidos. La Figura 3-5 muestra las posiciones de los campos de puerto de origen y destino en el comando `access-list` y estas palabras clave de número de puerto.

![](img/3.5.png)

Por ejemplo, considere la red simple que se muestra en la Figura 3-6. El servidor FTP se encuentra a la derecha, con el cliente a la izquierda. La figura muestra la sintaxis de una ACL que coincide con lo siguiente:

- Paquetes que incluyen un encabezado TCP
- Paquetes enviados desde la subred del cliente
- Paquetes enviados a la subred del servidor
- Paquetes con puerto de destino TCP 21 (puerto de control del servidor FTP)

![](img/3.6.png)

Para apreciar completamente la coincidencia del puerto de destino con los parámetros `eq 21`, considere los paquetes que se mueven de izquierda a derecha, de PC1 al servidor. Suponiendo que el servidor utiliza el conocido puerto 21 (puerto de control FTP), el encabezado TCP del paquete tiene un valor de puerto de destino de 21. La sintaxis de ACL incluye los parámetros `eq 21` después de la dirección IP de destino. La posición después de los parámetros de la dirección de destino es importante: esa posición identifica el hecho de que los parámetros `eq 21` deben compararse con el puerto de destino del paquete. Como resultado, la instrucción ACL que se muestra en la Figura 3-6 coincidiría con este paquete y el puerto de destino de 21 si se utiliza en cualquiera de las cuatro ubicaciones implicadas en las cuatro líneas con flechas discontinuas de la figura.

Por el contrario, la Figura 3-7 muestra el flujo inverso, con un paquete enviado por el servidor hacia PC1. En este caso, el encabezado TCP del paquete tiene un puerto de origen de 21, por lo que la ACL debe verificar el valor del puerto de origen de 21 y la ACL debe ubicarse en diferentes interfaces. En este caso, los parámetros `eq 21` siguen el campo de dirección de origen, pero vienen antes del campo de dirección de destino.

![](img/3.7.png)

Al examinar las ACL que coinciden con los números de puerto, primero considere la ubicación y la dirección en la que se aplicará la ACL. Esa dirección determina si el paquete se envía al servidor o desde el servidor. En ese momento, puede decidir si necesita verificar el puerto de origen o de destino en el paquete. Como referencia, la Tabla 3-3 enumera muchos de los números de puerto populares y sus protocolos y aplicaciones de capa de transporte. Tenga en cuenta que la sintaxis de los comandos 'access-list' acepta tanto los números de puerto como una versión abreviada del nombre de la aplicación.

| **Port Number(s)** | **Protocol** | **Application**    | **access-list Command Keyword** |
| ------------------ | ------------ | ------------------ | ------------------------------- |
| 20                 | TCP          | FTP data           | **ftp-data**                    |
| 21                 | TCP          | FTP control        | **ftp**                         |
| 22                 | TCP          | SSH                | **—**                           |
| 23                 | TCP          | Telnet             | **telnet**                      |
| 25                 | TCP          | SMTP               | **smtp**                        |
| 53                 | UDP, TCP     | DNS                | **domain**                      |
| 67                 | UDP          | DHCP Server        | **bootps**                      |
| 68                 | UDP          | DHCP Client        | **bootpc**                      |
| 69                 | UDP          | TFTP               | **tftp**                        |
| 80                 | TCP          | HTTP (WWW)         | **www**                         |
| 110                | TCP          | POP3               | **pop3**                        |
| 161                | UDP          | SNMP               | **snmp**                        |
| 443                | TCP          | SSL                | **—**                           |
| 514                | UDP          | Syslog             | **—**                           |
| 16,384–32,767      | UDP          | RTP (voice, video) | **—**                           |
En la tabla 3-4 se enumeran varios ejemplos de comandos de 'lista de acceso' que coinciden en función de los números de puerto. Cubra el lado derecho de la tabla e intente caracterizar los paquetes que coinciden con cada comando. A continuación, compruebe el lado derecho de la tabla para ver si está de acuerdo con la evaluación.

| **access-list Statement**                                          | **What It Matches**                                                                                                                                                                  |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **access-list 101 deny tcp any gt 49151 host 10.1.1.1 eq 23**      | Packets with a TCP header, any source IP address, with a source port greater than (gt) 49151, a destination IP address of exactly 10.1.1.1, and a destination port equal to (eq) 23. |
| **access-list 101 deny tcp any host 10.1.1.1 eq 23**               | The same as the preceding example, but any source port matches, because that parameter is omitted in this case.                                                                      |
| **access-list 101 deny tcp any host 10.1.1.1 eq telnet**           | The same as the preceding example. The **telnet** keyword is used instead of port 23.                                                                                                |
| **access-list 101 deny udp 1.0.0.0** **0.255.255.255 lt 1023 any** | A packet with a source in network 1.0.0.0/8, using UDP with a source port less than (lt) 1023, with any destination  IP address .                                                    |
### Configuración de ACL IP extendida

Debido a que las ACL extendidas pueden coincidir con tantos campos diferentes en los distintos encabezados de un paquete IP, la sintaxis del comando no se puede resumir fácilmente en un solo comando genérico. Sin embargo, los dos comandos de la Tabla 3-5 resumen las opciones de sintaxis que se tratan en este resumen.

| **Command**                                                                                                                                                                                                           | **Configuration Mode and Description**                                                                           |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **access-list** _access-list-number_ {**deny** \| **permit**} _protocol source source-wildcard destination_ _destination-wildcard_ [**log** \| **log-input**]                                                         | Global command for extended numbered access lists. Use a number between 100 and 199 or 2000 and 2699, inclusive. |
| **access-list** _access-list-number_ {**deny** \| **permit**} {**tcp** \| **udp**} _source source-wildcard_ [_operator_ [_port_]] _destination destination-wildcard_ [_operator_[_port_]] [**established**] [**log**] | A version of the **access-list** command with parameters specific to TCP and/or UDP.                             |
El proceso de configuración de las ACL extendidas coincide en su mayoría con el mismo proceso utilizado para las ACL estándar. Debe elegir la ubicación y la dirección en la que desea habilitar la ACL, en particular la dirección, para que pueda caracterizar si determinadas direcciones y puertos serán el origen o el destino. Configure la ACL mediante los comandos 'access-list' y, cuando haya terminado, habilite la ACL con el mismo comando 'ip access-group' que se utiliza con las ACL estándar. Todos estos pasos reflejan lo que se hace con las ACL estándar; Sin embargo, a la hora de configurar, tenga en cuenta las siguientes diferencias:

- Coloque las ACL extendidas lo más cerca posible de la fuente de los paquetes que se filtrarán. El filtrado cerca de la fuente de los paquetes ahorra algo de ancho de banda.
- Recuerde que todos los campos de un comando 'access-list' deben coincidir con un paquete para que se considere que el paquete coincide con esa instrucción 'access-list'.
- Utilice números del 100 al 199 y del 2000 al 2699 en los comandos de la 'lista de acceso'; ningún número es inherentemente mejor que otro.
### Listas de acceso IP extendidas: Ejemplo 1

Este ejemplo se centra en la comprensión de la sintaxis básica. En este caso, la ACL deniega a Bob el acceso a todos los servidores FTP en la Ethernet de R1 y deniega a Larry el acceso al servidor web de Server1. La Figura 3-8 muestra la topología de red; El ejemplo 3-1 muestra la configuración en R1.

![](img/3.8.png)

```
interface Serial0  ip address 172.16.12.1 255.255.255.0  ip access-group 101 in ! interface Serial1  ip address 172.16.13.1 255.255.255.0  ip access-group 101 in !

access-list 101 remark Stop Bob to FTP servers, and Larry to Server1 web 
access-list 101 deny tcp host 172.16.3.10 172.16.1.0 0.0.0.255 eq ftp 
access-list 101 deny tcp host 172.16.2.10 host 172.16.1.100 eq www 
access-list 101 permit ip any any
```

La primera instrucción ACL impide el acceso de Bob a los servidores FTP en la subred 172.16.1.0. La segunda instrucción impide el acceso de Larry a los servicios web en Server1. La declaración final permite el resto del tráfico.

Si nos centramos en la sintaxis por un momento, podemos ver varios elementos nuevos para revisar. En primer lugar, el número de lista de acceso para las listas de acceso ampliadas se encuentra en el rango de 100 a 199 o de 2000 a 2699. Después de la acción 'permitir' o 'denegar', el  _parámetro de protocolo_ define si desea verificar todos los paquetes IP o encabezados específicos, como encabezados TCP o UDP. Al comprobar los números de puerto TCP o UDP, debe especificar el protocolo TCP o UDP. Tanto FTP como la web utilizan TCP.

En este ejemplo se utiliza el parámetro 'eq', que significa "es igual a", para comprobar los números de puerto de destino para el control FTP (palabra clave **ftp**) y el tráfico HTTP (palabra clave **www**). Puede utilizar los valores numéricos o, para las opciones más populares, es válida una versión de texto más obvia. (Si tuvieras que escribir 'eq **80'**, la configuración mostraría 'eq www'.)

Este ejemplo habilita la ACL en dos lugares en R1: entrante en cada interfaz serial. Estas ubicaciones logran el objetivo de la ACL. Sin embargo, esa ubicación inicial se hizo para señalar que Cisco sugiere que los ubique lo más cerca posible del origen del paquete. Por lo tanto, el Ejemplo 3-2 logra el mismo objetivo que el Ejemplo 3-1 de detener el acceso de Bob a los servidores FTP en el sitio principal, y lo hace con una ACL en R3.

```
interface Ethernet0  ip address 172.16.3.1 255.255.255.0  ip access-group 103 in

access-list 103 remark deny Bob to FTP servers in subnet 172.16.1.0/24 
access-list 103 deny tcp host 172.16.3.10 172.16.1.0 0.0.0.255 eq ftp 
access-list 103 permit ip any any
```

La nueva configuración en R3 cumple con los objetivos de filtrar el tráfico de Bob, al mismo tiempo que cumple con el objetivo de diseño general de mantener la ACL cerca del origen de los paquetes. La ACL 103 en R3 se parece mucho a la ACL 101 en R1 del Ejemplo 3-1, pero esta vez, la ACL no se molesta en verificar los criterios para que coincidan con el tráfico de Larry, porque el tráfico de Larry nunca ingresará a la interfaz Ethernet 0 de R3. ACL 103 filtra el tráfico FTP de Bob a destinos en la subred 172.16.1.0/24, y todo el resto del tráfico que ingresa a la interfaz E0 de R3 llega a la red.

Listas de acceso IP extendidas: Ejemplo 2

El ejemplo 3-3, basado en la red que se muestra en la Figura 3-9, muestra otro ejemplo de cómo utilizar listas de acceso IP extendidas. En este ejemplo se utilizan los siguientes criterios:

- A Sam no se le permite el acceso a la subred de Bugs o Lucas.
- A los hosts de la Ethernet de Sevilla no se les permite el acceso a los hosts de la Ethernet de Yosemite.
- Todas las demás combinaciones están permitidas.

![](img/3.9.png)

```
interface ethernet 0  ip access-group 110 in !

access-list 110 deny ip host 10.1.2.1 10.1.1.0 0.0.0.255 
access-list 110 deny ip 10.1.2.0 0.0.0.255 10.1.3.0 0.0.0.255 
access-list 110 permit ip any any
```

Esta configuración resuelve el problema con pocas instrucciones mientras se mantiene la directriz de diseño de Cisco de colocar las ACL extendidas lo más cerca posible de la fuente del tráfico. La ACL filtra los paquetes que ingresan a la interfaz E0 de Yosemite, que es la primera interfaz de enrutador a la que ingresan los paquetes enviados por Sam. Si la ruta entre Yosemite y las otras subredes cambia con el tiempo, la ACL sigue aplicándose. Además, el filtrado exigido por el segundo requisito (no permitir que los hosts LAN de Sevilla accedan a los de Yosemite) se cumple con la segunda  **declaración de lista de acceso**. Detener el flujo de paquetes desde la subred LAN de Yosemite a la subred LAN de Sevilla detiene la comunicación efectiva entre las dos subredes. Alternativamente, la lógica contraria podría haberse configurado en Sevilla.

### Práctica Creación de comandos de lista de acceso

En la tabla 3-6 se proporciona un ejercicio de práctica que le ayudará a familiarizarse con la sintaxis del comando de **lista de acceso extendida**  , especialmente con la elección de la lógica de coincidencia correcta. Su trabajo: crear una ACL extendida de una línea que coincida con los paquetes. Las respuestas se encuentran en la sección "Respuestas a problemas de práctica anteriores", más adelante en este capítulo. Tenga en cuenta que si los criterios mencionan un protocolo de aplicación en particular, por ejemplo, "cliente web", eso significa que debe coincidir específicamente con ese protocolo de aplicación.

| **Problem** | **Criteria**                                                                                                                       |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| 1           | From web client 10.1.1.1, sent to a web server in subnet 10.1.2.0/24.                                                              |
| 2           | From Telnet client 172.16.4.3/25, sent to a Telnet server in subnet 172.16.3.0/25. Match all hosts in the client’s subnet as well. |
| 3           | ICMP messages from the subnet in which 192.168.7.200/26 resides to all hosts in the subnet where 192.168.7.14/29 resides.          |
| 4           | From web server 10.2.3.4/23’s subnet to clients in the same subnet as host 10.4.5.6/22.                                            |
| 5           | From Telnet server 172.20.1.0/24’s subnet, sent to any host in the same subnet as host 172.20.44.1/23.                             |
| 6           | From web client 192.168.99.99/28, sent to a web server in subnet 192.168.176.0/28. Match all hosts in the client’s subnet as well. |
| 7           | ICMP messages from the subnet in which 10.55.66.77/25 resides to all hosts in the subnet where 10.66.55.44/26 resides.             |
| 8           | Any and every IPv4 packet.                                                                                                         |
### ACL con nombre y edición de ACL

Ahora que tiene una buena comprensión de los conceptos básicos de las ACL IP de IOS, esta sección examina algunas mejoras en el soporte de IOS para ACL: ACL con nombre y edición de ACL con números de secuencia. Aunque ambas características son útiles e importantes, ninguna agrega ninguna función en cuanto a lo que un enrutador puede y no puede filtrar. En cambio, las ACL con nombre y los números de secuencia de ACL facilitan la memoria de los nombres de las ACL y la edición de las ACL existentes cuando es necesario cambiar una ACL.
### Listas de acceso de IP con nombre

Las ACL de IP con nombre tienen muchas similitudes con las ACL de IP numeradas. Se pueden utilizar para filtrar paquetes, además de para muchos otros fines. También pueden coincidir con los mismos campos: las ACL numeradas estándar pueden coincidir con los mismos campos que una ACL con nombre estándar, y las ACL con número extendido pueden coincidir con los mismos campos que una ACL con nombre extendida.

Por supuesto, existen diferencias entre las ACL con nombre y las numeradas. Las ACL con nombre originalmente tenían tres grandes diferencias en comparación con las ACL numeradas:
- Usar nombres en lugar de números para identificar la LCA, lo que facilita recordar el motivo de la LCA
- Uso de subcomandos de ACL, no comandos globales, para definir la acción y los parámetros coincidentes
- Uso de funciones de edición de ACL que permiten al usuario de la CLI eliminar líneas individuales de la ACL e insertar nuevas líneas

Puede aprender fácilmente la configuración de ACL con nombre simplemente convirtiendo las ACL numeradas para usar la configuración de ACL con nombre equivalente. La Figura 3-10 muestra una conversión de este tipo, utilizando una ACL estándar simple de tres líneas número 1. Para crear los tres **subcomandos  de permiso** para la ACL nombrada, copie literalmente partes de los tres comandos de ACL numerados, comenzando con la  **palabra clave** permit .

![](img/3.10.png)

