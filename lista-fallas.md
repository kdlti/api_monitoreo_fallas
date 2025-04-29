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
