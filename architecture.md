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

## 3. Descripción de módulos
### 3.1 main.py
Punto de entrada de la aplicación que inicializa, la conexión a la base de datos, el agente de CLI y muestra el menú principal.

### 3.2 cli/menu.py
Controla el flujo de navegación de la interfaz de usuario.
| Función              | Descripción                                      |
|----------------------|--------------------------------------------------|
| `show_main_menu()`   | Muestra el menú principal y maneja la selección del usuario. |
| `menu_anadir_v()`    | Muestra el menú de adicion de vehículos.          |
| `menu_borrar_v()`    | Muestra el menú de eliminación de vehículos.     |
| `menu_listar_v()`    | Muestra el menú de listado de vehículos.          |
| `menu_editar_v()`    | Muestra el menú de edicion de vehículos.    |
| `menu_buscar_v()`    | Muestra el menú de búsqueda de vehículos.         |
| `menu_anadir_ruta()`    | Muestra el menú de adición de rutas.              |
| `menu_listar_rutas()`    | Muestra el menú de listado de rutas.              |
| `menu_borrar_ruta()`    | Muestra el menú de eliminación de rutas.         |
| `menu_editar_ruta()`    | Muestra el menú de edición de rutas.              |
| `menu_anadir_entrega()`    | Muestra el menú de adición de entregas.           |
| `menu_listar_entregas()`    | Muestra el menú de listado de entregas.           |
| `menu_borrar_entrega()`    | Muestra el menú de eliminación de entregas.        |
| `menu_editar_entrega()`    | Muestra el menú de edición de entregas.           |
---
### 3.3 cli/formatters.py
Contiene funciones de presentacion.
| Función              | Descripción                                      |
|----------------------|--------------------------------------------------|
| `format_vehicle(vehicle)`   | Formatea un objeto Vehicle para su presentación en consola. |
| `format_ruta(ruta)`       | Formatea un objeto Ruta para su presentación en consola.   |
| `format_entrega(entrega)` | Formatea un objeto Entrega para su presentación en consola. |

---
### 3.4 logic/models.py
Define la estructura de datos del dominio
``` python
@dataclass
class Vehicle:
    id_vehiculo : str
    model: str
    capacidad_bateria_total: float
    nivel_bateria_actual: float
    autonomia_maxima_km: int
    estado : enum('disponible', 'en ruta', 'mantenimiento', 'cargando')
``` 

---
### 3.5 logic/validators.py
Funciones de validación sin efectos secundarios. Devuelven `True` o lanzan `ValidationError` con el código de error correspondiente.

** Funciones **
| Función              | Regla Aplicada               |
|----------------------|------------------------------|
| `validate_model(model)` | El modelo no puede estar vacío. |
| `validate_capacidad_bateria(capacidad)` | La capacidad de la batería debe ser un número positivo. |
| `validate_km_autonomia(autonomia)` | La autonomía máxima debe ser un número positivo. |
| `validate_estado(estado)` | El estado debe ser uno de: 'disponible', 'en ruta', 'mantenimiento', 'cargando'. |

---
### 3.6 logic/services.py
Orquesta los casos de uso. Es el único punto de contacto entre CLI y repositorio.
| Método              | Caso de Uso               |
|----------------------|---------------------------|
| `add_vehicle(datos)`  | Agrega un nuevo vehículo a la flota. |
| `delete_vehicle(id)` | Elimina un vehículo de la flota. |
