# ESPECIFICACION DE REQUISITOS DE SOFTWARE (SRS) - SISTEMA DE INVENTARIO

## VERSION LIGERA (AGILE SRS)

---

## 1. ALCANCE Y FRONTERAS DEL SOFTWARE

### 1.1 Proposito

El presente documento especifica los requisitos funcionales y no funcionales del sistema de control de inventario para una tienda local. Su objetivo es establecer un contrato tecnico entre el equipo de desarrollo y el cliente, definiendo con exactitud que se construira y que quedara fuera del alcance del proyecto.

### 1.2 Alcance del Sistema

El sistema permitira a los usuarios autorizados gestionar el inventario de productos de la tienda, controlando las existencias de manera estricta y generando alertas cuando los niveles de stock sean bajos. La plataforma sera accesible a traves de un navegador web y utilizara PostgreSQL como sistema de base de datos.

### 1.3 Fronteras (Fuera de Alcance)

- El sistema no gestionara ventas, facturacion o cobros.
- El sistema no incluira un modulo de gestion de clientes.
- El sistema no generara reportes financieros o contables.
- El sistema no integrara pasarelas de pago.
- El sistema no tendra aplicacion movil nativa en esta version.

---

## 2. PERSPECTIVA Y ACTORES (USER PERSONAS)

### 2.1 Actores del Sistema

| Actor | Descripcion |
|-------|-------------|
| Administrador | Usuario con permisos totales para gestionar productos, categorias, usuarios y configuraciones del sistema. |
| Almacenista | Usuario encargado del registro diario de entradas y salidas de productos, asi como de la verificacion de inventario fisico. |
| Supervisor | Usuario con permisos de visualizacion de reportes y alertas de inventario, sin capacidad de modificar datos. |

### 2.2 Roles y Permisos

| Funcionalidad | Administrador | Almacenista | Supervisor |
|---------------|:---:|:---:|:---:|
| Registrar producto | SI | SI | NO |
| Editar producto | SI | SI | NO |
| Eliminar producto | SI | NO | NO |
| Registrar entrada/salida | SI | SI | NO |
| Consultar inventario | SI | SI | SI |
| Generar alertas de stock bajo | SI | SI | SI |
| Gestionar usuarios | SI | NO | NO |
| Ver reportes | SI | SI | SI |

---

## 3. REQUISITOS FUNCIONALES AGRUPADOS

### 3.1 Modulo: Gestion de Productos

#### Historia de Usuario No. 1

**Titulo:** Registrar nuevo producto

**Formato:**
> COMO almacenista
> QUIERO registrar un nuevo producto en el sistema con sus datos basicos
> PARA mantener actualizado el inventario de la tienda

**Descripcion extendida:**
El almacenista debe poder ingresar la siguiente informacion del producto: codigo de barras (validado segun estandar de El Salvador), nombre, descripcion, categoria, precio unitario y cantidad inicial en stock. El sistema debe validar que el codigo de barras no exista previamente en la base de datos.

---

##### Criterios de Aceptacion (Gherkin)

```gherkin
Escenario: Registrar producto exitosamente
  DADO que el almacenista ha iniciado sesion en el sistema
  Y se encuentra en la seccion "Registrar producto"
  CUANDO ingresa el codigo de barras (valido segun estandar ES)
  Y completa todos los campos requeridos (nombre, descripcion, categoria, precio, stock inicial)
  Y presiona el boton "Guardar"
  ENTONCES el sistema valida que el codigo de barras no exista en la base de datos
  Y registra el producto en la base de datos PostgreSQL
  Y muestra el mensaje "Producto registrado exitosamente"

Escenario: Registrar producto con codigo de barras duplicado
  DADO que el almacenista esta en la seccion "Registrar producto"
  CUANDO ingresa un codigo de barras que ya existe en el sistema
  Y presiona el boton "Guardar"
  ENTONCES el sistema muestra el mensaje "El codigo de barras ya esta registrado"
  Y no permite guardar el producto

 ---
  #### Historia de Usuario No. 2

***Titulo:*** Editar informacion de producto

**Formato:**
> COMO almacenista
> QUIERO editar los datos de un producto existente
> PARA corregir informacion o actualizar precios

**Descripcion extendida:**
El almacenista debe poder buscar un producto por nombre o codigo de barras y modificar sus datos (nombre, descripcion, categoria, precio). El codigo de barras no debe ser modificable una vez creado.

---

##### Criterios de Aceptacion (Gherkin)

```gherkin
Escenario: Editar producto exitosamente
  DADO que el almacenista ha iniciado sesion
  Y busca un producto existente por codigo de barras
  CUANDO modifica el nombre, descripcion o precio del producto
  Y presiona el boton "Actualizar"
  ENTONCES el sistema guarda los cambios en la base de datos
  Y muestra el mensaje "Producto actualizado exitosamente"

Escenario: Intentar editar codigo de barras
  DADO que el almacenista esta editando un producto existente
  CUANDO intenta modificar el campo de codigo de barras
  ENTONCES el sistema deshabilita el campo y muestra "El codigo de barras no puede ser modificado"

  ---

## 4. RESTRICCIONES TECNOLOGICAS OBLIGATORIAS

| Restriccion | Detalle |
|-------------|---------|
| Sistema de base de datos | PostgreSQL (obligatorio segun requerimiento del cliente) |
| Lenguaje de backend | Node.js / Java / Python (a definir por el equipo) |
| Framework frontend | React / Angular / Vue.js (a definir por el equipo) |
| Compatibilidad de navegadores | Debe ser compatible con Chrome, Firefox y Edge en sus ultimas dos versiones |
| Validacion de codigos de barra | El sistema debe validar codigos de barra segun el estandar de El Salvador (codigos de barra GTIN-13 con prefijo 741) |
| Servidor | El sistema debe desplegarse en un servidor Linux o Windows Server con acceso a internet |

---

## 5. REQUISITOS DE CALIDAD (IEEE 830)

| ID | Requisito de Calidad | Descripcion | Criterio de Verificacion |
|----|----------------------|-------------|--------------------------|
| CAL-01 | Rendimiento | El sistema debe cargar la lista de inventario en menos de 2 segundos | Prueba de carga con 1,000 productos registrados |
| CAL-02 | Seguridad | Las contrasenas de los usuarios deben estar almacenadas utilizando hash (bcrypt) | Revision de codigo y pruebas de seguridad |
| CAL-03 | Seguridad | El sistema debe utilizar HTTPS para todas las comunicaciones | Verificacion de certificado SSL/TLS |
| CAL-04 | Disponibilidad | El sistema debe tener una disponibilidad del 99.5% | Monitoreo de uptime durante un mes |
| CAL-05 | Usabilidad | La interfaz debe ser responsiva y funcionar correctamente en dispositivos moviles y tabletas | Pruebas en diferentes tamanos de pantalla |
| CAL-06 | Integridad | El sistema debe validar que el codigo de barras ingresado cumpla con el estandar de El Salvador (prefijo 741) | Validacion de formato mediante expresion regular |
| CAL-07 | Escalabilidad | El sistema debe soportar hasta 100 usuarios concurrentes sin degradacion significativa | Prueba de carga con 100 usuarios simultaneos |

---

## 6. MATRIZ DE TRAZABILIDAD

| ID Requisito | Historia de Usuario | Componente Codigo | ID Test de QA |
|--------------|---------------------|-------------------|---------------|
| REQ-001 | US-01 (Registrar producto) | productController.js | TC-001 (Registro exitoso) |
| REQ-002 | US-01 (Validacion codigo de barra) | productController.js | TC-002 (Codigo duplicado) |
| REQ-003 | US-02 (Editar producto) | productController.js | TC-003 (Edicion exitosa) |
| REQ-004 | US-03 (Eliminar producto) | productController.js | TC-004 (Eliminacion exitosa) |
| REQ-005 | US-04 (Registrar entrada) | stockController.js | TC-005 (Entrada exitosa) |
| REQ-006 | US-05 (Registrar salida) | stockController.js | TC-006 (Salida exitosa) |
| REQ-007 | US-05 (Validacion stock suficiente) | stockController.js | TC-007 (Stock insuficiente) |
| REQ-008 | US-06 (Configurar stock minimo) | alertController.js | TC-008 (Configuracion exitosa) |
| REQ-009 | US-06 (Visualizar alerta) | alertController.js | TC-009 (Alerta visible) |
| REQ-010 | US-07 (Consultar inventario) | inventoryController.js | TC-010 (Visualizacion exitosa) |
| REQ-011 | US-08 (Visualizar historial) | historyController.js | TC-011 (Historial visible) |

---

## 7. CRITERIOS DE RECHAZO DE REQUISITOS

| Requisito Rechazado | Razón | Justificacion |
|---------------------|-------|---------------|
| "El sistema debe predecir la demanda futura de productos" | Inalcanzable | No es posible predecir con exactitud la demanda sin un sistema de inteligencia artificial complejo, fuera del alcance del proyecto |
| "La interfaz debe ser sumamente amigable" | No testeable | El termino "sumamente amigable" es subjetivo y no puede medirse objetivamente |

---

## 8. NOTAS DEL EQUIPO

**Preguntas pendientes para el cliente:**

- Que metodo de autenticacion se utilizara? (correo/contrasena, inicio de sesion unico, etc.)
- Se requiere la importacion/exportacion de productos desde archivos CSV?
- El sistema debe generar reportes en PDF o solo visualizacion en pantalla?
- Cuantos usuarios concurrentes se esperan en promedio?

**Suposiciones del equipo:**

- Se asume que el cliente ya tiene PostgreSQL configurado segun lo indicado
- Se asume que los codigos de barra seran ingresados manualmente por el almacenista
- Se asume que las alertas de stock bajo seran visibles unicamente dentro del sistema (sin correos electronicos ni SMS)
- Se asume que el sistema sera usado principalmente en horario comercial

---

## 9. INFORMACION DEL PROYECTO

| Elemento | Detalle |
|----------|---------|
| Nombre del sistema | Sistema de Control de Inventario Tienda Local |
| Cliente | Tienda local (nombre pendiente de confirmacion) |
| Equipo de desarrollo | Oscar Eduardo Guerrero Menendez |
| Repositorio Git | https://github.com/Eduarsystems/Eduarpotato |
| Fecha de entrega | 15/07/2026 |
| Version del documento | v1.0-SRS |

---

"El cliente no siempre tiene la razon, pero siempre tiene el problema."

$ git tag -a v1.0-srs -m "SRS base approved"