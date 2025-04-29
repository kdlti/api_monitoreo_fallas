# API de Integración Sistema KDL / UNIDESK-SIIP

### API REST KDL  
*versión 0.0.3*  

Este endpoint es responsable de actualizar el estado de fallas ya procesadas en el sistema.  

## Descripción  
Cuando una notificación de falla es visualizada por el usuario, se debe actualizar su estado a **"descargada"** para evitar que:  
- La misma notificación reaparezca en búsquedas futuras  
- Se generen duplicados en el sistema de monitoreo  

El proceso se realiza mediante el envío de una lista de IDs (identificadores únicos) a este endpoint.  

## Especificaciones Técnicas  

| Método | Endpoint                                  | Ejemplo de URL                                                      |  
|--------|------------------------------------------|--------------------------------------------------------------------|  
| POST   | `/confirma-fallas-descargadas/v1/maua`   | `https://api-afericao.kdltelegestao.com/confirma-fallas-descargadas/v1/maua` |  

### Parámetros  
- **Lista de IDs**: Array de números `uint32`  
- **Content-Type**: `application/json`  

## Ejemplo de Implementación  

```go
package main

import (
	"bytes"
	"encoding/json"
	"net/http"
)

func main() {
	// 1. Preparar datos
	ids := []uint32{123456789, 987654321, 111111111}
	jsonData, _ := json.Marshal(ids)

	// 2. Configurar request
	req, _ := http.NewRequest(
		"POST",
		"https://api-afericao.kdltelegestao.com/confirma-fallas-descargadas/v1/maua",
		bytes.NewBuffer(jsonData),
	)
	req.Header.Set("Content-Type", "application/json")

	// 3. Enviar solicitud
	client := &http.Client{}
	resp, _ := client.Do(req)
	defer resp.Body.Close()
}