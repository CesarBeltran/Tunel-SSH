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

Paso 1
Como primer lugar se sube servicio apache en el servidor Web VMKALI2 IP 192.168.31.179.
	systemctl start apache2
	

Paso 2
En el servidor SSH Gateway se realiza la adición el siguiente parámetro en el fichero /etc/ssh/ssh_config, esto con el fin de permitir las conexiones desde cualquier dirección IP. Después de esto se reinicia servicio ssh.
Si no se modifica este parámetro, los puertos que se abran en este servidor únicamente estarán disponibles para localhost, es decir 127.0.0.1. 

	GatewayPorts yes
	
	
.....


