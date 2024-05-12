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

