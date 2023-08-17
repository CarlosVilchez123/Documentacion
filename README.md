# Documentación Proyecto Smart Campus

## Proyecto Calidad de Aire consumo de APIs

Ruta -> Clidad de Aire Interior: src/pages/DetalleCA.js, Calidad de Aire Exterior: src/pages/DetalleCAI.js

Para hacer el consumo de las APIs que permiten extraer los datos de la Calidad de Aire de exteriores es necesario tener encuenta lo siguiente:
- Se requiere extraer la url del servidor: "http://www.beegons.com:8060"
- Se necesita determinar la url de la API a la cual se le va a realizar la peticion de extraccion de datos. 
    Esta url varia dependiendo los siguiente parametros: El nombre del lugar donde se ubican los sensores (Ctic, puerta 5 o puerta 3) y la cadena de consulta (id del sensore) de de la url de la API, esta cadena de consula depende del sensor; del cual, se esten solicitando datos.
    
    Notas:
        - Nombre de los lugares donde estan los sensores: ctic, puerta 5, puerta 3.
        - Id de los sensores por lugar geografico: (ctic) "beegons:rak-3272s-e", (puerta 5) "beegons:rak-3272s-f", (puerta 3) "beegons:rak-3272s-h"
        -






