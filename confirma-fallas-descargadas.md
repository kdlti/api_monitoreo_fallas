# API de Integración Sistema KDL / UNIDESK-SIIP

### API REST KDL
*versión 0.0.3*

Este endpoint es responsable de actualizar el estado de registros de fallas ya entregadas.

Cuando una notificación de falla es visualizada, es necesario actualizar su estado para indicar que ya ha sido tratada, con el fin de evitar que la misma notificación se muestre nuevamente en futuras consultas.

Esta actualización se realiza enviando una lista de identificadores a este endpoint, que será el responsable de actualizar el estado de la notificación.

Esta regla busca preservar el orden y la eficacia del sistema de monitoreo, para que los usuarios puedan centrarse únicamente en las notificaciones relevantes y útiles.

| Método | URI                                         | Ejemplo                                                   | 
|--------|---------------------------------------------|:----------------------------------------------------------| 
| POST   | `/confirma-fallas-descargadas/v1/maua`      | api-afericao.kdltelegestao.com/confirma-fallas-descargadas/v1/maua |

##### Ejemplo de uso
```go
// lista de valores uint32
listaUint32 = [123456789, 987654321, 111111111]

// convertir la lista a formato JSON
jsonData = json.dumps(listaUint32)

// definir los encabezados de la solicitud
headers = {"Content-Type": "application/json"}

// realizar la solicitud POST
response = requests.post(url="api-afericao.kdltelegestao.com/confirma-fallas-descargadas/v1/maua", data=jsonData, headers=headers)
```

## Nota importante
En esta versión no se requiere el envío de token de acceso.