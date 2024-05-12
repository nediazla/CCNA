## ¿Qué aprendí en este módulo?

### Configurar propiedades iniciales del router

Las siguientes tareas deben completarse al configurar la configuración inicial en un enrutador.

1. Configure el nombre del dispositivo.
2. Proteja el modo EXEC con privilegios.
3. Proteger el modo EXEC de usuario
4. Proteger el acceso remoto por Telnet y SSH
5. Proteja todas las contraseñas del archivo de configuración.
6. Proporcione una notificación legal.
7. Guarde la configuración.

### Configurar interfaces

Para que se pueda llegar a los routers, se debe configurar la interfaz de router. El router Cisco ISR 4321 está equipado con dos interfaces Gigabit Ethernet: GigabitEthernet 0/0/0 (G0 / 0/0) y GigabitEthernet 0/0/1 (G0 / 0/1). Las tareas para configurar una interfaz de router son muy similares a un SVI de administración en un switch. Mediante el comando `no shutdown` se activa la interfaz. La interfaz también debe estar conectada a otro dispositivo , como un switch o un router, para que la capa física se active. Existen varios comandos que se pueden utilizar para verificar la configuración de interfaz. `show ip interface brief` y `show ipv6 interface brief`, el `show ip route` y`show ipv6 route`, tambien `show interfaces`, `show ip interface` y `show ipv6 interface.`

### Configurar el Gateway Predeterminado

Para que un terminal se comunique a través de la red, se debe configurar con la información de dirección IP correcta, incluida la dirección de gateway predeterminado. En general, la dirección de gateway predeterminado es la dirección de la interfaz de router conectada a la red local del host. La dirección IP del dispositivo host y la dirección de interfaz de router deben estar en la misma red. Para conectarse y administrar un switch a través de una red IP local, debe tener configurada una interfaz virtual de switch (SVI). El SVI se configura con una dirección IPv4 y una máscara de subred en la LAN local. El switch también debe tener una dirección de puerta de enlace predeterminada configurada para administrar el switch de forma remota desde otra red. Para configurar un gateway predeterminado en un switch, use el comando `ip default-gateway ip-address` de configuración global Use la dirección IPv4 de la interfaz del enrutador local que está conectada al conmutador.