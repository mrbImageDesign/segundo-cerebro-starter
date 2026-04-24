---
name: procesa-pendientes
description: Procesa lo acumulado en `Boveda/pendientes-claude/` (ideas, clips-web, clips-youtube, notas-sueltas, briefs, por-clasificar). Lee cada MD, propone destino final en el vault según contenido, y mueve tras confirmación del usuario. Activar cuando el usuario diga "/pendientes", "procesa pendientes", "procesa lo acumulado" o similar.
user_invocable: true
command: pendientes
---

# Procesa pendientes-claude

Protocolo para vaciar el buzón de `Boveda/pendientes-claude/` y reubicar cada archivo en su sitio final del vault.

## Entrada

Las 6 subcarpetas en `Boveda/pendientes-claude/`:

| Subcarpeta | Contenido típico |
|---|---|
| `ideas/` | Ideas capturadas (para ejecutar, revisar o descartar) |
| `clips-web/` | Contenido de URLs web extraído |
| `clips-youtube/` | Resúmenes de vídeos YouTube sin clasificación clara |
| `notas-sueltas/` | Notas rápidas sin destino |
| `briefs/` | Paquetes completos tipo brief para sesión dedicada |
| `por-clasificar/` | Cajón de sastre |

## Protocolo

### 1. Escaneo

Listar todos los archivos `.md` en las 6 subcarpetas. Agrupar por subcarpeta con conteo:

```
📥 Pendientes actuales:
  ideas/        N archivos
  clips-web/    N
  clips-youtube/ N
  notas-sueltas/ N
  briefs/       N
  por-clasificar/ N
Total: X archivos
```

### 2. Procesamiento por lotes (máx 5 por tanda)

Para cada archivo:

1. **Leer contenido completo** (frontmatter + cuerpo)
2. **Extraer señales clave:** topics, herramientas mencionadas, referencias, URL origen, palabras clave del título
3. **Cross-check con memorias del usuario** (`project_*.md` en memoria persistente): si algún proyecto activo del usuario matchea con señales del archivo → proponer destino dentro de ese proyecto.
4. **Decidir destino propuesto** basado en:
   - Memorias de proyectos activos del usuario
   - Estructura actual de `Boveda/` (carpetas que ya existen)
   - Contenido del archivo

**Criterios generales de clasificación:**

| Señal | Destino típico |
|---|---|
| Relacionado con un proyecto activo del usuario | `Proyectos/<proyecto>/` (según memoria del usuario) |
| Tutorial técnico, curso, herramienta que el usuario está aprendiendo | `Personal/Cursos/<tema>/` o `Research/tutoriales/` |
| Referencia útil para el trabajo del usuario | `Research/referencias/` |
| Dato personal (salud, finanzas, vida) | `Personal/<tema>/` |
| Idea genérica sin contexto claro | `Research/ideas/` |
| Sin match claro | Dejar en `por-clasificar/` y preguntar al usuario |

5. **Verificar si ya existe archivo similar** en el destino. Si sí: sugerir merge en vez de duplicar.

6. **Reportar al usuario con formato compacto:**

```
[1/5] ideas/2026-04-24_comando-nuevo.md
  → Propuesta: Proyectos/<tu-proyecto>/ideas-funcionalidades.md (merge)
  → Razón: habla de mejorar el proyecto X que tienes activo
  → ¿OK, cambiar destino, o descartar?
```

### 3. Confirmación por lote

El usuario responde con uno de:
- `ok` → mover todos los propuestos
- `ok excepto 2,4` → mover todos menos los números indicados
- `2: <ruta>` → cambiar destino del archivo 2
- `descartar 3` → borrar el archivo 3
- `paro` → cancelar resto del lote

### 4. Ejecución

Para cada archivo confirmado:
1. Leer contenido
2. Si destino es carpeta nueva: crear archivo con nombre descriptivo
3. Si destino es archivo existente: añadir sección al final con fecha y fuente original
4. Mover el MD original (no copiar) — desaparece de `pendientes-claude/`
5. Si tenía `tipo: idea` en frontmatter, añadir sección al README del proyecto destino con `- [ ] <título>` como tarea pendiente

### 5. Reporte final

```
✅ Tanda 1/N completada:
  Movidos: 4
  Merged: 1
  Descartados: 0
  Pendientes: X archivos
¿Sigo con siguiente tanda?
```

## Reglas

- **Nunca** mover sin confirmar propuesta.
- **Nunca** crear estructura de carpetas nueva sin avisar.
- Si un archivo lleva >14 días en `pendientes-claude/`, sugerir descartarlo (probablemente ya no es relevante).
- Si hay duplicados exactos (mismo contenido en dos archivos), quedarse con el más reciente y borrar el viejo.
- Los archivos dentro de `briefs/` NUNCA se procesan automáticamente — requieren sesión de trabajo explícita.

## Referencias cruzadas

- `Boveda/pendientes-claude/README.md` — estructura de subcarpetas y regla de oro.
