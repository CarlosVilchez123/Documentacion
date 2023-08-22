# Documentación Proyecto Smart Campus

# Proyecto Calidad de Aire consumo de APIs

Ruta -> Clidad de Aire Interior: src/pages/DetalleCA.js, Calidad de Aire Exterior: src/pages/DetalleCAI.js

Para hacer el consumo de las APIs que permiten extraer los datos de la Calidad de Aire de exteriores es necesario tener encuenta lo siguiente:
- Se requiere extraer la url del servidor: "http://www.beegons.com:8060"
- Se necesita determinar la url de la API a la cual se le va a realizar la peticion de extraccion de datos. 
    Esta url varia dependiendo los siguiente parametros: El nombre del lugar donde se ubican los sensores (Ctic, puerta 5 o puerta 3) y la cadena de consulta (id del sensor) de la url de la API, esta cadena de consula depende del sensor; del cual, se esten solicitando datos.

## Consumo de APIs Exteriores
### Lugares donde estan ubicados los sensores
- ctic.
- puerta 5.
- puerta 3.

### Identificador de los sensores por lugar geografico
- ctic -> Id del Sensor: "beegons:rak-3272s-e".
- puerta 5 -> Id del Sensor: "beegons:rak-3272s-f".
- puerta 3 -> Id del Sensor: "beegons:rak-3272s-h".

### Identificadores del tipo de contaminante
Para poder determinar el tipo de contaminante que son filtrados es necesario poder utilizar un identificador.
Estos identificadores se agregaran a la cadena de consulta de la url de la siguiente manera:
- "1" representa si un contaminante ha sido seleccionado.
- "0" representa si un contaminante no ha sido seleccionado.

### Acceder a la API de descarga de datos en un archivo CSV
Para poder acceder a la API de descarga solamente se debe concatenar la siguiente consulta "api/v1/calidad-de-aire/descargar/".
Se debe concatenar a entre la url del servidor y los identificadores ya mencionados.
- Por ejemplo: http://www.beegons.com:8060/api/v1/calidad-de-aire/descargar/beegons:rak-3272s-e/10110011011

## Consumo de APIs Interiores
### Identificadores de los lugares en interiores
Solamente hay sensores ubicados en distintos salones dentro del CTIC.

- Interiores del CTIC
"Oficina de Administración" -> 1102;
"Laboratorio SmartCity" -> 1201;
"Oficina de Calidad Universitaria" -> 1202;
"Oficina de Capacitación" -> 1203;
"Secretaría" -> 1204

### Acceder a la API de descarga de datos en un archivo CSV
Para poder acceder a la API de descarga solamente se debe concatenar la siguiente consulta "api/v1/carga-viral/descargar/".
Se debe concatenar a entre la url del servidor y los identificadores ya mencionados.
- Por ejemplo: http://www.beegons.com:8060/api/v1/carga-viral/descargar/beegons:rak-3272s-e/


## Consumir APIs de peticion de informacion
Solamente se debe concatenar la consulta "api/v1/calidad-de-aire/" y para acceder a la informacion de los sensores de interior se debe concatenar "/api/v1/carga-viral/". Seguido de los respectivos identificadores de cada sensor y lugar solicitado.

# Proyecto Cuenta Persona (Conteo de aforo)
Ruta -> "src/pages/CuentaPersonas/ControlAforo.js"

Para obtener la cantidad de personas se requiere obtener la informacion que capturan los sensores, para obtener dicha informacion requerimos realizar la peticion a la api, y para obtener los datos actualizados del conteo de personas que ingresan a un resinto, requerimos habilitar un socket que permita recibir la información.

## Lamada a la API
Para poder realizar la llamada a la API de conteo personas requerimos los siguientes identificadores:
- la url del servidor (es la misma que el de calidad de aire)
- la cadena de la consulta que tiene la siguiente forma /api/v1/cuenta-persona/, el identificador del sensor del cual querramos extraer la informacion. Los id son ctic o smartcity y por ultimo concatenamos la siguiente cadena "?last=20&columns=001001"

## Habilitando el socket
Para poder habilitar un socket que permita escuchar un evento emitido por el cliente, para ello identificado el evento que se llama "CuentaPersonas/CuentaPersonas:ctic", una vez se haya escuchado el evento, se activara en el lado del servidor el manejador correspondiente a este evento y se ejecutara la peticion.

De la petición necesitamos extraer el dato de la ultima persona que entro a un resinto en especifico, de la data que estamos recibiendo solamente nos quedaremos con tiempo de entrada y se le asigna un valor numero a la persona que ha ingresado. Y seteamos el array de personas.

## Descarga de datos API de descarga  
Ruta -> "src/pages/CuentaPersonas/PopupDescarga.js"

Para hacer consumo de la API de descarga cuando necesitemos llamarla como funcion necesitamos la url del servidor y el id del resinto del cual querramos obtener información.

Dentro de la misma la laogica esta implementada de la siguiente manera:
- Necesitamos la ip del sensor, la cadena /api/v1/cuenta-persona/descargar el id del resinto del cual se requiere obtener información y el rango de fechas del historias de los sensores y por ultimo concatenamos la siguiente cadena "&columns=001001".

# Proyecto Calidad de Agua
Este proyecto tiene la siguiente ruta -> "src/pages/CalidadAgua.js"

## Llamada a la API
La llamada a esta API se realiza de forma similar a la del caso anterior, la url obtenida es similar a la url de llamada anterior, la diferencia es qye la cadena de consulta tiene la siguiente forma -> "/api/v1/calidad-de-agua?last=1&columns=00000111"

Luego, los datos obtenidos de esta solicitud solamente se capturan los datos de temperatura, ph y turbidez de la data obtenido y son setados en el hook de data actual. Previamente esta data debio haberse formateado en formato JSON.

## Habilitando el socket
Conectamos el socket con el servidor, la url del servidor es el siguiente: http://www.beegons.com:8060.
Y cuando ocurra una llamada a la API, se establece tipo de transporte de datos, se escoge "WebSocket" se llama al metodo con el cual se obtuvieron la data formateado en JSON, y el socket escucha el evento llamado "calidad_agua/calidad_agua:rak-3272s-o" y cuando el socket escucha se setean los datos con la data pedida de la API.

# Proyecto Smart Parking
La API que permite mostrar los datos del parking en tiempo real se llama RealTimeComponentSmartParking.js se encuentra el ruta -> "src/pages/SmartParking/RealTimeComponentSmartParking.js". 
## Consumo de API
Esta APi establece un array el cual tendra solo valores de 0s y 1s, 1 representa cuando un estacionamiento esta vacio y 0 representa que esta vacio. Lo que se hace es que se requiere habilitar un socket, y este debe escuchar el evento "smartparking_va/smartparking_va:ctic", luego el servidor activa el manejador correspondiente a este evento y se solicita la siguiente data.
- Contenedor -> contiene el arreglo de 0s y 1s del parking
- la imagen actualizada -> muestra al usuario los estacionamientos libres
- estado del contenedor 

Con el estado de la data obtenida se setean los datos obtenidos.

# Reconocimiento Facial
Esta es una API que complementa al proyecto del Conteo de Aforo ruta -> "src/pages/Cuentapersonas.js". Esta API obtiene, en imagenes, el rostro de las personas que van entrando a un resinto.

## Llamada a la API
Para poder hacer uso de esta API, necesitamos habilitar un socket, lo conectamos al servidor cuya url es la misma con la que se ha estado trabjando, y establecemos un transporte de datos "WebSocket", definimos el evento de escucha del socket "rec_fac/rec_fac:rec_fac02".

Luego, necesitamos optener la imgenes que estan almacenadas en un servidor local, cuya url es http://localhost:5000/image/${data.nombres.value}, data.nombre.value es el valor numero de la persona que se le asgina al ingresar al resinto.

Por ultimo, simplemente formateamos los datos obtenidos en formato JSON, en ese caso en particular, se establece el metodo de solicitud, el contenido de la cabecera, y el cuerpo de la solicitud.










