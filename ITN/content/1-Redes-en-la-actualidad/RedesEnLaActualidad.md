## Las Redes nos Conectan

Entre todos los elementos esenciales para la existencia humana, la necesidad de interactuar está justo después de la necesidad de sustentar la vida. La comunicación es casi tan importante para nosotros como el aire, el agua, los alimentos y un lugar para vivir.

En el mundo actual, estamos conectados como nunca antes gracias al uso de redes. Las personas que tienen alguna idea pueden comunicarse de manera instantánea con otras personas para hacer esas ideas realidad. Las noticias y los descubrimientos se conocen en todo el mundo en cuestión de segundos. Incluso, las personas pueden conectarse y jugar con amigos que estén del otro lado del océano y en otros continentes.
## No hay limites

Los avances en tecnologías de red son, quizá, los agentes de cambio más significativos en el mundo actual. Gracias a estos avances, podemos crear un mundo en el que las fronteras nacionales, las distancias geográficas y las limitaciones físicas se vuelven menos importantes y se convierten en obstáculos cada vez más fáciles de sortear.

Internet cambió la manera en la que se producen las interacciones sociales, comerciales, políticas y personales. La naturaleza inmediata de las comunicaciones en Internet alienta la formación de comunidades mundiales. Estas comunidades permiten una interacción social que no depende de la ubicación ni de la zona horaria.

La creación de comunidades en línea para el intercambio de ideas e información tiene el potencial de aumentar las oportunidades de productividad en todo el planeta.

La creación de la nube nos permite almacenar documentos e imágenes y acceder a ellos en cualquier lugar y en cualquier momento. Así que ya sea que estemos en un tren, en un parque o en la cima de una montaña, podemos acceder sin problemas a nuestros datos y aplicaciones desde cualquier dispositivo.
## Roles de Host

Cada computadora en una red se llama host o dispositivo final.
Los servidores son computadoras que proporcionan información a dispositivos finales:

•Servidores de correo electrónico.
•Servidores web.
•Servidores de archivos.

Los clientes son equipos que envían solicitudes a los servidores para recuperar información:

•Página web desde un servidor web.
•correo electrónico desde un servidor de correo electrónico

![[img/clienteservidor.png]]
## Punto a Punto

Es posible que un dispositivo sea un cliente y un servidor en una red Punto a Punto. Este tipo de diseño de red solo se recomienda para redes muy pequeñas.

![[img/puntoapunto.png]]
## Dispositivos finales

Un dispositivo final es el punto donde un mensaje se origina o se recibe. Los datos se originan con un dispositivo final, fluyen por la red y llegan a un dispositivo final.
## Dispositivos intermedios

Un dispositivo intermediario interconecta dispositivos finales. Los ejemplos incluyen switches, puntos de acceso inalámbrico, routers y firewalls.

La gestión de los datos a medida que fluyen a través de una red también es la función de un dispositivo intermediario, que incluye:

•Volver a generar y transmitir las señales de datos.
•Mantener información sobre qué vías existen en la red.
•Notificar a otros dispositivos los errores y las fallas de comunicación.
## Medio de red

La comunicación a través de una red se efectúa a través de un medio que permite que un mensaje viaje desde el origen hacia el destino.

## Diagrama de topología

### Física
Los diagramas de topología física ilustran la ubicación física de los dispositivos intermedios y la instalación de cables.
### Lógica
Los diagramas de topología lógica ilustran dispositivos, puertos y el esquema de direccionamiento de la red.
## LANS y WANS

Las infraestructuras de red pueden variar en gran medida en términos de:

•El tamaño del área que abarcan.
•La cantidad de usuarios conectados.
•La cantidad y los tipos de servicios disponibles.
•El área de responsabilidad

Los dos tipos de redes más comunes son los siguientes:

| LAN                                                                   | WAN                                                                  |
| --------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Interconectar dispositivos finales en un área limitada                | Interconectar LAN en amplias áreas geográficas                       |
| Administrado por una sola organización o individuo.                   | Generalmente administrado por uno o más proveedores de servicios     |
| Proporcionar ancho de banda de alta velocidad a dispositivos internos | Por lo general, proporciona enlaces de menor velocidad entre las LAN |
## Internet

Internet es una colección mundial de LAN y WAN interconectadas.

•Las redes LAN se conectan entre sí mediante redes WAN.
•Las WAN pueden usar cables de cobre, cables de fibra óptica y transmisiones inalámbricas.

Internet no pertenece a una persona o un grupo. Los siguientes grupos se desarrollaron para ayudar a mantener la estructura en Internet:

•IETF
•ICANN
•IAB
## Conexiones a Internet - Hogar y Oficinas pequeñas

| Tipo de Conexión                | Descripcion                                                                                                            |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Cable                           | Internet de alto ancho de banda, siempre encendido, ofrecido por los proveedores de servicios de televisión por cable. |
| DSL                             | Ancho de banda alto, siempre conectado, conexión a Internet que se ejecuta a través de una línea telefónica.           |
| Red Celular                     | utiliza una red de telefonía celular para conectarse a internet.                                                       |
| Satelite                        | gran beneficio para las zonas rurales sin proveedores de servicios de Internet.                                        |
| Marcacion Telefonica  - Dial-Up | Una opción económica de bajo ancho de banda que utiliza un módem.                                                      |
## Conexiones Empresariales

Las conexiones empresariales corporativas pueden requerir:

•Mayor ancho de banda
•Conexiones dedicadas
•Servicios gestionados

| Tipo de Conexion         | Descripcion                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Linea Dedicada arrendada | Estos son circuitos reservados dentro de la red del proveedor de servicios que conectan oficinas distantes con redes privadas de voz y / o datos. |
| WAN Ethernet             | Esto extiende la tecnología de acceso LAN a la WAN.                                                                                               |
| DSL                      | Business DSL está disponible en varios formatos, incluidas las líneas de suscriptor digital simétrico (SDSL).                                     |
| Satelite                 | Esto puede proporcionar una conexión cuando una solución cableada no está disponible.                                                             |
## Arquitectura de red

La arquitectura de red se refiere a las tecnologías que admiten la infraestructura que mueve los datos a través de la red.

Existen cuatro características básicas que las arquitecturas subyacentes deben abordar para cumplir con las expectativas del usuario:

***Tolerancia a fallas:***
Una red con tolerancia a fallas disminuye el impacto de una falla al limitar la cantidad de dispositivos afectados. Para la tolerancia a fallas, se necesitan varias rutas.
Las redes confiables proporcionan redundancia al implementar una red de paquetes conmutados:
- La conmutación por paquetes divide el tráfico en paquetes que se enrutan a través de una red.
- En teoría, cada paquete puede tomar una ruta diferente hacia el destino.

***Escalabilidad***
Una red escalable puede expandirse fácil y rápidamente para admitir nuevos usuarios y nuevas aplicaciones sin afectar el rendimiento de los servicios de los usuarios actuales

***Calidad de servicio (QoS)***
- La calidad de servicio (QoS) es el principal mecanismo que se utiliza para garantizar la entrega confiable de contenido a todos los usuarios.
- Con la implementación de una política de QoS, el router puede administrar más fácilmente el flujo del tráfico de voz y de datos

***Seguridad***
Existen dos tipos principales de seguridad de la red que se deben abordar: 
- Seguridad de la infraestructura de la red
- Seguridad física de los dispositivos de red
- Prevenir el acceso no autorizado a los dispositivos
- Seguridad de la información
- Protección de la información o de los datos transmitidos a través de la red

Tres objetivos de seguridad de la red:
- Confidencialidad: solo los destinatarios deseados pueden leer los datos
- Integridad: garantía de que los datos no se alteraron durante la transmisión
- Disponibilidad: garantía del acceso confiable y oportuno a los datos por