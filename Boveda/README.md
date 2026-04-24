# Mi Bóveda

Esta es tu bóveda de Obsidian. Ábrela desde Obsidian con "Open folder as vault".

## Estructura

| Carpeta | Qué va aquí |
|---|---|
| `Personal/` | Datos personales, contactos, finanzas, salud, documentos |
| `Proyectos/` | Un subdirectorio por cada proyecto activo |
| `Daily Notes/` | Notas diarias (Obsidian las crea automáticamente) |
| `Research/` | Investigaciones, lecturas largas, referencias |
| `pendientes-claude/` | Buzón temporal: ideas, clips, cosas por clasificar |
| `onboarding-data/` | Temporal: exports de Claude/ChatGPT/CV para ingesta inicial |
| `assets/` | Multimedia (imágenes, PDFs, etc.) |

## Cómo va creciendo

1. Usas Claude Code normal. Cada conversación puede generar notas en las carpetas correspondientes.
2. Cuando capturas ideas sueltas o enlaces desde el móvil (fase opcional), caen en `pendientes-claude/`.
3. Periódicamente dices `/pendientes` a Claude y él las clasifica a su destino final.
4. De vez en cuando dices `/lint-vault` para auditar qué se está quedando olvidado.

## Principios

- **Cada nota tiene un lugar claro.** Si dudas dónde va, déjala en `pendientes-claude/por-clasificar/` y yo te ayudo.
- **Usa `[[enlaces]]`** cuando menciones conceptos que tienen (o podrían tener) su propia nota. Obsidian construye el graph automáticamente.
- **Frontmatter YAML** en las notas importantes (fecha, tipo, tags) — facilita buscar después.
- **La bóveda crece contigo.** No la llenes de golpe. Añade cosas según las necesitas.
