# Pendientes para Claude

Carpeta donde aterriza todo lo que capturas durante el día y espera a que Claude lo procese junto contigo.

## Estructura

| Subcarpeta | Qué va aquí |
|---|---|
| `briefs/` | Paquetes completos de "idea tocha" con contexto + objetivo + material. Se procesan en sesión dedicada. |
| `clips-web/` | Extractos de URLs web (artículos, posts). |
| `clips-youtube/` | Transcripciones + resúmenes de vídeos de YouTube. |
| `ideas/` | Ideas rápidas capturadas para pensar después. |
| `notas-sueltas/` | Notas rápidas sin destino concreto. |
| `por-clasificar/` | Cajón de sastre — cosas que no supiste dónde meter. |

## Cómo procesar

Cuando acumules cosas (normalmente a partir de 3-5 archivos), dile a Claude:

```
/pendientes
```

Él lee cada archivo, te propone un destino final en la bóveda, y tras tu OK lo mueve a su sitio.

## Regla de oro

**Nada en `pendientes-claude/` es permanente.** Todo lo que entra aquí tiene que salir (procesado, reubicado o descartado) en algún momento. Si algo lleva más de 2 semanas aquí sin moverse, probablemente ya no es relevante — Claude te lo avisará con `/lint-vault`.
