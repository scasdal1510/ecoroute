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
- **Lógica de seguridad**

