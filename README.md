---

# Sistema de Gestión de Reservas de Actividades Recreativas

## Objetivo

Desarrollar una aplicación web en Angular 18 para gestionar reservas de actividades recreativas, implementando lazy loading, guards para la navegación, routing, reactive forms con validaciones, y comunicación con una API REST.

## Requerimientos Técnicos

1. Angular CLI 18.x
2. Bootstrap 5.x
3. MockAPI

## Estructura del Proyecto

### 1. Routing

- Implementar las siguientes rutas principales:
    - `/book-activity`: Formulario para la creación de reservas de actividades
    - `/my-bookings`: Lista de reservas realizadas por el usuario (acceso restringido)
    - `/login`: Formulario de inicio de sesión (usuario y contraseña)
- Implementar un combo con roles que incluya "Admin" y "Empleado".
    - El rol de **Admin** podrá ver todas las reservas y actividades.
    - El rol de **Empleado** solo tendrá acceso a las rutas permitidas.
- Implementar lazy loading para las rutas de las actividades y las reservas.
- Implementar guards para proteger la ruta `/my-bookings`, permitiendo solo el acceso a usuarios autenticados.

### 2. Formulario Reactivo

#### 2.1 Estructura Base

- Crear un FormGroup que incluya:
    - Información del usuario:
        - Nombre completo (requerido, mínimo 3 caracteres)
        - Email (requerido, formato email válido)
        - Teléfono de contacto (requerido)
    - Datos de la actividad:
        - Actividad (requerido, select desde la API)
        - Fecha (requerido)
        - Hora de inicio (requerido)
        - Hora de finalización (requerido)
        - Cantidad de personas (requerido, mínimo 1)
    - FormArray de servicios adicionales (opcional):
        - Tipo de servicio (select desde la API)
        - Cantidad (número)
        - Precio (calculado automáticamente según servicio y cantidad)

#### 2.2 Validaciones

- Validar la disponibilidad de la actividad mediante un endpoint asincrónico GET `/availability` antes de reservar.

### 3. Listado de Reservas

- Tabla responsive que muestre:
    - Código de reserva
    - Nombre completo del usuario
    - Actividad reservada
    - Fecha y hora
    - Cantidad de personas
    - Total de la reserva
    - Estado (pendiente/confirmada/cancelada)
- Permitir búsqueda de reservas por nombre del usuario o código de reserva.

### 4. Algoritmo de Procesamiento

- Calcular el total de la reserva incluyendo costos de servicios adicionales, si los hay.
- Aplicar un descuento del 10% para grupos de más de 15 personas.
- Generar un código único de reserva combinando iniciales del usuario y un número aleatorio de 4 dígitos.

## Documentación de la API

### 1. Endpoint de Actividades
**URL**: `https://679b8dc433d31684632448c9.mockapi.io/activities`  
**Descripción**: Obtiene la lista de actividades disponibles para reservar.

#### Formato de Respuesta:
```json
[
  {
    "id": "a1",
    "name": "Escalada",
    "capacity": 20,
    "pricePerPerson": 30,
    "description": "Una emocionante experiencia de escalada en roca."
  }
]
```

### 2. Endpoint de Servicios
**URL**: `https://679b8dc433d31684632448c9.mockapi.io/services`  
**Descripción**: Obtiene los servicios opcionales disponibles.

#### Formato de Respuesta:
```json
[
  {
    "id": "s1",
    "name": "Guía de Montaña",
    "pricePerPerson": 15
  }
]
```

### 3. Endpoint de Disponibilidad
**URL**: `https://671fe287e7a5792f052fdf93.mockapi.io/availability`  
**Descripción**: Verifica la disponibilidad de actividades en fechas específicas.

#### Formato de Respuesta:
```json
[
  {
    "id": "1",
    "activityId": "a1",
    "date": "2024-10-28",
    "available": false
  }
]
```

### 4. Endpoint de Reservas
**URL**: `https://671fe287e7a5792f052fdf93.mockapi.io/bookings`  
**Descripción**: Crea una nueva reserva.

#### Formato de Solicitud:
```json
{
  "reservationCode": "JUAN1234",
  "userName": "Juan Pérez",
  "userEmail": "juan@example.com",
  "contactPhone": "+1234567890",
  "activityId": "a1",
  "reservationDate": "2024-10-28",
  "startTime": "10:00",
  "endTime": "12:00",
  "totalPeople": 10,
  "services": [
    {
      "serviceId": "s1",
      "quantity": 5,
      "pricePerPerson": 15
    }
  ],
  "totalAmount": 300,
  "status": "confirmed"
}
```

### Notas Importantes

- Todas las fechas deben estar en formato ISO 8601.
- Las horas deben estar en formato 24 horas.
- Los valores monetarios están en la unidad de moneda predeterminada.

## Tabla de Evaluación

| Criterio                         | Puntos |
|----------------------------------|--------|
| Implementación de Routing        | 20     |
| Formulario Reactivo              | 25     |
| Validaciones de Formulario       | 15     |
| Listado de Reservas              | 20     |
| Algoritmo de Procesamiento       | 20     |
| Estilos                          | 10     |
| **Total**                        | **100**|

---