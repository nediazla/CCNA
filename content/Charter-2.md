![](img/switch-1.png)
# Listas básicas de control de acceso IPv4

## En este capítulo se tratan los siguientes temas del examen:

- Fundamentos de seguridad
- Configurar y verificar las listas de control de acceso

Las listas de control de acceso (ACL) IPv4 brindan a los ingenieros de redes la capacidad de programar un filtro en un enrutador. Cada router, en cada interfaz, tanto para la dirección de entrada como de salida, puede habilitar una ACL diferente con reglas diferentes. Las reglas de cada ACL le dicen al router qué paquetes descartar y cuáles permitir el paso.

En este capítulo se analizan los conceptos básicos de las ACL IPv4 y, en particular, un tipo de ACL IP: las ACL IP numeradas estándar. Las ACL numeradas estándar utilizan una lógica simple, coincidiendo solo en el campo de dirección IP de origen y utilizan un estilo de configuración que hace referencia a la ACL mediante un número. Este capítulo tiene como objetivo ayudarlo a aprender primero este tipo más simple de ACL. El siguiente capítulo, titulado "Listas avanzadas de control de acceso IPv4", completa la discusión describiendo otros tipos de ACL IP. Los otros tipos de ACL utilizan características que se basan en los conceptos que aprenderá en este capítulo, pero con más complejidad y opciones de configuración adicionales.

## Conceptos básicos de la lista de control de acceso IPv4

Las listas de control de acceso IPv4 (IP ACL) proporcionan a los ingenieros de red una forma de identificar diferentes tipos de paquetes. Para ello, la configuración de ACL enumera los valores que el router puede ver en los encabezados IP, TCP, UDP y otros. Por ejemplo, una ACL puede coincidir con paquetes cuya dirección IP de origen es 1.1.1.1, o paquetes cuya dirección IP de destino es alguna dirección en la subred 10.1.1.0/24, o paquetes con un puerto de destino del puerto TCP 23 (Telnet).

Las ACL IPv4 realizan muchas funciones en los routers Cisco, siendo el uso más común el filtro de paquetes. Los ingenieros pueden habilitar ACL en un router para que la ACL se encuentre en la ruta de reenvío de los paquetes a medida que pasan por el router. Una vez habilitado, el router considera si cada paquete IP se descartará o se permitirá que continúe como si la ACL no existiera.

Sin embargo, las ACL también se pueden utilizar para muchas otras funciones de IOS. Por ejemplo, las ACL se pueden utilizar para hacer coincidir paquetes para aplicar funciones de calidad de servicio (QoS). QoS permite que un router dé a algunos paquetes un mejor servicio y a otros paquetes un peor servicio. Por ejemplo, los paquetes que contienen voz digitalizada deben tener un retraso muy bajo, por lo que las ACL pueden coincidir con los paquetes de voz, con la lógica de QoS a su vez reenviando los paquetes de voz más rápidamente que los paquetes de datos.

En esta primera sección se presentan las ACL IP que se utilizan para el filtrado de paquetes, centrándose en estos aspectos de las ACL: las ubicaciones y la dirección en las que se habilitan las ACL, la coincidencia de paquetes mediante el examen de los encabezados y la toma de medidas después de que se haya emparejado un paquete.

## Ubicación y dirección de ACL

Los routers Cisco pueden aplicar la lógica ACL a los paquetes en el punto en el que los paquetes IP ingresan a una interfaz o en el punto en el que salen de una interfaz. En otras palabras, la ACL se asocia con una interfaz y para una dirección de flujo de paquetes (ya sea de entrada o de salida). Es decir, la ACL se puede aplicar de entrada al router, antes de que el router tome su decisión de reenvío (enrutamiento), o de salida, después de que el router tome su decisión de reenvío y haya determinado la interfaz de salida que se va a utilizar.

Las flechas de la Figura 2-1 muestran las ubicaciones en las que se pueden filtrar los paquetes que fluyen de izquierda a derecha en la topología. Por ejemplo, imagine que desea permitir los paquetes enviados por el host A al servidor S1, pero descartar los paquetes enviados por el host B al servidor S1. Cada línea con flechas representa una ubicación y una dirección en la que un router podría aplicar una ACL, filtrando los paquetes enviados por el host B.

Las cuatro líneas con flechas de la figura señalan la ubicación y la dirección de las interfaces de router utilizadas para reenviar el paquete desde el host B al servidor S1. En este ejemplo concreto, esas interfaces y dirección son entrantes en la interfaz F0/0 de R1, salientes en la interfaz S0/0/0 de R1, entrantes en la interfaz S0/0/1 de R2 y salientes en la interfaz F0/0 de R2. Si, por ejemplo, habilitó una ACL en la interfaz F0/1 de R2, en cualquier dirección, esa ACL no podría filtrar el paquete enviado desde el host B al servidor S1, porque la interfaz F0/1 de R2 no forma parte de la ruta de B a S1.

![](2.1.png)

En resumen, para filtrar un paquete, debe habilitar una ACL en una interfaz que procese el paquete, en la misma dirección en la que el paquete fluye a través de esa interfaz.

Cuando está habilitado, el router procesa cada paquete IP entrante o saliente utilizando esa ACL. Por ejemplo, si se habilita en R1 para los paquetes entrantes en la interfaz F0/0, R1 comparará cada paquete IP entrante en F0/0 con la ACL para decidir el destino de ese paquete: continuar sin cambios o descartarse.
## Paquetes coincidentes

Cuando piensa en la ubicación y la dirección de una ACL, ya debe estar pensando en qué paquetes planea filtrar (descartar) y cuáles desea dejar pasar. Para decirle al router esas mismas ideas, debe configurar el router con una ACL IP que coincida con los paquetes. _Los paquetes coincidentes_ se refieren a cómo configurar los comandos de ACL para ver cada paquete, enumerando cómo identificar qué paquetes deben descartarse y cuáles deben permitirse.

Cada ACL IP consta de uno o más comandos de configuración, y cada comando enumera detalles sobre los valores que se deben buscar dentro de los encabezados de un paquete. Por lo general, un comando de ACL utiliza lógica como "busque estos valores en el encabezado del paquete y, si los encuentra, descarte el paquete". (En su lugar, la acción podría ser permitir el paquete, en lugar de descartarlo). Específicamente, la ACL busca campos de encabezado que ya debería conocer bien, incluidas las direcciones IP de origen y destino, además de los números de puerto TCP y UDP.

Por ejemplo, considere un ejemplo con la Figura 2-2, en el que desea permitir paquetes del host A al servidor S1, pero descartar los paquetes del host B que van a ese mismo servidor. Todos los hosts ahora tienen direcciones IP y la figura muestra el pseudocódigo para una ACL en R2. La Figura 2-2 también muestra la ubicación elegida para habilitar la ACL: entrante en la interfaz S0/0/1 de R2.

La Figura 2-2 muestra una ACL de dos líneas en un rectángulo en la parte inferior, con una lógica de coincidencia simple: ambas instrucciones solo buscan coincidir con la dirección IP de origen en el paquete. Cuando está habilitado, R2 examina cada paquete IP entrante en esa interfaz y compara cada paquete con esos dos comandos ACL. Los paquetes enviados por el host A (dirección IP de origen 10.1.1.1) se permiten y los originados por el host B (dirección IP de origen 10.1.1.2) se descartan.

![](2.2.png)
### Tomar medidas cuando se produce un partido

Cuando se utilizan ACL IP para filtrar paquetes, solo se puede elegir una de las dos acciones. Los comandos de configuración utilizan las palabras clave  **deny** y  **permit**, y significan (respectivamente) descartar el paquete o permitir que siga funcionando como si la ACL no existiera.

Este resumen se centra en el uso de ACL para filtrar paquetes, pero IOS utiliza ACL para muchas más funciones. Esas características suelen usar la misma lógica de coincidencia. Sin embargo, en otros casos, las  **palabras clave deny** o **allow** implican alguna otra acción.

### Tipos de ACL IP

Cisco IOS ha admitido ACL IP desde los primeros días de los routers de Cisco. Comenzando con las ACL IP numeradas estándar originales en los primeros días de IOS, que podrían habilitar la lógica mostrada anteriormente en torno a la Figura 2-2, Cisco ha agregado muchas funciones de ACL, incluidas las siguientes:

- ACL numeradas estándar (1–99)
- ACL numeradas extendidas (100–199)
- Números de ACL adicionales (estándar 1300-1999, 2000-2699 ampliado)
- ACL con nombre
- Edición mejorada con números de secuencia

Este capítulo se centra únicamente en las ACL IP numeradas estándar, mientras que en el siguiente capítulo se analizan las otras tres categorías principales de ACL IP. En resumen, las ACL IP estarán numeradas o nombradas en el sentido de que la configuración identifica la ACL mediante un número o un nombre. Las ACL también serán estándar o extendidas, y las ACL extendidas tendrán capacidades mucho más sólidas para hacer coincidir paquetes. La Figura 2-3 resume las grandes ideas relacionadas con las categorías de ACL IP.

![](2.3.png)

### ACL IPv4 numeradas estándar

El título de esta sección sirve como una gran introducción, si puede decodificar lo que Cisco quiere decir con cada palabra específica. Esta sección trata sobre un tipo de filtro de Cisco (_ACL_) que coincide solo con la dirección IP de origen del paquete (_estándar_), está configurado para identificar la ACL mediante números en lugar de nombres (_numerados_) y examina los paquetes IPv4.

En esta sección se examinan los detalles de las ACL IP numeradas estándar. En primer lugar, examina la idea de que una ACL es una lista y qué lógica utiliza esa lista. A continuación, el texto analiza de cerca cómo hacer coincidir el campo de dirección IP de origen en el encabezado del paquete, incluida la sintaxis de los comandos. Esta sección termina con una visión completa de los comandos de configuración y verificación para implementar ACL estándar.
### Lógica de lista con ACL IP

Una sola ACL es una sola entidad y, al mismo tiempo, una lista de uno o más comandos de configuración. Como una sola entidad, la configuración habilita toda la ACL en una interfaz, en una dirección específica, como se muestra anteriormente en la Figura 2-1. Como una lista de comandos, cada comando tiene una lógica de coincidencia diferente que el router debe aplicar a cada paquete al filtrar mediante que ACL.

Al realizar el procesamiento de ACL, el router procesa el paquete, en comparación con la ACL, de la siguiente manera:

Las ACL utilizan la lógica de primera coincidencia. Una vez que un paquete coincide con una línea de la ACL, el router realiza la acción indicada en esa línea de la ACL y deja de buscar más en la ACL.

Para ver exactamente lo que eso significa, considere el ejemplo construido alrededor de la Figura 2-4. En la figura se muestra un ejemplo de ACL 1 con tres líneas de pseudocódigo. Este ejemplo aplica ACL 1 en la interfaz S0/0/1 de R2, entrante (la misma ubicación que en la Figura 2-2 anterior).

![](2.4.png)

Considere la lógica de ACL de primera coincidencia para un paquete enviado por el host A al servidor S1. La dirección IP de origen será 10.1.1.1 y se enrutará para que ingrese a la interfaz S0/0/1 de R2, impulsando la lógica ACL 1 de R2. R2 compara este paquete con la ACL, haciendo coincidir el primer elemento de la lista con una acción de permiso. Por lo tanto, se debe permitir el paso de este paquete, como se muestra en la Figura 2-5, a la izquierda.

![](2.5.png)

A continuación, considere un paquete enviado por el host B, dirección IP de origen 10.1.1.2. Cuando el paquete ingresa a la interfaz S0/0/1 de R2, R2 compara el paquete con la primera instrucción de ACL 1 y no hace una coincidencia (10.1.1.1 no es igual a 10.1.1.2). A continuación, R2 pasa a la segunda afirmación, que requiere alguna aclaración. El pseudocódigo ACL, en la Figura 2-4, muestra 10.1.1.x, que pretende ser una abreviatura de que cualquier valor puede existir en el último octeto. Comparando solo los primeros tres octetos, R2 decide que este último paquete tiene una dirección IP de origen que comienza con los primeros tres octetos 10.1.1, por lo que R2 considera que es una coincidencia en la segunda instrucción. R2 realiza la acción indicada (denegar) y descarta el paquete. R2 también detiene el procesamiento de ACL en el paquete, ignorando la tercera línea de la ACL.

Por último, considere un paquete enviado por el host C, de nuevo al servidor S1. El paquete tiene la dirección IP de origen 10.3.3.3, por lo que cuando ingresa a la interfaz S0/0/1 de R2 e impulsa el procesamiento de ACL en R2, R2 mira el primer comando en ACL 1. R2 no coincide con el primer comando ACL (10.1.1.1 en el comando no es igual a 10.3.3.3 del paquete). R2 examina el segundo comando, compara los tres primeros octetos (10.1.1) con la dirección IP de origen del paquete (10.3.3) y sigue sin encontrar ninguna coincidencia. A continuación, R2 examina el tercer comando. En este caso, el comodín significa ignorar los últimos tres octetos y simplemente comparar el primer octeto (10), para que el paquete coincida. A continuación, R2 realiza la acción indicada (permiso), lo que permite que el paquete siga funcionando.

Esta secuencia de procesamiento de una ACL como una lista ocurre para cualquier tipo de ACL de IOS: IP, otros protocolos, estándar o extendidos, con nombre o numerados.

Por último, si un paquete no coincide con ninguno de los elementos de la ACL, el paquete se descarta. La razón es que cada ACL de IP tiene una  _declaración de denegación de todo_ implícita al final de la ACL. No existe en la configuración, pero si un router sigue buscando en la lista y no se realiza ninguna coincidencia al final de la lista, IOS considera que el paquete ha coincidido con una entrada que tiene una acción de **denegación**.
### Coincidencia de lógica y sintaxis de comandos

Las ACL IP numeradas estándar utilizan el siguiente comando global:

```
access-list {1-99 | 1300-1999} {permit | deny} _matching-parameters_
```

Cada ACL numerada estándar tiene uno o más  comandos **de lista de acceso** con el mismo número, cualquier número de los rangos que se muestran en la línea de sintaxis anterior. (Un número no es mejor que el otro). IOS se refiere a cada línea de una ACL como una entrada de control de acceso (ACE), pero muchos ingenieros simplemente las llaman instrucciones ACL.

Además del número de ACL, cada  **comando access-list** también enumera la acción (**permitir** o **denegar**), además de la lógica coincidente. En el resto de esta sección se examina cómo configurar los parámetros de coincidencia, lo que, para las ACL estándar, significa que solo puede hacer coincidir la dirección IP de origen o partes de la dirección IP de origen mediante algo denominado máscara de comodín de ACL.
### Coincidencia de la dirección IP exacta

Para que coincida con una dirección IP de origen específica, la dirección IP completa, todo lo que tiene que hacer es escribir esa dirección IP al final del comando. Por ejemplo, en el ejemplo anterior se usa pseudocódigo para "permit if source = 10.1.1.1". El siguiente comando configura esa lógica con la sintaxis correcta utilizando el número 1 de ACL:  

```
access-list 1 permit 10.1.1.1
```

Hacer coincidir la dirección IP completa exacta es así de simple.

En versiones anteriores de IOS, la sintaxis incluía una  palabra clave host. En lugar de simplemente escribir la dirección IP completa, primero escribió la  palabra clave host y luego la dirección IP. Tenga en cuenta que en versiones posteriores de IOS, si utiliza la  palabra clave host, IOS acepta el comando pero luego elimina la palabra clave. 

```
access-list 1 permit host 10.1.1.1
```

Hacer coincidir un subconjunto de la dirección con comodines

A menudo, los objetivos comerciales que desea implementar con una ACL no coinciden con una sola dirección IP en particular, sino con un rango de direcciones IP. Tal vez desee hacer coincidir todas las direcciones IP de una subred. Tal vez desee hacer coincidir todas las direcciones IP en un rango de subredes. De todos modos, desea verificar si hay más de una dirección IP en un rango de direcciones.

IOS permite que las ACL estándar coincidan con un rango de direcciones utilizando una herramienta llamada máscara de _comodín_. Tenga en cuenta que no se trata de una máscara de subred. La máscara comodín (que este resumen abrevia como _máscara WC_) le da al ingeniero una forma de decirle a IOS que ignore partes de la dirección al hacer comparaciones, esencialmente tratando esas partes como comodines, como si ya coincidiesen.

Puedes pensar en las máscaras WC en decimal y en binario, y ambos tienen sus usos. Para empezar, piensa en las máscaras de WC en decimal, usando estas reglas:

**Decimal 0:** El router debe comparar este octeto como de costumbre.
**Decimal 255:** El router ignora este octeto, considerando que ya coincide.

Teniendo en cuenta estas dos reglas, considere la Figura 2-6, que muestra esta lógica utilizando tres máscaras WC diferentes pero populares: una que le dice al router que ignore el último octeto, otra que le dice al router que ignore los dos últimos octetos y otra que le dice al router que ignore los últimos tres octetos.

![](2.6.png)

Los tres ejemplos de los recuadros de la Figura 2-6 muestran dos números que son claramente diferentes.

La máscara WC hace que IOS compare solo algunos de los octetos, mientras que ignora otros octetos. Los tres ejemplos dan como resultado una coincidencia, porque cada máscara comodín le dice a IOS que ignore algunos octetos. El ejemplo de la izquierda muestra la máscara WC 0.0.0.255, que le dice al router que trate el último octeto como un comodín, esencialmente ignorando ese octeto para la comparación. De manera similar, el ejemplo central muestra la máscara WC 0.0.255.255, que le dice al router que ignore los dos octetos de la derecha. El caso más a la derecha muestra la máscara WC 0.255.255.255, lo que le dice al router que ignore los últimos tres octetos al comparar valores.

Para ver la máscara de WC en acción, piense en el ejemplo anterior relacionado con la Figura 2-4 y la Figura 2-5. La ACL de pseudocódigo en esas dos figuras usaba lógica que se puede crear usando una máscara WC. Como recordatorio, la lógica de la ACL de pseudocódigo en esas dos figuras incluía lo siguiente:

**Línea 1:** Hacer coincidir y permitir todos los paquetes con una dirección de origen exactamente 10.1.1.1.
**Línea 2:** Haga coincidir y deniegue todos los paquetes con direcciones de origen con los primeros tres octetos 10.1.1.
**Línea 3:** Hacer coincidir y permitir todas las direcciones con el primer octeto 10.

La Figura 2-7 muestra la versión actualizada de la Figura 2-4, pero con la sintaxis completa y correcta, incluidas las máscaras WC. En particular, tenga en cuenta el uso de la máscara WC 0.0.0.255 en el segundo comando, diciéndole a R2 que ignore el último octeto del número 10.1.1.0, y la máscara WC 0.255.255.255 en el tercer comando, diciéndole a R2 que ignore los últimos tres octetos del valor

```
10.0.0.0.
```

Por último, tenga en cuenta que cuando se utiliza una máscara WC, el  **parámetro source** definido  libremente del comando _access-list_ debe ser un 0 en cualquier octeto en el que la máscara WC sea un 255. IOS especificará una dirección de origen que sea 0 para las partes que se ignorarán, incluso si se configuraron valores distintos de cero.

![](2.7.png)

### Máscaras de comodín binario

Las máscaras comodín, como valores de número decimal con puntos (DDN), representan en realidad un número binario de 32 bits. Como un número de 32 bits, la máscara WC en realidad dirige la lógica del router bit a bit. En resumen, un bit de máscara WC de 0 significa que la comparación debe realizarse normalmente, pero un binario 1 significa que el bit es un comodín y se puede ignorar al comparar los números.

Afortunadamente, para los fines del estudio CCNA, y para la mayoría de las aplicaciones del mundo real, puede ignorar la máscara WC binaria. ¿Por qué? Bueno, generalmente queremos hacer coincidir un rango de direcciones que se puedan identificar fácilmente por un número de subred y una máscara, ya sea una subred real o una ruta de resumen que agrupa las subredes. Si puede describir el rango de direcciones con un número de subred y una máscara, puede encontrar los números que se usarán en su ACL con algunas matemáticas decimales simples, como se describe a continuación.

Encontrar la máscara de comodín adecuada para que coincida con una subred

En muchos casos, una ACL debe coincidir con todos los hosts de una subred en particular. Para hacer coincidir una subred con una ACL, puede utilizar el siguiente acceso directo:

- Utilice el número de subred como valor de origen en el  **comando access-list**.
- Utilice una máscara de comodín encontrada restando la máscara de subred de 255.255.255.255.

Por ejemplo, para la subred 172.16.8.0 255.255.252.0, use el número de subred (172.16.8.0) como parámetro de dirección y, a continuación, realice los siguientes cálculos matemáticos para encontrar la máscara comodín:
```
  255.255.255.255
– 255.255.252.0
   0 . 0 . 3 .255
```

Continuando con este ejemplo, un comando completo para esta misma subred sería el siguiente:

```
access-list 1 permit 172.16.8.0 0.0.3.255
```

La sección "Práctica de la aplicación de ACL IP estándar" le brinda la oportunidad de practicar la coincidencia de subredes al configurar ACL.

### Coincidencia de todas las direcciones

En algunos casos, querrá que un comando de ACL coincida con todos y cada uno de los paquetes que llegan a ese punto en la ACL. En primer lugar, hay que conocer la forma (sencilla) de hacer coincidir todos los paquetes utilizando la  palabra clave any. Y lo que es más importante, hay que pensar en cuándo hacer coincidir todos y cada uno de ellosPaquetes.

En primer lugar, para hacer coincidir todos y cada uno de los paquetes con un comando ACL, basta con utilizar la  palabra clave any para la dirección. Por ejemplo, para permitir todos los paquetes: 

```
access-list 1 allow any
```

Entonces, ¿cuándo y dónde debe usar dicho comando? Recuerde que todas las ACL IP de Cisco terminan con una **denegación implícita**  de cualquier concepto al final de cada ACL. Es decir, si un router compara un paquete con la ACL y el paquete no coincide con ninguna de las instrucciones configuradas, el router descarta el paquete. ¿Desea anular ese comportamiento predeterminado? Configure un **permiso cualquiera** al final de la ACL.

Es posible que también desee configurar explícitamente un comando para eliminar todo el tráfico (por ejemplo, `access-list 1 deny any`) al final de una ACL. ¿Por qué, cuando la misma lógica ya se encuentra al final de la ACL de todos modos? Bueno, los  **comandos show de  ACL** enumeran contadores para el número de paquetes que coinciden con cada comando en la ACL, pero no hay ningún contador para ese concepto implícito **de denegación** al  final de la ACL. Por lo tanto, si desea ver los contadores de cuántos paquetes coinciden con la  lógica de **denegación de cualquier** al final de la ACL, configure una **denegación explícita de cualquiera**.
### Implementación de ACL IP estándar

En este capítulo ya se han presentado todos los pasos de configuración por partes. En esta sección se resumen esas piezas como un proceso de configuración. El proceso también hace referencia al  **comando access-list**, cuya sintaxis genérica se repite aquí como referencia:

```
access-list _access-list-number_ {deny | permit} _source_ [_source-wildcard_]
```

|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Paso 1.** | Planifique la ubicación (router e interfaz) y la dirección (entrada o salida) en esa interfaz:<br><br>1.     Las ACL estándar deben colocarse cerca del destino de los paquetes para que no descarten involuntariamente paquetes que no deben descartarse.<br><br>2.     Debido a que las ACL estándar solo pueden coincidir con la dirección IP de origen de un paquete, identifique las direcciones IP de origen de los paquetes a medida que avanzan en la dirección que la ACL está examinando. |
| **Paso 2.** | Configure uno o más  comandos de configuración global **de lista de acceso** para crear la ACL, teniendo en cuenta lo siguiente:<br><br>1.     La búsqueda en la lista se realiza secuencialmente, utilizando la lógica de primera coincidencia.<br><br>2.     La acción predeterminada, si un paquete no coincide con ninguno de los  **comandos access-list**, es **denegar** (descartar) el paquete.                                                                                             |
| **Paso 3.** | Habilite la ACL en la interfaz del router elegida, en la dirección correcta, utilizando el  **subcomando ip access-group** _number_ {**in \| out**} interface .                                                                                                                                                                                                                                                                                                                                     |
El resto de esta sección muestra un par de ejemplos.

#### Ejemplo 1 de ACL numerada estándar

En el primer ejemplo se muestra la configuración para los mismos requisitos que se muestra en las figuras 2-4 y 2-5. Reformulado, los requisitos para esta ACL son los siguientes:

1.      Habilite la entrada de ACL en la interfaz S0/0/1 de R2.
2.      Permitir paquetes procedentes del host A.
3.      Denegar los paquetes procedentes de otros hosts de la subred del host A.
4.      Permita paquetes provenientes de cualquier otra dirección en la red de Clase A 10.0.0.0.
5.      El ejemplo original no hacía ningún comentario sobre qué hacer de forma predeterminada, así que simplemente deniega todo el resto del tráfico.

El ejemplo 2-1 muestra una configuración correcta completada, comenzando con el proceso de configuración, seguido de la salida del  `comando show running-config`.

```
R2# configure terminal //Enter configuration commands, one per line. End with CNTL/Z.

R2(config)# access-list 1 permit 10.1.1.1
R2(config)# access-list 1 deny 10.1.1.0 0.0.0.255
R2(config)# access-list 1 permit 10.0.0.0 0.255.255.255
R2(config)# interface S0/0/1 
R2(config-if)# ip access-group 1 in
R2(config-if)# ^Z
R2# show running-config 

! Lines omitted for brevity

access-list 1 permit 10.1.1.1 
access-list 1 deny 10.1.1.0 0.0.0.255 
access-list 1 permit 10.0.0.0 0.255.255.255
```

En primer lugar, preste mucha atención al proceso de configuración que aparece en la parte superior del ejemplo. Tenga en cuenta que el  `comando access-list` no cambia el símbolo del sistema del símbolo del sistema del modo de configuración global, ya que el  `comando access-list` es un comando de configuración global. Luego, compare eso con el resultado del  comando `show running-config`: los detalles son idénticos en comparación con los comandos que se agregaron en el modo de configuración. Finalmente, asegúrese de anotar el `ip access-group 1` en el  comando, en la `interfaz S0/0/1` de R2, que habilita la lógica ACL (tanto la ubicación como la dirección).

El Ejemplo 2-2 enumera algunos resultados del Router R2 que muestran información sobre esta ACL. El  **comando `show ip access-lists` enumera detalles sobre las ACL IPv4 solamente, mientras que el  `comando show access- lists` enumera detalles sobre las ACL IPv4 más cualquier otro tipo de ACL que esté configurado actualmente; por ejemplo, ACL IPv6 .

```
R2# show ip access-lists

Standard IP access list 1

    10 permit 10.1.1.1 (107 matches)
    20 deny   10.1.1.0, wildcard bits 0.0.0.255 (4 matches)
	30 permit 10.0.0.0, wildcard bits 0.255.255.255 (10 matches)

R2# show access-lists

Standard IP access list 1

    10 permit 10.1.1.1 (107 matches)
    20 deny   10.1.1.0, wildcard bits 0.0.0.255 (4 matches)     
    30 permit 10.0.0.0, wildcard bits 0.255.255.255 (10 matches)

R2# show ip interface s0/0/1

Serial0/0/1 is up, line protocol is up

Internet address is 10.1.2.2/24

  Broadcast address is 255.255.255.255   Address determined by setup command
  MTU is 1500 bytes
  Helper address is not set
  Directed broadcast forwarding is disabled   
  Multicast reserved groups joined: 224.0.0.9
  Outgoing access list is not set
  Inbound access list is 1! 
  Lines omitted for brevity
```

El resultado de estos comandos muestra dos elementos de interés. La primera línea de salida en este caso indica el tipo (estándar) y el número. Si existiera más de una ACL, vería varias estrofas de salida, una por ACL, cada una con una línea de encabezado como esta. A continuación, estos comandos enumeran los recuentos de paquetes para el número de paquetes que el router ha hecho coincidir con cada comando. Por ejemplo, hasta ahora 107 paquetes han coincidido con la primera línea de la ACL.

Finalmente, al final del ejemplo se muestra el resultado del  **comando show ip interface**. Este comando enumera, entre muchos otros elementos, el número o el nombre de cualquier ACL IP habilitada en la interfaz según el  subcomando `ip access-group` interface.

#### Ejemplo 2 de ACL numerada estándar

Para el segundo ejemplo, use la Figura 2-8 e imagine que su jefe le da algunos requisitos apresuradamente en el pasillo. Al principio, le dice que quiere filtrar los paquetes que van desde los servidores de la derecha hacia los clientes de la izquierda. A continuación, dice que quiere que permita el acceso de los hosts A, B y otros hosts de su misma subred al servidor S1, pero deniegue el acceso a ese servidor a los hosts de la subred del host C. A continuación, le dice que, además, a los hosts de la subred del host A se les debe denegar el acceso al servidor S2, pero a los hosts de la subred del host C se les debe permitir el acceso al servidor S2, todo filtrando los paquetes que van solo de derecha a izquierda. A continuación, le dice que coloque la ACL entrante en la interfaz F0/0 de R2.

![](2.8.png)

Si revisas todos los comentarios del jefe, los requisitos podrían reducirse a lo siguiente:

1.      Habilite la entrada de ACL en la interfaz F0/0 de R2.
2.      Permitir que los paquetes del servidor S1 vayan a los hosts de la subred de A.
3.      Denegar los paquetes del servidor S1 que van a los hosts de la subred de C.
4.      Permitir que los paquetes del servidor S2 vayan a los hosts de la subred de C.
5.      Denegar los paquetes del servidor S2 que van a los hosts de la subred de A.
6.      (No hubo ningún comentario sobre qué hacer de forma predeterminada; use el valor predeterminado implícito **de denegar todo** ).

Resulta que no puedes hacer todo lo que tu jefe te pidió con un ACL estándar. Por ejemplo, considere el comando obvio para el requisito número 2: `access-list 2 permit 10.2.2.1`. Esto permite todo el tráfico cuya IP de origen es 10.2.2.1 (servidor S1). El siguiente requisito le pide que filtre (deniegue) los paquetes procedentes de esa misma dirección IP. Incluso si agregara otro comando que verificara la dirección IP de origen 10.2.2.1, el router nunca llegaría a ella, porque los routers utilizan la lógica de primera coincidencia al buscar la ACL. No puede comprobar la dirección IP de destino y la de origen, ya que las ACL estándar no pueden comprobar la dirección IP de destino.

Para resolver este problema, ¡deberías conseguir un nuevo jefe! No, en serio, hay que replantearse el problema y cambiar las reglas. En la vida real, probablemente utilizaría una ACL extendida en su lugar, que le permite verificar tanto la dirección IP de origen como la de destino.

Con el fin de practicar otra ACL estándar, imagina que tu jefe te permite cambiar los requisitos. En primer lugar, utilizará dos ACL salientes, ambas en el enrutador R1. Cada ACL permitirá que el tráfico de un solo servidor se reenvíe a esa LAN conectada, con los siguientes requisitos modificados:

1. Utilizando una ACL saliente en la interfaz F0/0 de R1, permita paquetes del servidor S1 y deniegue todos los demás paquetes.
2.  Mediante una ACL saliente en la interfaz F0/1 de R1, permita paquetes del servidor S2 y deniegue todos los demás paquetes.

En el ejemplo 2-3 se muestra la configuración que completa estos requisitos.

```
access-list 2 remark This ACL permits server S1 traffic to host A's subnet 
access-list 2 permit 10.2.2.1 ! 
access-list 3 remark This ACL permits server S2 traffic to host C's subnet 
access-list 3 permit 10.2.2.2 ! 
interface F0/0  ip access-group 2 out !
interface F0/1  ip access-group 3 out
```

Como se destaca en el ejemplo, la solución con ACL número 2 permite todo el tráfico del servidor S1, con esa lógica habilitada para los paquetes que salen de la interfaz F0/0 de R1. El resto del tráfico se descartará debido a la **denegación implícita**  de todo al final de la ACL. Además, ACL 3 permite el tráfico del servidor S2, que luego puede salir de la interfaz F0/1 de R1. Además, tenga en cuenta que la solución muestra el uso del  **parámetro access-list remark**, que le permite dejar documentación de texto que permanece con la ACL.

### Consejos para la solución de problemas y la verificación

La solución de problemas de ACL IPv4 requiere cierta atención a los detalles. En particular, debe estar preparado para mirar la dirección y la máscara de comodín y predecir con confianza las direcciones que coinciden con esos dos parámetros combinados. Los próximos problemas de práctica un poco más adelante en este capítulo pueden ayudarte a prepararte para esa parte del trabajo. Pero algunos otros consejos también pueden ayudarlo a verificar y solucionar problemas de ACL en los exámenes.

En primer lugar, puede saber si el router está haciendo coincidir los paquetes o no con un par de herramientas. Ejemplo

2-2 ya mostró que IOS mantiene estadísticas sobre los paquetes coincidentes por cada línea de una ACL. Además, si agrega la  palabra clave log al final de un  **comando access-list**, IOS emite mensajes de registro con estadísticas ocasionales sobre las coincidencias de esa línea particular de la ACL. Tanto las estadísticas como los mensajes de registro pueden ser útiles para decidir qué línea de la ACL coincide con un paquete.

Por ejemplo, el Ejemplo 2-4 muestra una versión actualizada de ACL 2 del Ejemplo 2-3, esta vez con la  palabra clave log agregada. A continuación, la parte inferior del ejemplo muestra un mensaje de registro típico, que muestra la coincidencia resultante basada en un paquete con la dirección IP de origen 10.2.2.1 (coincidente con la ACL) con la dirección de destino 10.1.1.1 .

```
R1# show running-config 
!lines removed for brevity access-list 2 remark This ACL permits server S1 traffic to host A's subnet access-list 2 permit 10.2.2.1 log!

interface F0/0  
ip access-group 2 out

R1#

Feb 4 18:30:24.082: %SEC-6-IPACCESSLOGNP: list 2 permitted 0 10.2.2.1 -> 10.1.1.1, 1 packet
```

Cuando resuelva problemas de una ACL por primera vez, antes de entrar en los detalles de la lógica de coincidencia, tómese el tiempo para pensar tanto en la interfaz en la que está habilitada la ACL como en la dirección del flujo de paquetes. A veces, la lógica de coincidencia es perfecta, pero la ACL se ha habilitado en la interfaz incorrecta, o en la dirección incorrecta, para que coincida con los paquetes configurados para la ACL.

Por ejemplo, la Figura 2-9 repite la misma ACL mostrada anteriormente en la Figura 2-7. La primera línea de esa ACL coincide con la dirección de host específica 10.1.1.1. Si esa ACL existe en el enrutador R2, colocar esa ACL como ACL entrante en la interfaz S0/0/1 de R2 puede funcionar, ya que los paquetes enviados por el host 10.1.1.1, en el lado izquierdo de la figura, pueden ingresar a la interfaz S0/0/1 de R2. Sin embargo, si R2 habilita ACL 1 en su interfaz F0/0, para los paquetes entrantes, la ACL nunca coincidirá con un paquete con la dirección IP de origen 10.1.1.1, porque los paquetes enviados por el host 10.1.1.1 nunca ingresarán a esa interfaz. Los paquetes enviados por 10.1.1.1 saldrán de la interfaz F0/0 de R2, pero nunca entrarán en ella, solo por la topología de red.

![](2.9.png)

### Práctica de la aplicación de ACL IP estándar

Algunos temas de CCNA, como las ACL, simplemente requieren más ejercicios y práctica que otros. Las ACL requieren que pienses en parámetros que coincidan con rangos de números, y eso, por supuesto, requiere cierto uso de matemáticas y algún uso de procesos.

Esta sección proporciona algunos problemas prácticos y consejos, desde dos perspectivas. En primer lugar, en esta sección se le pide que cree ACL estándar de una línea para que coincidan con algunos paquetes. En segundo lugar, esta sección le pide que interprete los comandos de ACL existentes para describir qué paquetes coincidirá con la ACL. Ambas habilidades son útiles para los exámenes.

### Práctica Creación de comandos de lista de acceso

En esta sección, practique familiarizarse con la sintaxis del  comando **access-list**, especialmente con la elección de la lógica de coincidencia correcta. Estas habilidades serán útiles cuando lea sobre las ACL extendidas y con nombre en el próximo capítulo.

En primer lugar, la siguiente lista resume algunos consejos importantes que se deben tener en cuenta a la hora de elegir parámetros coincidentes con cualquier  **comando de lista de acceso**:

- Para que coincida con una dirección específica, simplemente indique la dirección.
- Para hacer coincidir todas y cada una de las direcciones, utilice la  **palabra clave** any.
- Para hacer coincidir solo el primero, dos o tres octetos de una dirección, utilice las máscaras WC 0.255.255.255, 0.0.255.255 y 0.0.0.255, respectivamente. Además, haga que el parámetro source (address) tenga 0 en los octetos comodín (aquellos octetos con 255 en la máscara comodín).
- Para hacer coincidir una subred, utilice el ID de subred como origen y busque la máscara WC restando la máscara de subred DDN de 255.255.255.255.

En la Tabla 2-2 se enumeran los criterios para varios problemas de práctica. Su trabajo: cree una ACL estándar de una línea que coincida con los paquetes. Las respuestas se enumeran en la sección "Respuestas a problemas de práctica anteriores", más adelante en este capítulo.

| **Problem** | **Criteria**                                                |
| ----------- | ----------------------------------------------------------- |
| 1           | Packets from 172.16.5.4                                     |
| 2           | Packets from hosts with 192.168.6 as the first three octets |
| 3           | Packets from hosts with 192.168 as the first two octets     |
| 4           | Packets from any host                                       |
| 5           | Packets from subnet 10.1.200.0/21                           |
| 6           | Packets from subnet 10.1.200.0/27                           |
| 7           | Packets from subnet 172.20.112.0/23                         |
| 8           | Packets from subnet 172.20.112.0/26                         |
| 9           | Packets from subnet 192.168.9.64/28                         |
| 10          | Packets from subnet 192.168.9.64/30                         |
### Ingeniería inversa de ACL a rango de direcciones

En algunos casos, es posible que no esté creando su propia ACL. En su lugar, es posible que tenga que interpretar algunos  **comandos de lista de acceso** existentes  . Para responder a este tipo de preguntas en los exámenes, debe determinar el rango de direcciones IP que coinciden con una combinación particular de dirección/máscara comodín en cada instrucción de ACL.

Bajo ciertas suposiciones que son razonables para las certificaciones CCNA, calcular el rango de direcciones que coinciden con una ACL puede ser relativamente simple. Básicamente, el rango de direcciones comienza con la dirección configurada en el comando ACL. El rango de direcciones termina con la suma del campo de dirección y la máscara de comodín. Eso es todo.

Por ejemplo, con el comando **access-list 1 permit 172.16.200.0 0.0.7.255**, el extremo inferior del rango es simplemente 172.16.200.0, tomado directamente del propio comando. Luego, para encontrar el extremo superior del rango, simplemente agregue este número a la máscara de WC, de la siguiente manera:

```
 172.16.200.0
+  0. 0.  7.255
 172.16.207.255
```

Para esta última práctica, observe los  comandos **de lista de acceso existentes**  en la Tabla 2-3. En cada caso, haga una anotación sobre la dirección IP exacta, o el rango de direcciones IP, que coincida con el comando.

| **Problem** | **Commands for Which to Predict the Source Address Range** |
| ----------- | ---------------------------------------------------------- |
| 1           | **access-list 1 permit 10.7.6.5**                          |
| 2           | **access-list 2 permit 192.168.4.0 0.0.0.127**             |
| 3           | **access-list 3 permit 192.168.6.0 0.0.0.31**              |
| 4           | **access-list 4 permit 172.30.96.0 0.0.3.255**             |
| 5           | **access-list 5 permit 172.30.96.0 0.0.0.63**              |
| 6           | **access-list 6 permit 10.1.192.0 0.0.0.31**               |
| 7           | **access-list 7 permit 10.1.192.0 0.0.1.255**              |
| 8           | **access-list 8 permit 10.1.192.0 0.0.63.255**             |

Curiosamente, IOS permite al usuario de CLI escribir un  **comando access-list** en modo de configuración, e IOS potencialmente cambiará el parámetro de dirección antes de colocar el comando en el archivo running-config. Este proceso de solo encontrar el rango de direcciones que coinciden con el  `comando access-list` espera que el  comando `access-list` provenga del router, de modo que dichos cambios se hayan completado.

El cambio que IOS puede hacer con un  `comando access-list` es convertir a 0 cualquier octeto de una dirección para la cual el octeto de la máscara comodín sea 255. Por ejemplo, con una máscara comodín de 0.0.255.255, IOS ignora los dos últimos octetos. IOS espera que el campo de dirección termine con dos ceros. De lo contrario, IOS aún acepta el  `comando access-list`, pero IOS cambia los dos últimos octetos de la dirección a 0s. El ejemplo 2-5 muestra un ejemplo, donde la configuración muestra la dirección 10.1.1.1, pero la máscara comodín 0.0.255.255.

```
R2#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
R2(config)#
R2(config)# access-list 21 permit 10.1.1.1 0.0.255.255
R2(config)# ^Z
R2#
R2#show ip access-lists
Standard IP access list 21
10 permit 10.1.0.0 0.0.255.255
```

Las matemáticas para encontrar el rango de direcciones se basan en el hecho de que el comando es completamente correcto o que IOS ya ha establecido estos octetos de direcciones en 0, como se muestra en el ejemplo.