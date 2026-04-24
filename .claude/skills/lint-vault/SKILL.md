---
name: lint-vault
description: Audita el vault Obsidian detectando archivos abandonados en pendientes (>14 días), notas huérfanas (sin links entrantes ni salientes), carpetas con poca actividad y duplicados potenciales. Genera reporte accionable. Activar cuando el usuario diga "/lint-vault", "revisa la bóveda", "¿qué tengo olvidado?" o similar.
user_invocable: true
command: lint-vault
---

# Lint del vault

Auditoría no-destructiva del vault. NUNCA borra nada. Solo reporta y propone.

## Pasos

### 1. Pendientes olvidados (>14 días)

Recorrer `Boveda/pendientes-claude/**/*.md` y para cada archivo:
- Extraer fecha del frontmatter (`fecha:`) o del nombre del archivo (patrón `YYYY-MM-DD_*.md`)
- Si `hoy - fecha > 14 días` → marcar como candidato a limpieza

Reporte:

```
⏰ Pendientes >14 días (N archivos):
  - pendientes-claude/ideas/2026-04-08_...md (16 días)
  - pendientes-claude/clips-web/2026-04-05_...md (19 días)
  ...
```

Para cada uno: ofrecer **descartar**, **procesar** (invocar `/pendientes`) o **dejar**.

### 2. Huérfanos del graph (sin links)

Recorrer todos los `.md` de `Boveda/` (excluyendo `.obsidian/`, `assets/`, `_external_repos/`). Para cada archivo:
- Contar links **salientes**: `[[...]]` en el cuerpo
- Contar links **entrantes**: usar grep para ver cuántos otros archivos referencian este (por nombre de archivo sin extensión)
- Si `salientes == 0 AND entrantes == 0` → huérfano

Excluir de huérfanos:
- `README.md` de cualquier carpeta (son entrada)
- `CLAUDE.md`, `MEMORY.md` (son schema)
- Daily Notes (por naturaleza son logs, no siempre conectados)
- Archivos en `assets/`

Reporte:

```
🔗 Huérfanos del graph (N archivos):
  - Proyectos/X/2026-04-24_....md (0 links salientes, 0 entrantes)
  ...
```

Para cada uno: sugerir añadir 1-2 `[[links]]` a conceptos obvios del contenido, o mover a una carpeta con contexto.

### 3. Carpetas con poca actividad

Recorrer subcarpetas de `Boveda/` y contar archivos `.md` directos + fecha del último archivo modificado:
- Si una carpeta tiene 0 archivos (solo un README o nada) → "vacía"
- Si último archivo tiene >90 días → "dormida"

Reporte:

```
📂 Carpetas con poca actividad:
  - Research/asistentes_personales (vacía, solo README)
  - Proyectos/XYZ (último cambio hace 120 días, ¿sigue activo?)
```

### 4. Duplicados potenciales

Buscar archivos con nombres similares (Levenshtein < 3 o comparten primeras 30 chars del slug):

```
📑 Posibles duplicados:
  - .../tema-abc-parte-1.md
  - .../tema-abc-parte1.md
  (similitud 95% en título)
```

Sugerir merge manual.

### 5. Tamaño de memorias persistentes

Verificar que `MEMORY.md` en `~/.claude/projects/<proyecto>/memory/` tenga <200 líneas (límite de contexto cómodo). Si excede, sugerir ejecutar el skill `consolidate-memory` (si está disponible) o hacer una pasada manual.

## Reporte final

Formato compacto en ~15 líneas:

```
📊 Lint del vault — YYYY-MM-DD

⏰ Pendientes >14 días:        X (ver detalle arriba)
🔗 Huérfanos del graph:        X
📂 Carpetas dormidas:          X
📑 Duplicados potenciales:     X
📝 MEMORY.md:                  N líneas (límite cómodo 200)

Acciones propuestas:
  1. Descartar N pendientes viejos
  2. Añadir [[links]] a N huérfanos
  3. Revisar si proyectos X e Y siguen activos
  4. Merge de M duplicados

¿Empezamos por la acción 1?
```

## Reglas

- **NUNCA borrar archivos.** Solo reportar y proponer.
- Para mover archivos, invocar `/pendientes` si aplica.
- Si el vault tiene >5000 archivos, limitar escaneo a las carpetas modificadas en los últimos 30 días para no bloquear la sesión.
- No lintear archivos dentro de `_external_repos/`, `.git/`, `.obsidian/`, `node_modules/`.

## Frecuencia recomendada

- Manual: cuando el usuario lo pida.
- Automático (si el usuario quiere): una vez por semana como scheduled task.
