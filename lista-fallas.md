# API de Integración Sistema KDL

### API REST KDL  
*versión 0.0.3*  

Este endpoint entrega información sobre piezas con fallas en el sistema de telegestión.

## Especificaciones Técnicas  

| Método | Endpoint                     | Ejemplo de URL                                              |  
|--------|------------------------------|------------------------------------------------------------|  
| GET    | `/lista-fallas/v1/maua`      | `https://api-afericao.kdltelegestao.com/lista-fallas/v1/maua` |  

### Parámetros Opcionales  
| Parámetro | Tipo   | Valor por Defecto | Descripción                                                                 |  
|-----------|--------|:-----------------:|-----------------------------------------------------------------------------|  
| `limit`   | `int`  | **1000**          | Número máximo de ítems devueltos.                                           |  
| `offset`  | `int`  | **0**             | Paginación de resultados (ej: `offset=400` ignora los primeros 400 ítems).  |  

El parámetro `limit` especifica el número máximo de documentos devueltos. Ejemplo: `limit=10` devuelve máximo 10 documentos.

El parámetro `offset` ignora N documentos antes de devolver resultados. Ejemplo: `offset=10` con `limit=10` devuelve documentos 11-20.


**Ejemplo de uso**:  
`?limit=200&offset=400`

## Respuesta de Ejemplo  
```json
{
    "data": {
        "type": "lista-fallas",
        "result": [
            {
                "id": 2281356173,
                "idSimuc": "2231021764",
                "idSimcon": "2236026",
                "etiqueta": "IP0505091",
                "subPrefeitura": "PINHEIROS",
                "dateTime": "2023-02-06T13:32:58Z",
                "status": 51
            }
        ]
    },
    "offset": 3027,
    "limit": 1000,
    "total": 3028,
    "success": true,
    "elapsedTime": "294.0598ms"
}
```

### Diccionario del Resultado:
##### Bloque PRINCIPAL:
| Identificador | Tipo     | Descripción                                                           | 
|:--------------|----------|:----------------------------------------------------------------------| 
| data          | `object` | Resultado de la consulta                                              | 
| total         | `int`    | Cantidad de ítems encontrados                                         |
| totalRetornado | `int`  | Cantidad de ítems devueltos                                          |
| limit         | `int`    | Cantidad de ítems devueltos por página                                | 
| offset        | `int`    | Parámetro utilizado para paginación de resultados                     |
| success       | `bool`   | Estado de éxito/falla de la interacción                               | 
| elapsedTime   | `string` | Duración de la consulta                                               | 

##### Bloque DATA:
| Identificador | Tipo            | Descripción                             | 
|:--------------|-----------------|:----------------------------------------| 
| type          | `string`        | Tipo de interacción                     | 
| result        | `array<object>` | Lista de piezas con fallas encontradas  | 

##### Bloque RESULT:
| Identificador         | Tipo     | Descripción                             | 
|:----------------------|----------|:----------------------------------------| 
| id                    | `uint32` | Identificador del evento                | 
| idSimuc               | `string` | Identificador de la pieza con falla     |
| idSimcon              | `string` | Identificador del concentrador          |
| etiqueta              | `string` | Identificador de la etiqueta            | 
| subPrefeitura         | `string` | Identificador de la subprefectura       | 
| dateTime              | `string` | Fecha/Hora/Minuto del evento            |
| status                | `string` | Código del evento                       |

##### Bloque Código de Estado:
| Status | Descripción                                                                                                 | 
|:-------|:------------------------------------------------------------------------------------------------------------| 
| 1      | LUMINARIA encendida durante el día                                                                          |
| 2      | LUMINARIA apagada durante la noche                                                                          | 
| 4      | LUMINARIA con caída de potencia                                                                             |
| 5      | LUMINARIA con falla o defecto                                                                               |
| 51     | SIMUC sin comunicación (evento informado cada 6h)                                                           | 
| 52     | SIMUC sin lecturas recolectadas desde la instalación (evento informado cada 6h, después de 48h de instalación) | 
| 203    | SIMCON fuera de línea, sin conexión (30+ minutos)                                                           |
| 204    | SIMCON fuera de línea, sin conexión (60+ minutos)                                                           |
| 205    | SIMCOM en batería                                                                                           |
| 251    | RED en tensión por debajo de 190V                                                                           |
| 252    | RED en tensión alta > 264V                                                                                  |

## Detalle del Estado

**1 - LUMINARIA encendida durante el día**

**2 - LUMINARIA apagada durante la noche**

**4 - LUMINARIA con caída de potencia**  
1. Una luminaria que enciende con una potencia reducida en un 30% de la potencia  
nominal registrada al momento de la instalación, excepto cuando esté dimerizada.

**5 - LUMINARIA con falla o defecto**  
Se consideran luminarias con fallas o defectos:  
1. Se envía un comando (siguiendo todos los pasos) para encender y no enciende;  
2. El sensor de luz indica baja luminosidad para encender la luminaria y no enciende;  
3. Dentro del horario programado para encender la luminaria, no enciende;

**51 - SIMUC sin comunicación** (evento informado cada 6h)  
Cuando el sistema indica que el SIMUC no devuelve datos de lectura:  
1. Falta de energía en el punto;  
2. Interferencia de radio en el lugar;  
3. Conexión incorrecta o mal contacto en la toma de la luminaria;  
4. Defecto en la pieza;

**52 - SIMUC sin lecturas recolectadas desde la instalación** (evento informado cada 6h,  
después de 48h de la instalación)

**203 - SIMCON fuera de línea, sin conexión** (30+ minutos)  
**204 - SIMCON fuera de línea, sin conexión** (60+ minutos)  
Cuando el SIMCON se encuentra:  
Sin señal del primer operador celular;  
Sin señal del segundo operador celular;  
Falta de energía en el lugar con descarga de la batería;  
Interferencia de señal causada por otro tipo de radio de alta potencia;

**205 - SIMCOM en batería**  
Cuando el SIMCOM se encuentra:  
Falta de energía en el punto de alimentación;  
Fuente interna dañada por sobrecargas eléctricas;

**251 - RED en tensión por debajo de 190V**

**252 - RED en tensión alta > 264V**

## Nota importante
En esta versión no se requiere el envío de token de acceso.


