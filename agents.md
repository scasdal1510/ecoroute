---
title: "Programa de gestion de flota de vehiculos electricos"
version: "0.1"
date: "12/05/2026"
---
# agents.md - Ecoroute

## Vision general 

Este documento describe los agentes (roles de IA o automatizados) que pueden operar sobre la aplicación de gestión de flota de vehículos. Cada agente tiene un propósito concreto, un conjunto de herramientas permitidas y restricciones explícitas.

| Rol | Responsabilidad |
| :--- | :--- |
| **Encargado** | Gestionar la flota de vehículos, incluyendo la planificación de rutas, el monitoreo del estado de los vehículos y la gestión de entregas. |
| **Sistema de planificación de rutas** | Generar rutas óptimas para los vehículos eléctricos, considerando factores como el consumo de batería, el tráfico y las condiciones climáticas. |
| **Sistema de monitoreo de vehículos** | Supervisar el estado de los vehículos en tiempo real, incluyendo la ubicación, el nivel de batería y posibles fallas. |
| **Sistema de gestión de entregas** | Coordinar las entregas, asignar tareas a los vehículos y asegurar que las entregas se realicen a tiempo. |

## Diagrama de interaccion

```
graph TD
    Encargado -->|Planificar rutas| SistemaPlanificacion
    Encargado -->|Monitorear vehículos| SistemaMonitoreo
    Encargado -->|Gestionar entregas| SistemaGestion
    SistemaPlanificacion -->|Generar rutas óptimas| Encargado
    SistemaMonitoreo -->|Proporcionar datos en tiempo real| Encargado
    SistemaGestion -->|Coordinar entregas| Encargado
```
