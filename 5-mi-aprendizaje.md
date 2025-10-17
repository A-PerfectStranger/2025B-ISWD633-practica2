## Conocimientos Antes de la Práctica

Antes de realizar esta práctica, mi comprensión sobre Docker se limitaba principalmente a la creación y ejecución de contenedores básicos. Tenía una idea general de cómo funcionan las imágenes y los contenedores, pero no conocía en detalle cómo estos pueden comunicarse entre sí mediante redes, ni cómo gestionar configuraciones dinámicas a través de variables de entorno.  

Tampoco estaba familiarizado con las prácticas recomendadas para proteger datos sensibles en entornos Docker, por lo que conceptos como Docker Secrets eran completamente nuevos para mí.


## Durante la Práctica

Trabajé con variables de entorno. Aprendí a definir variables dinámicamente al ejecutar un contenedor con la opción `-e`.  

```
  docker run --name mysql -e DB_USER=admin -e DB_PASS=12345 mysql
 ```
Descubrí que Docker permite crear redes personalizadas para que los contenedores se comuniquen de forma aislada.
```
docker network create mi_red
docker run -d --name db --network mi_red mysql
docker run -d --name app --network mi_red miapp
```

Investigando sobre Docker Secrets, entendí que es una forma segura de almacenar y distribuir datos sensibles (como contraseñas, certificados o claves API) sin exponerlos en variables de entorno ni en el código fuente.
Docker Secrets guarda estos datos encriptados y solo los hace accesibles a los contenedores que los necesitan.

```
echo "mi_clave_supersecreta" | docker secret create db_password -
```

Luego, este secreto puede ser montado dentro del contenedor como un archivo seguro en /run/secrets/.
Esto evita el riesgo de exponer credenciales en imágenes, docker inspect o logs.

## Problemas Encontrados y Solución
Durante la práctica, el principal problema que enfrenté fue que mi contenedor de aplicación no podía comunicarse con la base de datos. Descubrí que ambos estaban corriendo en redes distintas, por lo que no se reconocían entre sí.
La solución fue crear una red personalizada con docker network create y conectar ambos contenedores a ella. Una vez hecho esto, la aplicación pudo resolver el nombre del servicio de la base de datos correctamente.

