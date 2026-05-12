# SPEC.md - EcoRoute

## 1. Visión general

Herramienta para gestionar flota de furgonetas eléctricas.

## 2. Autores

| Autores | Descripcion |
|---------|-------------|
|Encargado| Persona encargada de usar la herramienta |

## 3. Casos de uso

- **Registrar vehículos**
  1. El encargado registra los vechículos ingresando modelo, capacidad de batrería y autonomía
  2. El sistema valida los datos.
  3. El sistema guarda los datos.
  - Flujo alternativo:
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido".
    - Si el vehículo ya existe -> error: "El vehículo a registrar ya esta registrado".

- **Eliminar vehículos**
  1. El encargado especifica mediante el el id el vehículo el cual quiera eliminar.
  2. El sistema busca el vehículo
  3. El sistema elimina el vehículo
  - Flujo alternativo:
    - Si el vehículo no existe -> error: "El vehículo no esta registrado".

- **Editar vehículo**
  1. El encargado edita los campos que quiera.
  2. El sistema valida los datos.
  3. El sistema edita los datos.
  - Flujo alternativo:
    - Si el vehículo no existe -> error: "El vehículo no esta registrado".
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido".
- **Lógica de seguridad**
  1. El sistema detecta si la ruta consume mas del 80% de la batería.
  2. El sistema rechaza la ruta.
  - Flujo alternativo:
    - Si la ruta consume menos del 80% de la batería -> el sistema acepta la ruta.
- **Crear Entrega**
  1. El encargado ingresa destino,peso, prioridad y ventana horaria.
  2. El sistema valida los datos.
  3. El sistema asigna la entrega a un vehículo.
  - Flujo alternativo:
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido".
- **Editar Entrega**
  1. El encargado edita los campos que quiera.
  2. El sistema valida los datos.
  3. El sistema edita los datos.
  - Flujo alternativo:
    - Si la entrega no existe -> error: "La entrega no esta registrada".
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido
- **Eliminar Entrega**
  1. El encargado pone el id de la entrega que quiera eliminar.
  2. El sistema busca la entrega.
  3. El sistema elimina la entrega.
  - Flujo alternativo:
    - Si la entrega no existe -> error: "La entrega no esta registrada".
- **Generar Rutas**
  1. El encargado añade los datos de la ruta.
  2. El sistema valida los datos.
  3. El sistema genera la ruta.
  - Flujo alternativo:
    - Si el tipo de datos es invalido -> error: "Tipo de datos invalido

  
## 4.Modelo de datos
```
Vehículo {
  id_vehículo:  string   // Identificador único (ej: "VAN-001"). Debe seguir un patrón alfanumérico.
  modelo:   string   // Modelo del vehículo
  capacidad_batería:   float   // Capacidad de la batería en kWh
  nivel_batería:     float   // Porcentaje de carga actual de la batería.(0-100)
  autonomia_maxima:     int   // Cuantos km puede recorrer con 100% de carga.
  estado : string   // Estado del vehículo (disponible, en ruta, mantenimiento)
  }
```
