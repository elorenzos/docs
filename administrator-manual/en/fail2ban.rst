Fail2ban analiza los archivos de registro (por ejemplo: file: `/ var / log / apache / error_log`) y prohíbe las IP que muestran signos maliciosos: demasiadas fallas en las contraseñas, búsquedas de vulnerabilidades, etc. Generalmente Fail2Ban se usa para actualizar las reglas del firewall rechace las direcciones IP durante un período de tiempo específico, aunque también se podría configurar cualquier otra acción arbitraria (por ejemplo, enviar un correo electrónico). Fuera de la caja Fail2Ban viene con filtros para varios servicios (Apache, Dovecot, Ssh, Postfix, etc.).

Fail2Ban puede reducir la tasa de intentos de autenticación incorrectos, sin embargo, no puede eliminar el riesgo que presenta una autenticación débil. Para mejorar la seguridad, abra el acceso al servicio solo para redes seguras utilizando el firewall.

Instalación
============
Instale desde el Centro de software o use la línea de comando: ::

  yum instala nethserver-fail2ban

Ajustes
========

Fail2ban se puede configurar en la categoría de seguridad del administrador del servidor. La mayoría de los ajustes se pueden cambiar en la pestaña: guilabel: `Configuración`, solo el terminal debe configurar los ajustes realmente avanzados.

Cárceles
-----
Una cárcel está habilitada y comienza a proteger un servicio cuando instala un nuevo módulo, la cárcel correspondiente (si existe) se activa automáticamente después de la instalación del paquete.
Todas las cárceles se pueden deshabilitar individualmente en la configuración de Jails.

Número de intentos
    Número de coincidencias (es decir, el valor del contador) que desencadena una acción de prohibición en la IP.

Espacio de tiempo
    El contador se establece en cero si no se encuentra ninguna coincidencia dentro de los segundos de "tiempo de búsqueda".

Tiempo de ban
    Duración de la IP para ser prohibido.

La cárcel recidiva es perpetua
    Cuando una IP va varias veces a la cárcel, la cárcel recidiva lo prohíbe por mucho más tiempo. Si está habilitado, es perpetuo.

Red
-------
Permitir prohibiciones en la LAN
    De forma predeterminada, los intentos fallidos de su red local se ignoran, excepto cuando habilitó la opción.
Lista blanca de IP / redes
    La IP que figura en el área de texto nunca será prohibida por fail2ban (una IP por línea). La red podría ser permitida en el panel de la Red de Confianza.

Email
-----

Enviar notificaciones por correo electrónico
    Habilitar para enviar correos electrónicos administrativos.

Correos electrónicos de los administradores
    Lista de direcciones de correo electrónico de los administradores (una dirección por línea).

Notificar eventos de inicio / parada de la cárcel.
    Enviar notificaciones por correo electrónico cuando se inicia o se detiene una cárcel.

Unban IP
========

Las direcciones IP están prohibidas cuando se encuentran varias veces en el registro, durante un tiempo de búsqueda específico. Se almacenan en una base de datos para volver a prohibirse cada vez que reinicie el servidor o el servicio. Para desbancar una IP puede usar la pestaña: guilabel: `Unban IP` en la categoría de estado del administrador del servidor.

Estadística
==========

La pestaña: guilabel: `Estadísticas de prohibición 'está disponible en la categoría de estado del administrador del servidor, le da el número total de prohibiciones por cárcel, así como el total de todas las prohibiciones.

Herramientas
=====

Fail2ban-client
---------------

Fail2ban-client es parte de las rpm fail2ban, da el estado de fail2ban y todas las carceles disponibles: ::

  estado del cliente fail2ban

Para ver una cárcel específica: ::

  fail2ban-client status sshd

Para ver qué archivos de registro se monitorean para una cárcel: ::

  fail2ban-client obtiene nginx-http-auth logpath

Fail2ban-listban
----------------

Fail2ban-listban cuenta las direcciones IP actualmente y está totalmente prohibida en todas las cárceles activadas, al final muestra las direcciones IP que aún están prohibidas por shorewall. ::

  fail2ban-listban

Fail2ban-regex
--------------

Fail2ban-regex es una herramienta que se utiliza para probar la expresión regular en sus registros, es parte del software fail2ban. Solo se permite un filtro por cárcel, pero es posible especificar varias acciones, en líneas separadas.

La documentación es `legible en el proyecto fail2ban <http://fail2ban.readthedocs.io/en/latest/filters.html>` _.

::

  fail2ban-regex / var / log / YOUR_LOG /etc/fail2ban/filter.d/YOUR_JAIL.conf --print-all-matched

También puede probar la expresión regular personalizada directamente: ::

  fail2ban-regex / var / log / secure '^% (__ prefix_line) s (?: error: PAM:)? [aA] uthentication (?: failure | error) para. * de <HOST> (a través de \ S +)? \ s * $ '

Fail2ban-unban
--------------

Fail2ban-unban se usa para anular una IP cuando la prohibición debe eliminarse manualmente. ::

  fail2ban-unban <IP>

También puede usar el comando incorporado con fail2ban-client: ::

  fail2ban-client set <JAIL> unbanip <IP>

Quien es
=====

Si desea consultar la base de datos de IP `` whois`` y obtener el origen de la IP prohibida por correo electrónico, puede instalar el `` whois`` rpm.
