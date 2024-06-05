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

