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

