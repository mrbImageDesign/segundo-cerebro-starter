---
name: cierra-sesion
description: Cierre de sesión con acumulación de contexto. Extrae decisiones, avances y contexto nuevo de la conversación actual y los persiste en la bóveda y en la memoria persistente de Claude. Activar cuando el usuario diga "/cierre", "cierra sesión", "cerramos" o cuando la conversación sea larga y esté concluyendo.
user_invocable: true
command: cierre
---

# Cierre de sesión

Cerrar una sesión guardando todo el contexto relevante para que la siguiente arranque sabiéndolo.

## Pasos

### 1. Resumen de la conversación

Extrae los puntos relevantes de lo que ha pasado en esta sesión:

- **Decisiones tomadas** (tecnológicas, de negocio, personales).
- **Ideas importantes discutidas** que valen para más adelante.
- **Cambios en la situación del usuario** (nuevas preferencias, correcciones, eventos relevantes).
- **Avances concretos en proyectos** (tareas completadas, milestones, bloqueos resueltos).
- **Tareas pendientes nuevas** que han surgido.

### 2. Clasificar y persistir

Según el tipo de contenido:

#### Información sobre el usuario (perfil, preferencias, situación)

- Actualizar la memoria correspondiente (`user_*.md`, `feedback_*.md`) en `~/.claude/projects/<proyecto>/memory/`.
- Si aparece un patrón nuevo de cómo quiere trabajar → nuevo `feedback_*.md` con **Why:** y **How to apply:**.

#### Avances o decisiones de proyecto

- Actualizar `Boveda/Proyectos/<proyecto>/README.md`:
  - Marcar tareas completadas con `- [x]`
  - Añadir tareas nuevas con `- [ ]`
- Si es info no-obvia sobre el estado del proyecto → actualizar o crear `project_<slug>.md` en memoria.

#### Ideas o conceptos nuevos sin proyecto asignado

- Crear nota en `Boveda/Daily Notes/` con la fecha del día (`YYYY-MM-DD.md`) con el contenido.
- Si la idea es accionable para más adelante → mover a `Boveda/pendientes-claude/ideas/`.

#### Referencias a sistemas externos

- Nuevo `reference_*.md` en memoria con URL/descripción de qué es y para qué sirve.

### 3. Sincronizar con Git (si el repo está configurado)

Si el usuario ha configurado Git para sincronizar entre dispositivos:

```bash
git add -A && git commit -m "cierre de sesión $(date +%Y-%m-%d)" && git push
```

Si no hay remote configurado, avisar brevemente (solo una vez por usuario, no cada sesión): *"Tu repo no tiene remote. Si quieres sincronizar con GitHub entre dispositivos, te explico cómo."*

### 4. Confirmación al usuario

Reportar en **máximo 3 líneas**: qué se guardó, dónde, y cualquier pendiente crítico para la próxima sesión.

Ejemplo:
```
✅ Cerrado. Guardado: avance en Proyecto X (README actualizado),
nueva preferencia de comunicación (feedback_formato.md),
y 2 ideas en pendientes-claude/ideas/.
Próximo arranque: completar la tarea Y.
```

## Reglas

- **No guardar información trivial ni duplicada.** Si ya existe en memoria o en el vault, no lo repitas.
- **Priorizar actualizar sobre crear.** Antes de crear un archivo nuevo, busca si hay uno relacionado que puedas enriquecer.
- **Si no hubo nada relevante que persistir, dilo directo**: *"No hay cambios que guardar de esta sesión."*
- **El cierre es rápido.** No una retrospectiva de 20 líneas. Captura lo esencial y fuera.
- **No inventes contexto.** Solo persiste lo que el usuario realmente dijo o decidió.

## Activación proactiva

Si llevas >15 intercambios en la conversación o se acaba de concluir un tema importante, propón:

> "¿Cerramos sesión? Puedo guardar lo relevante de lo que hemos hablado."

Espera que el usuario confirme antes de ejecutar.
