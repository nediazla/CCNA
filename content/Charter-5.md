![](img/switch-1.png)

# Protección de los  dispositivos de red

Este capítulo cubre los siguientes temas de examen:
- Fundamentos de red
	- Explicar el papel de los componentes de la red
		- Firewalls e IPS de próxima generación
- Servicios de IP
	- Configurar dispositivos de red para acceso remoto utilizando SSH
- Fundamentos de seguridad
	- Configurar el control de acceso al dispositivo utilizando contraseñas locales

 Todos los dispositivos en la red incluyen algunos métodos para que los dispositivos se comuniquen legítimamente utilizando la red. Para proteger esos dispositivos, el plan de seguridad incluirá una amplia variedad de herramientas y técnicas de mitigación.
 
Este capítulo se centra en dos necesidades de seguridad particulares en una red empresarial. Primero, el acceso a la CLI de los dispositivos de red debe protegerse. El equipo de ingeniería de red debe poder acceder a los dispositivos de forma remota, por lo que los dispositivos deben permitir el acceso remoto de SSH (y posiblemente Telnet). La primera mitad de este capítulo analiza cómo configurar las contraseñas para mantenerlas seguras y cómo filtrar los intentos de inicio de sesión en los propios dispositivos.

La segunda mitad del capítulo recurre a dos funciones de seguridad diferentes con mayor frecuencia implementadas con electrodomésticos especialmente diseñados: firewalls e IPSS. Estos dispositivos juntos monitorean el tráfico en tránsito para determinar si el tráfico es legítimo o si podría ser parte de algún exploit. Si se considera que es parte de una exploit, o si está contrario a las reglas definidas por los dispositivos, pueden descartar los mensajes, detener cualquier ataque antes de comenzar.
### Asegurando contraseñas de iOS
La mejor manera de proteger las contraseñas en los dispositivos Cisco IOS es no almacenar contraseñas en dispositivos iOS. Es decir, para cualquier función que pueda usar un servidor externo de autenticación, autorización y contabilidad (AAA), úselo. Sin embargo, es común almacenar algunas contraseñas en una configuración de enrutador o conmutador, y esta primera sección del capítulo analiza algunas de las formas de proteger esas contraseñas.

Como breve revisión, la Figura 5-1 resume una configuración de seguridad de inicio de sesión típica en un enrutador o conmutador. En la parte inferior izquierda, verá el soporte de Telnet configurado, con el uso de una contraseña solo (no se requiere nombre de usuario). A la derecha, la configuración agrega soporte para iniciar sesión con el nombre de usuario y la contraseña, admitiendo los usuarios de Telnet y SSH. La parte superior izquierda muestra el comando único requerido para definir una contraseña de habilitación de manera segura.

![](img/5.1.png)

El resto de esta primera sección analiza cómo hacer que estas contraseñas sean seguras. En particular, esta sección busca formas de evitar mantener las contraseñas de texto claro en la configuración y almacenar las contraseñas de manera que dificultan a los atacantes aprender la contraseña.
### Cifrar contraseñas de iOS anteriores con contraseña de servicio a cifrado
Algunas contraseñas de iOS de estilo anterior crean una exposición de seguridad porque las contraseñas existen en el archivo de configuración como texto claro. Estas contraseñas de texto de claro se pueden ver en versiones impresas de los archivos de configuración, en una copia de copia de seguridad del archivo de configuración almacenado en un servidor, o como se muestra en la pantalla de un ingeniero de red.

 Cisco intentó resolver este problema de texto de claro agregando un comando para cifrar esas contraseñas: el comando de configuración global de contraseña `service password-encription`. Este comando encripta las contraseñas que normalmente se mantienen como texto claro, específicamente las contraseñas de estos comandos: 
 
**password** password (console or vty mode)
**username** name **password** password (global)
**enable password** password (global)
 
Para ver cómo funciona, el ejemplo 5-1 muestra cómo el comando de contraseña de contraseña de servicio encripta la contraseña de la consola de texto de texto. El ejemplo usa el show running-config | Sección de línea Con 0 Comando tanto antes como después del cifrado; Este comando enumera solo la sección de la configuración sobre la consola.

```
Switch3# show running-config | section line con 0 
line con 0  
password cisco  
login

Switch3# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch3(config)# service password-encryption
Switch3(config)# ^Z

Switch3# show running-config | section line con 0
line con 0  
password 7 070C285F4D06  
login
```

Un examen detallado del resultado del comando `show running-config` antes y después revela tanto el efecto obvio como un nuevo concepto. El proceso de cifrado ahora oculta la contraseña original en texto claro. Además, IOS necesita una forma de indicar que el valor en el comando `password` muestra una contraseña cifrada en lugar de texto sin cifrar. IOS agrega el tipo de cifrado o codificación “7” al comando, que se refiere específicamente a contraseñas cifradas con el comando `service password-encryption`. (IOS considera que las contraseñas de texto sin cifrar son del tipo 0; algunos comandos enumeran el 0 y otros no).

Mientras que el comando global `service password-encryption` cifra las contraseñas, el comando global `no service password-encryption` no descifra inmediatamente las contraseñas a su estado de texto sin cifrar. En cambio, el proceso funciona como se muestra en la Figura 5-2. Básicamente, después de ingresar el comando `no service password-encryption`, las contraseñas permanecen cifradas hasta que las cambie.

![](img/5.2.png)

Desafortunadamente, el comando `service password-encryption` no protege muy bien las contraseñas. Armado con el valor cifrado, puede buscar en Internet y encontrar sitios con herramientas para descifrar estas contraseñas. De hecho, puede tomar la contraseña cifrada de este ejemplo, conectarla a uno de estos sitios y se descifrará como "cisco". Por lo tanto, el comando `service password-encryption` ralentizará a los curiosos, pero no detendrá a un atacante experto.

**Codificación de contraseñas habilitadas con hashes**

En los primeros días de IOS, Cisco usaba el comando global `enable password password` para definir la contraseña que los usuarios tenían que usar para alcanzar el modo habilitar (después de usar el comando `enable` EXEC). Sin embargo, como se acaba de señalar, el comando `enable password password` almacenó la contraseña como texto sin cifrar, y el comando `service password-encryption` cifró la contraseña de una manera que se podía descifrar fácilmente.

Cisco resolvió el problema de las formas débiles de almacenar la contraseña del comando global `enable password password` haciendo un reemplazo más seguro: el comando global `enable secret password`. Sin embargo, ambos comandos existen en IOS incluso hoy en día. Las siguientes páginas analizan estos dos comandos desde varios ángulos, incluidas las interacciones entre estos dos comandos, por qué el comando `enable secret` es más seguro, junto con una nota sobre algunos avances en cómo IOS protege `enable secret password`.
#### Interacciones entre habilitar contraseña y habilitar secreto
Primero, para la vida real: utilice el comando global `enable secret password` e ignore el comando global `enable password password`. Esto ha sido así durante unos 20 años.

Sin embargo, para ser completo, Cisco nunca ha eliminado el comando `enable password`, mucho más débil, de IOS. Entonces, en un solo conmutador (o enrutador), puede configurar uno u otro,

#### Hacer que Enable Secret sea verdaderamente secreto con un hash
El comando  `enable secret` protege el valor de la contraseña al ni siquiera almacenar la contraseña en texto claro en la configuración. Sin embargo, esa frase puede causarle un poco de confusión: si el enrutador o conmutador no recuerda la contraseña en texto claro, ¿cómo puede saber el conmutador que el usuario escribió la contraseña correcta después de usar el comando `enable`

Primero, de forma predeterminada, IOS utiliza una función hash llamada Message Digest 5 (MD5) para almacenar un valor alternativo en la configuración, en lugar de la contraseña de texto sin cifrar. Piense en MD5 como una fórmula matemática bastante compleja. Además, esta fórmula se elige de modo que, incluso si conoce el resultado exacto de la fórmula (es decir, el resultado después de introducir la contraseña en texto sin cifrar a través de la fórmula como entrada), sea computacionalmente difícil calcular la contraseña en texto sin cifrar original. . La Figura 5-3 muestra las ideas principales:

![](img/5.3.png)

Entonces, si la contraseña original en texto claro no se puede volver a crear, ¿cómo puede un conmutador o enrutador usarla para compararla con la contraseña en texto claro escrita por el usuario? La respuesta depende de otro hecho sobre estos hashes de seguridad como MD5: cada entrada de texto sin cifrar da como resultado un resultado único de la fórmula matemática.

El comando `enable secret fred` genera un hash MD5. Si un usuario escribe **fred** al intentar ingresar al modo habilitar, IOS ejecutará MD5 con ese valor y obtendrá el mismo hash MD5 que aparece en el comando `enable secret`, por lo que IOS permite al usuario acceder a habilitar modo. Si el usuario escribiera cualquier otro valor además de **fred**, IOS calcularía un hash MD5 diferente al valor almacenado con el comando `enable secret`, y IOS rechazaría el intento de ese usuario de alcanzar el modo de habilitación.

Sabiendo ese hecho, el conmutador puede hacer una comparación cuando un usuario escribe una contraseña después de usar el comando EXEC `enable` de la siguiente manera:

| **Step 1.** | IOS calcula el hash MD5 de la contraseña en el comando `enable secret` y almacena el hash de la contraseña en la configuración.                                                                                                     |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Step 2.** | Cuando el usuario escribe el comando `enable` para alcanzar el modo habilitar, una contraseña que debe compararse con ese comando de configuración, IOS codifica la contraseña en texto sin cifrar tal como la escribió el usuario. |
| **Step 3.** | IOS compara los dos valores hash: si son iguales, la contraseña ingresada por el usuario debe ser la misma que la contraseña configurada.                                                                                           |

Como resultado, IOS puede almacenar el hash de la contraseña pero nunca almacenar la contraseña en texto claro; sin embargo, aún puede determinar si el usuario escribió la misma contraseña.

Los conmutadores y enrutadores ya utilizan la lógica que se describe aquí, pero puede ver la evidencia observando la configuración del conmutador. El ejemplo 5-2 muestra la creación del comando **enable secret**, con algunos detalles relacionados. Este ejemplo muestra el valor almacenado (en hash) como se revela en la salida del comando "show running-configuration". Ese resultado también muestra que IOS cambió el comando `enable secret fred` para enumerar el tipo de cifrado 5 (lo que significa que la contraseña enumerada es en realidad un hash MD5 de la contraseña de texto sin cifrar). La larga cadena de texto galimatías es el hash, lo que impide que otros lean la contraseña.

```
Switch3(config)# enable secret fred
Switch3(config)# ^Z 

Switch3# show running-config | include enable secret

enable secret 5 $1$ZGMA$e8cmvkz4UjiJhVp7.maLE1

Switch3# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Switch3(config)# no enable secret
Switch3(config)# ^Z
```

El final del ejemplo también muestra un punto importante sobre la eliminación de la contraseña secreta de habilitación: después de estar en el modo `enable`, puede eliminar la contraseña secreta de habilitación usando el comando `no enable secret`, sin siquiera tener que ingresar el valor de la contraseña. También puede sobrescribir la contraseña anterior simplemente repitiendo el comando enable secret. Pero no puede ver la contraseña original en texto claro.
### Hashes mejorados para Enable Secret de Cisco
El uso de cualquier función hash para codificar contraseñas depende de varias características clave de la función hash particular. En particular, cada valor de entrada posible debe dar como resultado un único valor hash, de modo que cuando los usuarios escriban una contraseña, solo un valor de contraseña coincida con cada valor hash. Además, el algoritmo hash debe resultar en matemáticas computacionalmente difíciles para calcular la contraseña de texto claro basada en el valor hash para disuadir a los atacantes.

El algoritmo hash MD5 existe desde hace unos 30 años. A lo largo de esos años, las computadoras se han vuelto mucho más rápidas y los investigadores han encontrado formas creativas de atacar el algoritmo MD5, haciendo que MD5 sea menos difícil de descifrar. Es decir, a alguien que viera su configuración en ejecución le resultaría más fácil recrear sus contraseñas secretas en texto claro que en los primeros años de MD5.

Estos hechos no pretenden decir que MD5 sea malo, pero como muchas funciones criptográficas anteriores a MD5, se han logrado avances y se necesitaban nuevas funciones. Para brindar opciones más recientes que crearían un desafío mucho mayor para los atacantes, Cisco agregó dos hashes adicionales en la década de 2010, como se indica en la Figura 5-4.

![](img/5.4.png)

IOS ahora admite dos tipos de algoritmos alternativos en las imágenes de IOS de enrutador y conmutador más recientes. Ambos usan un hash SHA-256 en lugar de MD5, pero con dos opciones más nuevas, cada una de las cuales tiene algunas diferencias en los detalles de cómo cada algoritmo usa SHA-256. La Tabla 5-2 muestra la configuración de los tres tipos de algoritmos en el comando `enable secret`.

| **Command**                                        | **Type** | **Algorithm** |
| -------------------------------------------------- | -------- | ------------- |
| **enable [algorithm-type md5] secret** _password_  | 5        | MD5           |
| **enable algorithm-type sha256 secret** _password_ | 8        | SHA-256       |
| **enable algorithm-type scrypt secret** _password_ | 9        | SHA-256       |

El ejemplo 5-3 muestra el cambio del comando `enable secret` de MD5 al algoritmo scrypt. Es de destacar que el ejemplo muestra que solo debe existir un comando `enable secret` entre esos tres comandos en la Tabla 5-2. Básicamente, si configura otro comando `enable secret` con un tipo de algoritmo diferente, ese comando reemplaza cualquier comando `enable secret` existente.

```
R1# show running-config | include enable
enable secret 5 $1$ZSYj$725dBZmLUJ0nx8gFPTtTv0
R1# configure terminal
Enter configuration commands, one per line. End with CNTL/Z. 

R1(config)# enable algorithm-type scrypt secret mypass1
R1(config)# ^Z

R1# 
R1# show running-config | include enable
enable secret 9 $9$II/EeKiRW91uxE$fwYuOE5EHoii16AWv2wSywkLJ/KNeGj8uK/24B0TVU6 
R1#
```

Siguiendo el proceso que se muestra en el ejemplo, el primer comando confirma que el comando `enable secret` actual usa codificación tipo 5, lo que significa que usa MD5. En segundo lugar, el usuario configura la contraseña utilizando un algoritmo tipo scrypt. El último comando confirma que solo existe un comando `enable secret` en la configuración, ahora con codificación tipo 9.
### Codificación de contraseñas para nombres de usuario locales
Cisco agregó el comando `enable secret` en la década de 1990 para superar los problemas con el comando `enable password`. Los comandos `username`, `password` y `user secret` tienen un historial similar. Originalmente, IOS admitía el comando `username user password password`, un comando que tenía los mismos problemas de ser una contraseña de texto sin cifrar o un valor mal cifrado (con la función de cifrado de contraseña del servicio). Muchos años después, Cisco agregó el comando global `username user secret password`, que codificaba la contraseña como un hash MD5, y Cisco agregó soporte para los hashes SHA-256 más nuevos más adelante.

Hoy en día, se prefiere el comando `username secret` es preferible sobre el comando `username password`;  sin embargo, IOS no utiliza la misma lógica para el comando `username` que para permitir que los comandos `enable secret` y `enable password` existan en la misma configuración. IOS permite
- Solo un comando de nombre de usuario para un nombre de usuario determinado: ya sea un comando `username name password password` o un comando `username name secrte password`.
- Una combinación de comandos (`username password` y `username secret`) en el mismo enrutador o conmutador (para diferentes nombres de usuario)

Debe utilizar el comando `username secret` en lugar del comando `username password` cuando sea posible. Sin embargo, tenga en cuenta que algunas funciones de IOS requieren que el enrutador conozca una contraseña de texto sin cifrar mediante el comando `username` (por ejemplo, cuando se realizan algunos métodos de autenticación comunes para enlaces serie llamados PAP y CHAP). En esos casos, aún necesitarás usar el comando `username` y `password`

Como se mencionó, las versiones más recientes de IOS tanto en conmutadores como en enrutadores utilizan opciones de codificación adicionales más allá de MD5, tal como lo admite el comando `enable secret`. La Tabla 5-3 muestra la sintaxis de esas tres opciones en el comando `username`, con la opción MD5 mostrada como una opción porque es la opción predeterminada utilizada con el comando `username secret`.

| **Command**                                                        | **Type** | **Algorithm** |
| ------------------------------------------------------------------ | -------- | ------------- |
| **username** _name_ [**algorithm-type md5**] **secret** _password_ | 5        | MD5           |
| **username** _name_ **algorithm-type sha256 secret** _password_    | 8        | SHA-256       |
| **username** _name_ **algorithm-type scrypt secret** _password_    | 9        | SHA-256       |

### Controlar los ataques a contraseñas con ACL
Los atacantes pueden intentar iniciar sesión repetidamente en sus dispositivos de red para obtener acceso, pero IOS tiene una función que usa ACL para evitar que el atacante vea siquiera una solicitud de contraseña. Cuando un usuario externo se conecta a un enrutador o conmutador mediante Telnet o SSH, IOS utiliza una línea vty para representar la conexión de ese usuario. IOS puede aplicar una ACL a las líneas vty, filtrando las direcciones que pueden hacer telnet o SSH en el enrutador o conmutador. Si se filtra, el usuario nunca ve un mensaje de inicio de sesión.

Por ejemplo, imagine que todos los dispositivos del personal de ingeniería de redes se conectan a la subred 10.1.1.0/24. La política de seguridad establece que solo el personal de ingeniería de redes debe poder realizar telnet o SSH en cualquiera de los enrutadores Cisco de una red. En tal caso, la configuración que se muestra en el Ejemplo 5-4 podría usarse en cada enrutador para denegar el acceso desde direcciones IP que no estén en esa subred.

```
line vty 0 4  
login  
password cisco  
access-class 3 in 
! 
! Next command is a global command that matches IPv4 packets with 
! a source address that begins with 10.1.1.  
access-list 3 permit 10.1.1.0 0.0.0.255
```

El comando `access-class` se refiere a la lógica de coincidencia en la `access-list 3`. La palabra clave in se refiere a conexiones Telnet y SSH en este enrutador; en otras palabras, personas que realizan telnet en este enrutador. Tal como está configurado, ACL 3 verifica la dirección IP de origen de los paquetes para detectar conexiones Telnet entrantes.

IOS también admite el uso de ACL para filtrar conexiones Telnet y SSH salientes. Por ejemplo, considere un usuario que usa Telnet o SSH por primera vez para conectarse a la CLI y ahora se encuentra en modo de usuario o habilitado. Con un filtro vty saliente, IOS aplicará la lógica ACL si el usuario intenta los comandos telnet o ssh para conectarse desde el dispositivo local a otro dispositivo.

Para configurar una ACL de VTY saliente, utilice el comando `access-class` `acl out` en el modo de configuración de VTY. Una vez configurado, el enrutador filtra cualquier intento realizado por los usuarios actuales de vty de utilizar los comandos telnet y ssh para iniciar nuevas conexiones a otros dispositivos.

De las dos opciones (proteger las conexiones entrantes y salientes), proteger las conexiones entrantes, con diferencia, la más importante y la más común. Sin embargo, para ser completos, las ACL de VTY salientes tienen una característica sorprendentemente extraña en la forma en que usan la ACL. Cuando se utiliza la palabra clave `out`, la ACL de IP estándar enumerada en el comando `access-class` en realidad mira la dirección IP de destino y no la de origen. Es decir, filtra según el dispositivo al que intenta conectarse el comando telnet o ssh.
### Firewalls y sistemas de prevención de intrusiones
El siguiente tema examina las funciones de un par de tipos diferentes de dispositivos de red: firewalls y sistemas de prevención de intrusiones (IPS). Ambos dispositivos funcionan para proteger redes, pero con objetivos y enfoques ligeramente diferentes.

Tradicionalmente, un firewall se ubica en la ruta de reenvío de todos los paquetes para que luego pueda elegir qué paquetes descartar y cuáles permitir. Al hacerlo, el firewall protege la red de diferentes tipos de problemas al permitir que solo los tipos de tráfico previstos entren y salgan de la red. De hecho, en su forma más básica, los firewalls realizan el mismo tipo de trabajo que los enrutadores con las ACL, pero los firewalls pueden realizar esa función de filtrado de paquetes con muchas más opciones, además de realizar otras tareas de seguridad.

La Figura 5-5 muestra un diseño de red típico para un sitio que utiliza un firewall físico. La figura muestra un firewall, como el firewall Cisco Adaptive Security Appliance (ASA), conectado a un enrutador Cisco, que a su vez se conecta a Internet. Todo el tráfico empresarial que vaya hacia o desde Internet se enviaría a través del firewall. El firewall consideraría sus reglas y elegiría para cada paquete si se le debe permitir el paso.

![](img/5.5.png)

Aunque los firewalls tienen algunas características similares a las de un enrutador (como reenvío y filtrado de paquetes), brindan características de seguridad mucho más avanzadas que un enrutador tradicional. Por ejemplo, la mayoría de los firewalls pueden utilizar los siguientes tipos de lógica para elegir si descartar o permitir un paquete:
- Al igual que las ACL de IP del enrutador, haga coincidir las direcciones IP de origen y destino
- Al igual que las ACL de IP del enrutador, identifique las aplicaciones haciendo coincidir su TCP estático conocido y Puertos UDP
- Observe los flujos de la capa de aplicación para saber qué puertos TCP y UDP adicionales utiliza un flujo en particular y filtre según esos puertos.
- Haga coincidir el texto en el URL de una solicitud HTTP (es decir, observe y compare el contenido de lo que a menudo se denomina dirección web) y haga coincidir patrones para decidir si se permite o deniega la descarga de la página web identificada por ese URL.
- Mantenga la información de estado almacenando información sobre cada paquete y tome decisiones sobre el filtrado de paquetes futuros en función de la información de estado histórica (lo que se denomina inspección de estado o firewall con estado).

La función de firewall con estado proporciona los medios para prevenir una variedad de ataques y es una de las diferencias más obvias entre el procesamiento ACL de un enrutador y el filtrado de seguridad mediante un firewall. Los enrutadores deben dedicar el menor tiempo posible a procesar cada paquete para que los paquetes experimenten un pequeño retraso al pasar a través del enrutador. El enrutador no puede tomarse el tiempo para recopilar información sobre un paquete y luego, para paquetes futuros, considerar alguna información de estado guardada sobre paquetes anteriores al tomar una decisión de filtrado. Debido a que se centran en la seguridad de la red, los firewalls guardan cierta información sobre los paquetes y pueden considerar esa información para futuras decisiones de filtrado.

Como ejemplo de los beneficios de utilizar un firewall con estado, considere un simple ataque de denegación de servicio (DoS). Un atacante puede realizar este tipo de ataque contra un servidor web utilizando herramientas que crean (o comienzan a crear) un gran volumen de conexiones TCP al servidor. El firewall podría permitir conexiones TCP a ese servidor normalmente, pero imagine que el servidor normalmente podría recibir 10 nuevas conexiones TCP por segundo en condiciones normales y 100 por segundo en los momentos de mayor actividad. Un ataque DoS podría intentar miles o más de conexiones TCP por segundo, aumentando el uso de CPU y RAM en el servidor y, finalmente, sobrecargando el servidor hasta el punto de que no pueda atender a usuarios legítimos.
Un firewall con estado podría rastrear la cantidad de conexiones TCP por segundo (es decir, registrar información de estado basada en paquetes anteriores), incluida la cantidad de solicitudes de conexión TCP desde cada dirección IP de cliente a cada dirección de servidor. El firewall con estado podría detectar una gran cantidad de conexiones TCP, verificar su información de estado y luego notar que la cantidad de solicitudes es muy grande desde una pequeña cantidad de clientes a ese servidor en particular, lo cual es típico de algunos tipos de ataques DoS. El firewall con estado podría entonces comenzar a filtrar esos paquetes, ayudando al servidor web a sobrevivir al ataque, mientras que un firewall sin estado o una ACL de enrutador no habrían tenido la información histórica del estado para darse cuenta de que se estaba produciendo un ataque DoS.
### Zonas de seguridad
Los firewalls no sólo filtran paquetes, sino que también prestan mucha atención a qué host inicia las comunicaciones. Ese concepto es más obvio con TCP como protocolo de capa de transporte, donde el cliente inicia la conexión TCP enviando un segmento TCP que establece solo el bit SYN (como se ve en la Figura 1-5 en el Capítulo 1).

Los firewalls utilizan una lógica que considera qué host inició una conexión TCP observando estos segmentos TCP iniciales. Para ver la importancia de quién inicia las conexiones, piense en una red empresarial típica con conexión a Internet, como se muestra en la Figura 5-6. La empresa tiene usuarios dentro de la empresa que abren navegadores web e inician conexiones a servidores web a través de Internet. Sin embargo, al tener una conexión a Internet que funcione, esa misma empresa abre la posibilidad de que un atacante intente crear una conexión TCP con los servidores web internos de la empresa utilizados para el procesamiento de nóminas. Eso sí, la empresa no quiere que internautas aleatorios o atacantes puedan conectarse a su servidor.

Los firewalls utilizan el concepto de zonas de seguridad (también llamadas zona para abreviar) al definir qué hosts pueden iniciar nuevas conexiones. El firewall tiene reglas, y esas reglas definen qué host puede iniciar conexiones de una zona a otra. Además, al utilizar zonas, un firewall puede colocar varias interfaces en la misma zona, en casos en los que a varias interfaces se les deben aplicar las mismas reglas de seguridad. La Figura 5-7 muestra la idea de que la parte interior de la empresa se considera en una zona separada en comparación con las interfaces conectadas a Internet.

![](img/5.6.png)

![](img/5.7.png)

La regla de firewall más básica cuando se usan dos zonas como la Figura 5-7 se reduce a esta lógica:
Permita que los hosts de la zona interior inicien conexiones con los hosts de la zona exterior, para un conjunto predefinido de puertos seguros y conocidos (como el puerto HTTP 80, por ejemplo).

Tenga en cuenta que con esta regla simple, se permite el tráfico correcto mientras se filtra el tráfico no deseado de forma predeterminada. Los cortafuegos normalmente no permiten todo el tráfico a menos que una regla permita específicamente el paquete. Entonces, con esta simple regla para permitir que los usuarios internos inicien conexiones a la zona externa, y solo eso, el firewall también evita que los usuarios externos inicien conexiones con los hosts internos.

La mayoría de las empresas tienen una zona interior y exterior, así como una zona especial llamada zona desmilitarizada (DMZ). Aunque el nombre DMZ proviene del mundo real, se ha utilizado en TI durante décadas para referirse a una zona de seguridad de firewall utilizada para colocar servidores que deben estar disponibles para el uso de los usuarios en la Internet pública. Por ejemplo, la Figura 5-8 muestra un diseño típico de borde de Internet, con la adición de un par de servidores web en su DMZ conectados a través del firewall. Luego, el firewall necesita otra regla que permita a los usuarios de la zona exterior (es decir, los usuarios de Internet) iniciar conexiones a esos servidores web en la DMZ. Al separar esos servidores web en la DMZ, lejos del resto de la empresa, la empresa puede evitar que los usuarios de Internet intenten conectarse a los dispositivos internos en la zona interior, evitando muchos tipos de ataques.

![](img/5.8.png)

### Sistemas de prevención de intrusiones (IPS)
Tradicionalmente, un firewall funciona con un conjunto de reglas configuradas por el usuario sobre dónde se debe permitir que fluyan los paquetes en una red. El firewall debe ubicarse en la ruta de los paquetes para poder filtrarlos, redirigirlos para su recopilación y análisis posterior, o dejarlos continuar hacia su destino.

Un sistema de prevención de intrusiones (IPS) tradicional puede ubicarse en la ruta que toman los paquetes a través de la red y puede filtrar paquetes, pero toma sus decisiones con una lógica diferente. El IPS primero descarga una base de datos de firmas de exploits. Cada firma define diferentes valores de campo de encabezado que se encuentran en secuencias de paquetes utilizados por diferentes exploits. Luego, el IPS puede examinar los paquetes, compararlos con las firmas de exploits conocidos y detectar cuándo los paquetes pueden ser parte de un exploit conocido. Una vez identificado, el IPS puede registrar el evento, descartar paquetes o incluso redirigir los paquetes a otra aplicación de seguridad para su posterior examen.

Un IPS tradicional se diferencia de los firewalls en que en lugar de que un ingeniero de la empresa defina reglas para esa empresa en función de las aplicaciones (por número de puerto) y zonas, el IPS aplica la lógica basada en firmas proporcionadas principalmente por el proveedor del IPS. Esas firmas buscan este tipo de ataques:
- Dos
- DDoS
- Worms
- Viruses

Para cumplir su misión, el IPS necesita descargar y seguir actualizando su base de datos de firmas. Los expertos en seguridad trabajan para crear las firmas. Luego, el IPS debe descargar la base de datos de firmas de exploits y seguir descargando actualizaciones a lo largo del tiempo, como se muestra en la Figura 5-9.

![](img/5.9.png)

Por ejemplo, piense en lo que sucede cuando se crea un virus informático completamente nuevo. Los productos de seguridad basados ​​en host, como el software antivirus, deben instalarse en las computadoras dentro de la empresa. Estas herramientas utilizan un modelo similar al IPS y mantienen una base de datos actualizada de firmas de virus. Las firmas pueden buscar patrones en cómo un virus informático podría almacenarse dentro de archivos en la computadora, o en archivos enviados a la computadora a través de correo electrónico o navegadores web. Pero habrá un desfase de tiempo entre el día en que se descubrió el virus (llamados ataques de día cero) y el momento en que los investigadores desarrollaron una firma de virus, cambiaron su base de datos y dieron tiempo a todos los hosts para actualizar su software antivirus. Los anfitriones están en riesgo durante este lapso de tiempo.

El IPS brinda un servicio gratuito de prevención de virus. Los investigadores buscarán formas en que un IPS pueda reconocer el mismo virus mientras se desplaza a través de la red con nuevas firmas de IPS; por ejemplo, buscando paquetes con un puerto particular y una cadena hexadecimal particular en la carga útil de la aplicación. Una vez desarrollados, los dispositivos IPS de la red deben actualizarse con la nueva base de datos de firmas, protegiéndolos contra ese virus. Tanto la protección basada en host como la basada en IPS desempeñan un papel importante, pero el hecho de que un IPS proteja secciones de una red significa que el IPS a veces puede reaccionar más rápidamente ante nuevas amenazas para proteger los hosts.
### Cisco Next-Generation Firewalls
A mediados de la década de 2010, Cisco y algunos de sus competidores comenzaron a utilizar el término próxima generación cuando hablaban de sus productos de seguridad para enfatizar algunas de las características más nuevas. En resumen, un firewall de próxima generación (NGFW) y un IPS de próxima generación (NGIPS) son los productos IPS y firewall actuales de Cisco.

Sin embargo, el uso del término próxima generación va mucho más allá de una simple etiqueta de marketing: el término enfatiza algunos cambios y mejoras importantes a lo largo de los años. La industria de la seguridad ve ciclos interminables de nuevos ataques seguidos de nuevas soluciones, y algunas soluciones requieren nuevas características del producto o incluso nuevos productos. Algunos de los cambios que han requerido nuevas características de seguridad incluyen la proliferación de dispositivos móviles (dispositivos que salen de la empresa, se conectan a Internet y regresan a la empresa), lo que crea un nivel de riesgo completamente nuevo. Además, ninguna función o dispositivo de seguridad (firewall, IPS, antimalware) por sí solo puede detener algunas amenazas, por lo que las herramientas de próxima generación deben poder trabajar mejor juntas para brindar soluciones. En resumen, los productos de próxima generación tienen características realmente útiles que no se encuentran en sus productos predecesores.

En cuanto a los productos de Cisco, durante muchos años Cisco denominó sus firewalls como Cisco Adaptive Security Appliance (ASA). Alrededor de 2013, Cisco adquirió Sourcefire, una empresa de productos de seguridad. Muchas de las características del firewall (e IPS) de próxima generación provienen del software adquirido a través de esa adquisición. A partir de 2019, todos los firewalls vendidos actualmente por Cisco tienen nombres que evocan recuerdos de la adquisición de Sourcefire, y la mayor parte de la línea de productos de firewall se denomina firewalls Cisco Firepower.

Por supuesto, un NGFW todavía realiza las funciones tradicionales de un firewall, como el filtrado de estado al comparar campos en los encabezados IP, TCP y UDP, y usar zonas de seguridad al definir reglas de firewall. Para brindar una idea de algunas de las características más nuevas de próxima generación, considere el desafío de hacer coincidir paquetes con puertos:

1. Cada aplicación basada en IP debe utilizar un puerto conocido.
2. Los atacantes saben que los firewalls filtrarán los puertos más conocidos de las sesiones iniciadas desde la zona exterior hacia la zona interior (consulte la Figura 5-8).
3. Los atacantes utilizan el escaneo de puertos para encontrar cualquier puerto que el firewall de una empresa permita en este momento.
4. Los atacantes intentan utilizar un protocolo de su elección (por ejemplo, HTTP), pero con el puerto no estándar encontrado mediante el escaneo de puertos como una forma de intentar conectarse a hosts dentro de la empresa.

La secuencia enumera un resumen de algunos de los pasos que los atacantes deben seguir, pero no enumera todas las tareas. Sin embargo, incluso a esta profundidad, se puede ver cómo los atacantes pueden encontrar una manera de enviar paquetes más allá del firewall corporativo.

¿La solución? Un firewall de próxima generación que analiza los datos de la capa de aplicación para identificar la aplicación en lugar de depender de los números de puerto TCP y UDP utilizados. Cisco realiza su inspección profunda de paquetes utilizando una función llamada Visibilidad y control de aplicaciones (AVC). Cisco AVC puede identificar muchas aplicaciones en función de los datos enviados (encabezados de capa de aplicación más estructuras de datos de aplicación mucho más allá de los encabezados TCP y UDP). Cuando se utiliza con un Cisco NGFW, en lugar de hacer coincidir los números de puerto, el firewall coincide con la aplicación, derrotando ataques como el que se acaba de describir.

La siguiente lista menciona algunas de las características de un NGFW. Tenga en cuenta que, si bien NGFW es un término útil, la línea entre un firewall tradicional y un firewall de próxima generación puede ser un poco borrosa, ya que los términos describen productos que han pasado por cambios repetidos durante largos períodos de tiempo. Sin embargo, esta lista resume algunos de los puntos clave:
- **Firewall tradicional**: un NGFW realiza funciones de firewall tradicionales, como filtrado de firewall con estado, NAT/PAT y terminación de VPN.
- **Visibilidad y control de aplicaciones (AVC)**: esta función analiza en profundidad los datos de la capa de aplicación para identificar la aplicación. Por ejemplo, puede identificar la aplicación basándose en los datos, en lugar del número de puerto, para defenderse de ataques que utilizan números de puerto aleatorios.
- **Protección avanzada contra malware**: las plataformas NGFW ejecutan múltiples servicios de seguridad, no solo como una plataforma para ejecutar un servicio separado, sino para una mejor integración de funciones. Se puede ejecutar una función antimalware basada en red en el propio firewall, bloqueando las transferencias de archivos que instalarían malware y guardando copias de los archivos para su posterior análisis.
- **Filtrado de URL**: esta función examina las URL en cada solicitud web, las clasifica y filtra o limita el tráfico según las reglas. El grupo de seguridad de Cisco Talos monitorea y crea puntajes de reputación para cada dominio conocido en Internet, y el filtrado de URL puede utilizar esos puntajes en su decisión de categorizar, filtrar o limitar tarifas.
- **NGIPS**: Los productos Cisco NGFW también pueden ejecutar su función NGIPS junto con el cortafuegos.
Tenga en cuenta que para cualquiera de los servicios que se benefician de estar en la misma ruta que atraviesan los paquetes, como un firewall, tiene sentido que con el tiempo esas funciones puedan migrar para ejecutarse en el mismo producto. Entonces, cuando el diseño necesita un firewall e IPS en la misma ubicación de la red, estos productos NGFW pueden ejecutar la función NGIPS como se muestra en el dispositivo combinado en la Figura 5-10.

![](img/5.10.png)

### Cisco Next-Generation IPS
Los productos Cisco IPS de próxima generación (NGIPS) han seguido un camino similar al de los productos Cisco NGFW. Cisco agregó por primera vez funciones NGIPS principalmente a través de su adquisición de Sourcefire, y los productos Cisco IPS ahora actuales también usan el nombre Firepower. De hecho, como línea de productos, los productos de hardware NGFW y NGIPS son los mismos productos, con la capacidad de ejecutar tanto NGFW como NGIPS.

Al igual que el NGFW, el NGIPS añade funciones a un IPS tradicional. Por ejemplo, uno de los mayores problemas con un IPS tradicional viene con el volumen de eventos de seguridad registrados por el IPS. Por ejemplo:
1. Un IPS compara la base de datos de firmas, que enumera todos los exploits conocidos, con todos los mensajes.
2. Genera eventos, a menudo mucho más de los que el personal de seguridad puede leer.
3. El personal debe filtrar mentalmente los acontecimientos para encontrar la proverbial aguja en el pajar, algo que sólo es posible gracias al trabajo duro, la vasta experiencia y la voluntad de cavar.

Un NGIPS ayuda con este problema de varias maneras. Primero, un NGIPS examina el contexto recopilando datos de todos los hosts y los usuarios de esos hosts. El NGIPS conocerá el sistema operativo, los niveles de revisión del software, qué aplicaciones se están ejecutando, los puertos abiertos, los protocolos de transporte y los números de puerto en uso, etc. Armado con esos datos, el NGIPS puede tomar decisiones mucho más inteligentes sobre qué eventos registrar.

Por ejemplo, considere un NGIPS colocado en una red para proteger una LAN de campus donde se conectan los usuarios finales, pero no existe ningún centro de datos en esa parte de la red. Además, todas las PC ejecutan Windows, y posiblemente la misma versión, según la política corporativa. La base de datos de firmas incluye firmas para exploits de hosts Linux, Mac, versiones de Windows inexistentes en esa parte de la red y exploits que se aplican a aplicaciones de servidor que no se ejecutan en esos hosts. Después de recopilar esos datos, un NGIPS puede sugerir restar importancia a las comprobaciones de exploits que no se aplican a esos puntos finales, dedicar más tiempo y centrarse en los eventos que podrían ocurrir, reduciendo en gran medida la cantidad de eventos registrados.
La siguiente lista menciona algunas de las características de Cisco NGIPS:

- **IPS tradicional**: un NGIPS realiza funciones de IPS tradicionales, como usar firmas de explotación para comparar flujos de paquetes, crear un registro de eventos y posiblemente descartar y/o redirigir paquetes.
- **Visibilidad y control de aplicaciones (AVC)**: al igual que con los NGFW, un NGIPS tiene la capacidad de analizar en profundidad los datos de la capa de aplicación para identificar la aplicación.
- **Conciencia contextual**: las plataformas NGFW recopilan datos de los hosts: sistema operativo, versión/nivel de software, parches aplicados, aplicaciones en ejecución, puertos abiertos, aplicaciones que actualmente envían datos, etc. Esos hechos informan al NGIPS sobre las vulnerabilidades a menudo más limitadas en una parte de la red para que el NGIPS pueda centrarse en las vulnerabilidades reales y al mismo tiempo reducir en gran medida la cantidad de eventos registrados.
- **Filtrado basado en reputación**: el grupo de inteligencia de seguridad de Cisco Talos investiga diariamente las amenazas a la seguridad y genera los datos utilizados por el portafolio de seguridad de Cisco. Parte de esos datos identifica a los malos actores conocidos, según la dirección IP, el dominio, el nombre o incluso una URL específica, con una puntuación de reputación para cada uno. Un Cisco NGIPS puede realizar un filtrado basado en la reputación, teniendo en cuenta las puntuaciones.
- **Nivel de impacto del evento**: el personal de seguridad necesita evaluar los eventos registrados, por lo que un NGIPS proporciona una evaluación basada en los niveles de impacto, con caracterizaciones del impacto si un evento es realmente algún tipo de ataque.

Si desea aprender un poco más sobre estos temas por su propio interés, permítame recomendarle un par de recursos. Primero, consulte los artículos y publicaciones de blogs de Cisco Talos Intelligence Group (www.talosintelligence.com). La organización Cisco Talos investiga problemas de seguridad en todo el mundo en todo el espectro de productos de seguridad. 

### Comandos de Referencia

| Comando          | Modo / Propósito / Descripción                                 |
| ---------------- | -------------------------------------------------------------- |
| `line console 0` | Command that changes the context to console configuration mode |
|                  |                                                                |
