# executable decompiler
Hemos encontrado un ejecutable llamado level03 al hacerle un cat hemos encontrado codigo compilado en C.
Hemos usados una herramienta para obtener el codigo fuente original donde hemos visto esta linea:
return system("/usr/bin/env echo Exploit me");

El programa env utiliza $PATH para buscar donde esta el comando echo. Busca en orden y el primero que encuentra lo ejecuta.
Podemos engañar al programa añadiendo al principio del $PATH un directorio con nuestro propio echo.
Gracias a esto:
v1 = getegid();
    v2 = geteuid();
    v0 = v1;
    setresgid(v1, v1);
    v0 = v2;
    setresuid(v2, v2);
Se ejecuta con los permisos del dueño del archivo que este caso es flag03.
Por ejemplo al escribir en el archivo "whoami" te devuelve flag03. Por lo tanto al ejecutar "getflag" lo hace como si fuera flag03 y te devuelve la contraseña. 