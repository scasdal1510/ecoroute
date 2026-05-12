# SPEC.md - EcoRoute

## 1. Visión general

Herramienta para gestionar flota de furgonetas eléctricas.

## 2. Autores

| Autores | Descripcion |
|---------|-------------|
|Encargado| Persona encargada de usar la herramienta |

## 3. Casos de uso

### CU-01 Registrar vehículos ##
  1. El encargado registra los vechículos ingresando modelo, capacidad de batrería y autonomía
  2. El sistema valida los datos.
  3. El sistema guarda los datos.
  - Flujo alternativo:
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido".
    - Si el vehículo ya existe -> error: "El vehículo a registrar ya esta registrado".

### CU-02 Eliminar vehículos ##
  1. El encargado especifica mediante el el id el vehículo el cual quiera eliminar.
  2. El sistema busca el vehículo
  3. El sistema elimina el vehículo
  - Flujo alternativo:
    - Si el vehículo no existe -> error: "El vehículo no esta registrado".
### CU-03 Editar vehículo ##
  1. El encargado edita los campos que quiera.
  2. El sistema valida los datos.
  3. El sistema edita los datos.
  - Flujo alternativo:
    - Si el vehículo no existe -> error: "El vehículo no esta registrado".
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido".
## CU-04 Listar vehículos ##
  1. El encargado solicita la lista de vehículos.
  2. El sistema muestra la lista de vehículos registrados.

### CU-05 Lógica de seguridad ##
  1. El sistema detecta si la ruta consume mas del 80% de la batería.
  2. El sistema rechaza la ruta.
  - Flujo alternativo:
    - Si la ruta consume menos del 80% de la batería -> el sistema acepta la ruta.
### CU-06 Crear Entrega ##
  1. El encargado ingresa destino,peso, prioridad y ventana horaria.
  2. El sistema valida los datos.
  3. El sistema asigna la entrega a un vehículo.
  - Flujo alternativo:
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido".
### CU-07 Editar Entrega ##
  1. El encargado edita los campos que quiera.
  2. El sistema valida los datos.
  3. El sistema edita los datos.
  - Flujo alternativo:
    - Si la entrega no existe -> error: "La entrega no esta registrada".
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido
### CU-08 Eliminar Entrega ##
  1. El encargado pone el id de la entrega que quiera eliminar.
  2. El sistema busca la entrega.
  3. El sistema elimina la entrega.
  - Flujo alternativo:
    - Si la entrega no existe -> error: "La entrega no esta registrada".
  ### CU-09 Listar Entregas ##
  1. El encargado solicita la lista de entregas.
  2. El sistema muestra la lista de entregas registrados.
### CU-10 Generar Rutas ##
  1. El encargado añade los datos de la ruta.
  2. El sistema valida los datos.
  3. El sistema genera la ruta.
  - Flujo alternativo:
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido
### CU-11 Editar Rutas ##
  1. El encargado edita los campos que quiera.
  2. El sistema valida los datos.
  3. El sistema edita los datos.
  - Flujo alternativo:
    - Si la ruta no existe -> error: "La ruta no esta registrada".
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido
### CU-12 Eliminar Rutas ##
  1. El encargado pone el id de la ruta que quiera eliminar.
  2. El sistema busca la ruta.
  3. El sistema elimina la ruta.
  - Flujo alternativo:
    - Si la ruta no existe -> error: "La ruta no esta registrada".
### CU-13 Listar Rutas ##
  1. El encargado solicita la lista de rutas.
  2. El sistema muestra la lista de rutas registrados.  

  
## 4.Modelo de datos
```
Vehiculo {
  id_vehículo:  string   // Identificador único (ej: "VAN-001"). Debe seguir un patrón alfanumérico.
  modelo:   string   // Modelo del vehículo
  capacidad_batería:   float   // Capacidad de la batería en kWh
  nivel_batería:     float   // Porcentaje de carga actual de la batería.(0-100)
  autonomia_maxima:     int   // Cuantos km puede recorrer con 100% de carga.
  estado : string   // Estado del vehículo (disponible, en ruta, mantenimiento)
  }

  Entrega {
  id_entrega: string   // Identificador único del paquete.
  destino_coordenadas: string   // Ubicación exacta de la entrega.
  peso: float   // Influye en el consumo (opcional para aumentar dificultad).
  prioridad: int   // Prioridad de la entrega (1-3).
  ventana_horaria: string   // Ejemplo: "09:00 – 11:00".
  }
  Ruta {
  id_ruta: string   // ID de la jornada.
  vehiculo_asignado: fk  // Referencia al ID del vehículo..
  lista_entregas list // Lista ordenada de IDs de entregas.
  consumo_estimado_bateria: float   // Porcentaje que se restará tras completar la ruta.
  }
```
## 5. Reglas de negocio
| ID | Regla |
|----|-------|
| RN-01 | Un vehículo no puede ser asignado a más de una ruta al mismo tiempo. |
| RN-02 | El consumo estimado de batería para una ruta no puede exceder el 80% de la capacidad total del vehículo asignado. |
| RN-03 | Las entregas con prioridad 1 deben ser programadas antes que cualquier otra |
| RN-04 | Las entregas deben ser programadas dentro de su ventana horaria especificada. |
| RN-05 | Si un vehículo está en mantenimiento, en ruta o cargando, no puede ser asignado a ninguna ruta hasta que su estado cambie a disponible. |

# 6. Requisitos no funcionales
| ID | Requisito |
|----|------------|
| RNF-01 | La aplicación debe ser accesible desde dispositivos móviles y de escritorio. |
| RNF-02 | El sistema debe responder a las acciones del usuario en menos de 100ms. |
| RNF-03 | Se debe implementar autenticación para garantizar que solo usuarios autorizados puedan acceder a la aplicación. |
| RNF-04 | Sin dependencias externas de red en runtime (funciona offline). |

# 7. Mensajes de error estándar
| Código de error | Mensaje de error |
|-----------------|------------------|
| ERR-001 | Tipo de datos inválido. |
| ERR-002 | El vehículo a registrar ya está registrado. |
| ERR-003 | El vehículo no está registrado. |
| ERR-004 | La entrega no está registrada. |

