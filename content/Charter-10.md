![](img/switch-1.png)

# Traducción de Direcciones de Red

Este capítulo cubre los siguientes temas del examen:
- Servicios IP
	- Configurar y verificar la NAT de origen interna utilizando estática y grupos

Este capítulo examina una parte muy popular e importante de las redes empresariales y de pequeñas oficinas/oficinas domésticas (SOHO): la traducción de direcciones de red o NAT. NAT ayudó a resolver un gran problema con IPv4: el espacio de direcciones IPv4 se habría consumido por completo a mediados de la década de 1990. Una vez consumido, Internet no pudo seguir creciendo, lo que habría frenado significativamente el desarrollo de Internet.
### Perspectivas sobre la escalabilidad de las direcciones IPv4
El diseño original de Internet requería que cada organización solicitara y recibiera uno o más números de red IPv4 con clase registrados. Las personas que administraron el programa se aseguraron de que ninguna de las redes IP fuera reutilizada. Mientras cada organización utilice sólo direcciones IP dentro de sus propios números de red registrados, las direcciones IP nunca se duplicarán y el enrutamiento IP podría funcionar bien.

Conectarse a Internet utilizando solo un número de red registrado, o varios números de red registrados, funcionó bien durante un tiempo. Desde principios hasta mediados de la década de 1990, se hizo evidente que Internet estaba creciendo tan rápido que todos los números de red IP estarían asignados a mediados de la década de 1990. Surgió la preocupación de que las redes disponibles quedarían completamente asignadas y algunas organizaciones no podrían conectarse a Internet.

