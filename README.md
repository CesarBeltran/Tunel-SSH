# Tunel SSH


## Tunnel SSH Tunnel para redireccion de paquetes.

En este laboratorio se creó un túnel SSH entre dos servidores, Servidor Gateway y Servidor Web. El objetivo del túnel es proteger el tráfico de red entre estos dos servidores y permitir el acceso seguro a un servicio web apache
Ejecución de la práctica:
Para esta práctica se usan dos maquina virtuales Kali Linux versión 2023 con direcciones IP configuradas en el mismo segmento y hospedadas en Vmware Workstation.
La máquina cliente Windows es el mismo Host donde están las dos maquinas virtuales.


Para esta práctica se usan dos maquina virtuales Kali Linux versión 2023 con direcciones IP configuradas en el mismo segmento y hospedadas en Vmware Workstation.
La máquina cliente Windows es el mismo Host donde están las dos maquinas virtuales.


<p align="center">
<img src="https://raw.githubusercontent.com/CesarBeltran/Tunel-SSH/main/Img/Tabla_Srv.jpg" width="740" />
</p>


<p align="center">
<img src="https://raw.githubusercontent.com/CesarBeltran/Tunel-SSH/main/Img/Diagrama.jpg" width="740" />
</p>

### Paso 1
Como primer lugar se sube servicio apache en el servidor Web VMKALI2 IP 192.168.31.179.

	systemctl start apache2
	

### Paso 2
En el servidor SSH Gateway se realiza la adición el siguiente parámetro en el fichero /etc/ssh/ssh_config, (NO en el fichero sshd_config) esto con el fin de permitir las conexiones desde cualquier dirección IP. Después de esto se reinicia servicio ssh.
Si no se modifica este parámetro, los puertos que se abran en este servidor únicamente estarán disponibles para localhost, es decir 127.0.0.1. 

	GatewayPorts yes
	

### Paso 3
En este paso se debe crear en el servidor Gateway un script con el comando necesario para el túnel SSH, sin embargo, inicialmente se va a realizar la ejecución del comando de forma manual para verificar el funcionamiento.
Para crear el túnel SSH se realiza con el siguiente comando, se agrega el parámetro (-v) para activar el modo verbose y poder ver en tiempo real el estado del túnel y sus conexiones.

	ssh -v -N -T -C -L 8080:192.168.31.136:80 192.168.31.179


Como se puede observar a continuación, se está realizando autenticación hacia el servidor Web (VMKALI2 IP 192.168.31.179) a través de llave publica, adicionalmente el puerto 8080 está escuchando en todas las interfaces del servidor.


Desde el navegador del servidor Windows se realiza conexión hacia el servidor Gateway por el puerto 8080 de forma exitosa como se muestra a continuación.
http://kali:8080 

<p align="center">
<img src="https://raw.githubusercontent.com/CesarBeltran/Tunel-SSH/main/Img/Webp8080.jpg" width="740" />
</p>


Luego de confirmar que el túnel se crea correctamente se realiza el Script etc/init.d/sshgateway.sh
	
	#!/bin/sh
	ssh -N -T -C -L 8080: 192.168.31.136:80 192.168.31.179

Por último, se crea un link simbólico para que el script sshgateway sea iniciado automáticamente cuando el sistema operativo inicia en runlevel 5

	ln -s /etc/init.d/sshgateway.sh /etc/rc5.d/S99sshgateway.sh


Se realiza pruebas de reinicio sobre el servidor Gateway confirmando que luego del reinicio, el script es ejecutado automáticamente.

#### Nota
Para esta practica de laboratorio, se habian configurado los servidores previamente para para permitir la conexión haciendo uso de llave publica/privada con el fin que no sea necesario especificar usuario y password al momento de crear el tunel SSH.
Dicho procedimiento se encuentra en el siguiente repositorio: https://github.com/CesarBeltran/Autenticacion-ssh-llave-publica

