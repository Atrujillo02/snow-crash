# SCP Command
scp -P4242 level02@localhost:level02.pcap level02.pcap

# Cosa
Nos hemos encontrado un archivo .pcap que son trazas de red.

Con una herramienta como Wireshark o tcpdump podemos leerlo y extraer la información


# Telnet
Analizandolo vemos que hay paquetes TCP y con la utilidad Follow TCP Stream de Wireshark podemos
juntar el texto que se envió.


Vemos que es un texto en claro de una comunicación con un Telnet.
Se ve claramente las partes en las que le pide el nombre de usuario y contraseña.

Se ve claramente:

> Password: 
> ft_wandr...NDRel.L0L

En telnet los puntos significan que se está borrando un caracter ya enviado, por lo tanto,
la true contraseña sería:

> ft_waNDReL0L

Esa es la contraseña de flag02.

