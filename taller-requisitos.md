# TALLER PRÁCTICO: EL RETO DEL ANALISTA ÁGIL
## SESIÓN 02 - MÓDULO DOCUMENTACIÓN

---

## REQUISITO CAÓTICO ORIGINAL

"Dev, quiero que los choferes vean viajes cercanos en tiempo real, pero que sea seguro y no se roben los datos. Y que si aceptan, les diga la ruta más corta para no gastar gas. Ah, y que la app no se trabe si hay 1,000 personas al mismo tiempo."

---

## ANÁLISIS DE REQUISITOS

### 1. REQUISITOS FUNCIONALES

| No. | Requisito Funcional | Descripción |
|-----|---------------------|-------------|
| 1 | Mostrar viajes disponibles en tiempo real | El sistema debe mostrar a los choferes los viajes cercanos con actualización continua |
| 2 | Permitir aceptar viajes | El chofer debe poder aceptar un viaje desde la lista de disponibles |
| 3 | Calcular ruta más corta | El sistema debe calcular y mostrar la ruta óptima al destino del pasajero |
| 4 | Optimizar ruta para ahorrar combustible | La ruta calculada debe considerar la distancia más corta para minimizar gastos de gasolina |
| 5 | Geolocalización en tiempo real | El sistema debe utilizar GPS para ubicar choferes y pasajeros |

### 2. REQUISITOS NO FUNCIONALES

| No. | Requisito No Funcional | Descripción |
|-----|------------------------|-------------|
| 1 | Seguridad de datos | Todos los datos sensibles (ubicación, información de usuarios) deben estar encriptados |
| 2 | Autenticación robusta | Solo choferes autorizados pueden acceder al sistema (login con credenciales) |
| 3 | Rendimiento con alta demanda | El sistema debe soportar 1,000 usuarios concurrentes sin degradación |
| 4 | Tiempo de respuesta | Las actualizaciones de viajes cercanos deben mostrarse en menos de 2 segundos |
| 5 | Disponibilidad | El sistema debe tener 99.5% de disponibilidad |
| 6 | Prevención de robo de datos | Implementar medidas contra interceptación de datos (HTTPS, tokens JWT) |

---

## COMPARATIVA DE TIPOS DE REQUISITOS

| Característica del Sistema | Tipo de Requisito |
|---------------------------|-------------------|
| Mostrar viajes cercanos en tiempo real | Funcional |
| Calcular ruta más corta | Funcional |
| Encriptación de datos sensibles | No Funcional |
| Soporte para 1,000 usuarios concurrentes | No Funcional |
| Autenticación de choferes | Funcional |
| Tiempo de respuesta menor a 2 segundos | No Funcional |
| Actualización automática de viajes cada 5 segundos | No Funcional |
| Notificar al pasajero cuando se acepta el viaje | Funcional |

---

## HISTORIAS DE USUARIO

### Historia de Usuario No. 1

**Título:** Visualización y aceptación de viajes cercanos

**Formato:**
> COMO chofer de la plataforma de transporte
> QUIERO ver los viajes cercanos en tiempo real y aceptar aquellos que me convengan
> PARA optimizar mi tiempo y aumentar mis ingresos

**Descripción extendida:** 
El chofer debe poder visualizar un mapa con los viajes solicitados cerca de su ubicación actual. Cada viaje debe mostrar: punto de recogida, destino, distancia aproximada y tarifa estimada. Al aceptar un viaje, el sistema debe bloquearlo para otros choferes y notificar al pasajero.

---

### Criterios de Aceptación (Gherkin) - Historia No. 1

```gherkin
Escenario: Visualizar viajes cercanos disponibles
  DADO que el chofer ha iniciado sesión en la aplicación
  Y su ubicación GPS está activada
  CUANDO abre el mapa de viajes disponibles
  ENTONCES el sistema muestra los viajes dentro de un radio de 5 kilómetros
  Y cada viaje muestra: punto de recogida, destino, distancia y tarifa estimada
  Y los viajes se actualizan cada 5 segundos automáticamente

Escenario: Aceptar un viaje disponible
  DADO que el chofer visualiza un viaje cercano disponible
  CUANDO presiona el botón "Aceptar viaje"
  ENTONCES el viaje se asigna al chofer
  Y el viaje deja de mostrarse a otros choferes
  Y el sistema notifica al pasajero que el chofer está en camino
  Y se muestra la ruta sugerida hacia el punto de recogida
  Escenario: Calcular ruta más corta al destino
  DADO que el chofer ha aceptado un viaje
  Y el sistema conoce la ubicación actual del chofer
  CUANDO el chofer presiona "Iniciar ruta"
  ENTONCES el sistema calcula la ruta más corta hacia el punto de recogida
  Y muestra instrucciones paso a paso (giros, calles, tiempo estimado)
  Y la ruta se actualiza dinámicamente si el chofer se desvía

Escenario: Optimización de ruta para ahorrar combustible
  DADO que el sistema ha calculado la ruta al destino
  CUANDO existen múltiples rutas posibles
  ENTONCES el sistema selecciona la ruta de menor distancia
  Y muestra la distancia total en kilómetros
  Y estima el costo de combustible basado en la distancia