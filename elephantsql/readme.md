# ELEPHANT SQL

## ¿Que es ElephantSQL?
ElephantSQL es una plataforma de hosting de base de datos, que nos permite trabajar con Bases de datos de PostgreSQL en la nube, mientras el mismo se encarga de la instalación, actualizaciones o copias de seguridad.

[Enlace a ElephantSQL](https://www.elephantsql.com/)

## Creando instancias
Lo primero que tenemos que hacer es crear una cuenta propia. Se nos da opciones como crearla desde cero o incluso vincular una cuenta de GitHub. Una vez creada dicha cuenta, es hora de crear instancias para cada una de las bases de datos que vamos a crear (La idea es copiar aquellas presentes en SQLZOO).

![image](./img/img1.png "Logo Title Text 1")

Cada una de estas instancias cuenta con un Plan específico adaptado a nuestras necesidades (Variados capacidad de almacenaiento, velocidad de acceso, conexiones concurrentes, followers...). Para nosotros el plan gratuito es mas que suficiente, pero hemos de tener en cuenta en todo momento que solo contamos con 20MB de almacenamiento y un maximo de cinco conexiones concurrentes.

![image](./img/img2.png "Logo Title Text 1")

Una vez seleccionado el plan, es hora de darle nombre a nuestra instancia. Como ya dije antes, creo una instancia por cada base de datos de SQLZOO (world, nobel, movies, goal...). Las etiquetas, si bien útiles cuando se amontonan gran cantidad de instancias distintas agrupadas por temas similares, no las considero necesarias en este caso. 

![image](./img/img3.png "Logo Title Text 1")

Es hora de elegir un servicio de hosting. Existen varios disponibles (Amazon, Google, Azure...). El que mas nos guste o mas cercano sea a nuestra zona.

![image](./img/img4.png "Logo Title Text 1")

Creada la primera instancia, repetimos el proceso hasta tener las cuatro base de datos de SQLZOO repartidas con una instancia cada una.

![image](./img/img5.png "Logo Title Text 1")

Cada instancia contará con un servidor (Dependiente de la plataforma de hosting), un usuario y base de datos por defecto y una contraseña (El puerto suele ser 5432). Debemos tener presentes estos datos a la hora de conectarse al servidor de base de datos mas adelante.

![image](./img/img6.png "Logo Title Text 1")

Teniendo esto en cuenta, dejamos nuestra cuenta de Elephant por el momento, y vamos a trabajar en Windows.

## Instalando PGAdmin y SQLShell (psql)
Descargamos el instalador de PostgreSQL.
[Enlace](https://www.postgresql.org/download/)


![image](./img/img7.png "Logo Title Text 1")

El instalador de Postgre trae varias características como el servidor, Stack Builder (que facilita la incorporación de nuevos módulos al sistema), pero las dos cosas que nos interesan son Pgadmin, que nos permite gestionar las bases de datos a través de interfaz gráfica, y Command Line Tools, que permite lo mismo pero a golpe de comandos. Voy a explicar como poner en funcionamiento cada una, pero la idea es trabajar con comandos una vez detallada la configuración.

![image](./img/img8.png "Logo Title Text 1")

## Acceder al Servidor desde psql
Para acceder al servidor desde psql, nos vamos al CMD y usamos el comando SQL con la siguiente estructura:
> Ruta\psql.exe -h servidor -U usuario -d basededatospordefecto -p puerto

![image](./img/img9.png "Logo Title Text 1")

Hasta donde se, no hay manera de incluir la contraseña en este comando, ya que se considera extremadamente inseguro.

Una vez realizada la conexión e indicada la contraseña, ya podemos empezar a trabajar con nuestra base de datos desde la linea de comandos, haciendo las operaciones de DQL o DML vistas hasta ahora.

![image](./img/img10.png "Logo Title Text 1")

## Acceder al Servidor desde Pgadmin
Abrimos pgadmin, que nos proporciona una interfaz en el navegador para gestionar la base de datos. PAra el primer uso, nos perdirá una contraseña para el usuario master.

![image](./img/img11.png "Logo Title Text 1")

Creamos un nuevo servidor, el cual se conectará al de ElephantSQL:

![image](./img/img12.png "Logo Title Text 1")

Le damos nombre a la conexión, la cual no necesariamente tiene que verse correspondida con el nombre de la base de datos.

![image](./img/img13.png "Logo Title Text 1")

Volvemos a usar los datos de la instancia de ElephantSQL para indicar servidor, usuario, base de datos y contraseña.

![image](./img/img14.png "Logo Title Text 1")

Creada la conexión ya podemos empezar a trabajar con la base de datos de Elephant. En este momento ya habia creado la tabla world y metido un par de datos desde la linea de comandos, con lo cual podemos ver los resultados desde la interfaz.

![image](./img/img15.png "Logo Title Text 1")



