---
title: "Programa de gestion de flota de vehiculos electricos"
version: "0.1"
date: "12/05/2026"
---
# architecture.md - Arquitectura técnica

## Visión general
La aplicacion sigue una arquitectura en capas (Layered Architecture) para separar la lógina de presentacion de datos con tres niveles bien diferenciados. La comunicacion entre capas es siempre descendente: la capa superior puede llamar a la capa inferior, pero no al revés.

```
┌─────────────────────────────────────────────────────────┐
│                   CAPA DE PRESENTACIÓN                  │
│                   cli/  (cli_agent)                     │
│   main.py  ·  menu.py  ·  formatters.py                 │
└───────────────────────┬─────────────────────────────────┘
                        │  llama a
┌───────────────────────▼─────────────────────────────────┐
│                 CAPA DE LÓGICA DE NEGOCIO               │
│                logic/  (logic_agent)                    │
│     services·py  validators.py  ·  models.py    │
└───────────────────────┬─────────────────────────────────┘
                        │  llama a
┌───────────────────────▼─────────────────────────────────┐
│                   CAPA DE DATOS                         │
│                   db/  (db_agent)                       │
│   connection.py  ·  vehicle_repo.py  ·  migrations/     │
└───────────────────────┬─────────────────────────────────┘
                        │
                    ┌───▼───┐
                    │ MySQL │
                    └───────┘
```

## Estructura de carpetas

```
eco_route/
|
├── main.py         # Punto de entrada de la aplicación
|
|
├── cli/                # Capa de presentación
│   ├── cli_agent.py    # Agente de CLI para interactuar con el usuario
│   ├── menu.py         # Funciones para mostrar menús y opciones
│   └── formatters.py   # Funciones para formatear la salida en consola
├── logic/              # Capa de lógica de negocio
│   ├── logic_agent.py   # Agente de lógica para procesar la lógica de negocio
│   ├── services.py     # Servicios que implementan la lógica de negocio
│   ├── validators.py   # Validadores para asegurar la integridad de los datos
│   └── models.py       # Definición de modelos de datos (e.g., Vehicle)
├── db/                 # Capa de datos
│   ├── db_agent.py     # Agente de base de datos para interactuar con MySQL
│   ├── connection.py   # Configuración de la conexión a la base de datos
│   ├── vehicle_repo.py  # Repositorio para operaciones CRUD de vehículos
│   └── migrations/     # Scripts de migración para la base de datos
├── test/
│   ├── __init__.py   # Archivo de inicialización para el paquete de pruebas
|   ├── test_validators.py    # Pruebas para los validadores


```