# Script injection
Hemos encontrado un archivo ".pl" al abrirlo hemos visto que es un servidor en perl, que escucha el puerto 4747 peticiones HTTP.
Todo lo que mandes por el parametro "X" lo ejecuta en el servidor con el permiso del dueño del archivo que en este caso es flag04.

## comando usado
curl 'localhost:4747?x=$(getflag)'