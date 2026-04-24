---
name: ingesta-contexto
description: Procesa archivos externos que el usuario coloca en `Boveda/onboarding-data/` (exports de Claude, ChatGPT, CV en PDF, etc.) y destila el contexto útil en memorias. Identifica proyectos recurrentes, contactos habituales, preferencias detectadas, y rellena user_*, project_* y reference_* sin necesidad de que el usuario cuente todo a mano. Activar cuando el usuario escriba "/ingesta-contexto", "/ingesta" o "procesa los exports".
user_invocable: true
command: ingesta-contexto
---

# Ingesta de contexto externo

Cargar meses de contexto ya existente del usuario para que la memoria arranque rica desde el día 1.

## Qué procesa

Todo lo que el usuario haya colocado en `Boveda/onboarding-data/`. Formatos esperados:

| Fuente | Formato típico | Qué extraer |
|---|---|---|
| Export Claude | `.zip` o `.json` con conversaciones | Proyectos recurrentes, temas de interés, contactos mencionados, tono habitual del usuario |
| Export ChatGPT | `.zip` con `conversations.json` | Idem |
| CV / LinkedIn PDF | `.pdf` | Trayectoria profesional, skills, idiomas, experiencia |
| Apple Notes / Google Keep export | `.html` / `.txt` | Notas personales, ideas, listas |
| Chats WhatsApp/Telegram export | `.txt` / `.zip` | Contactos frecuentes, estilo de comunicación |

## Protocolo

### 1. Escaneo inicial

1. Listar contenido de `Boveda/onboarding-data/`. Si está vacío, avisar al usuario:
   > "La carpeta `Boveda/onboarding-data/` está vacía. Coloca ahí los exports que quieras procesar y vuelve a ejecutar `/ingesta-contexto`. ¿Te explico cómo descargar los exports de Claude/ChatGPT?"

2. Agrupar archivos por fuente detectada (por extensión + contenido). Reportar al usuario:
   ```
   📥 Detectado en onboarding-data/:
     - export_claude.zip (1 archivo)
     - conversations.json (ChatGPT)
     - CV_2026.pdf (1 archivo)
   ```

3. Confirmar con el usuario antes de procesar:
   > "Voy a procesar estos 3 archivos. Tardará unos minutos. El contenido se destila en memorias y los archivos originales los muevo a `_archivo_onboarding/` al terminar. ¿OK?"

### 2. Procesamiento por fuente

#### Export Claude (`.zip` con `conversations.json`)

1. Descomprimir si es `.zip`.
2. Leer `conversations.json`. Cada entrada es una conversación con mensajes.
3. Para cada conversación: extraer primer mensaje del usuario + título → clasificar por tema.
4. Identificar temas recurrentes (contar apariciones):
   - Proyectos mencionados (nombres propios, empresas, productos repetidos)
   - Personas mencionadas (contactos habituales)
   - Tools/plataformas recurrentes (Obsidian, Figma, Excel, etc.)
   - Preguntas repetidas (indica áreas donde el usuario necesita apoyo frecuente)
5. Filtrar:
   - **Incluir en memoria:** patrones que aparecen en >2 conversaciones distintas.
   - **Descartar:** conversaciones únicas o preguntas factuales sueltas.
6. Generar `project_*` para proyectos recurrentes identificados.
7. Actualizar `user_perfil.md` con contexto adicional detectado.
8. Crear `reference_*` con herramientas/sistemas externos que el usuario usa.

#### Export ChatGPT (`conversations.json`)

Mismo protocolo que Claude. El JSON tiene estructura ligeramente distinta — la identificación de temas es la misma.

#### CV / LinkedIn PDF

1. Extraer texto del PDF.
2. Identificar secciones estándar: experiencia, educación, skills, idiomas, ubicación.
3. Actualizar `user_perfil.md` con:
   - Trayectoria resumida (3-5 líneas con los roles principales)
   - Skills/tecnologías clave
   - Idiomas
4. Crear `user_profesional.md` con el detalle si es denso (empresas, años, logros destacables).

#### Otras fuentes

Procesar con criterio similar: extraer señales útiles, descartar ruido, generar memorias estructuradas.

### 3. Reporte al usuario

Tras procesar TODO, resumir en ~10-15 líneas:

```
✅ Ingesta completada.

De tus exports de Claude y ChatGPT he identificado:
- 8 proyectos recurrentes (he creado memorias para los 4 más activos: X, Y, Z, W)
- 12 contactos/nombres que aparecen >3 veces (top 5: ...)
- Herramientas que usas habitualmente: Figma, Notion, Excel, ...
- Tu estilo de comunicación suele ser: directo, prefieres respuestas concretas, odias las listas con viñetas largas

De tu CV he actualizado:
- Tu trayectoria profesional en user_profesional.md
- Tus skills principales en user_perfil.md

Memorias creadas/actualizadas:
  [lista de archivos]

Archivos procesados movidos a `_archivo_onboarding/` (puedes borrarlos cuando quieras).

¿Reviso alguna memoria contigo o arrancamos a trabajar?
```

### 4. Limpieza

Tras confirmar con el usuario:
1. Mover archivos originales de `onboarding-data/` a `_archivo_onboarding/` (por si quiere conservarlos) O borrarlos si el usuario lo pide.
2. NO borrar automáticamente sin preguntar.

## Reglas críticas

### Privacidad

- **Los archivos NO salen del PC del usuario.** Todo procesamiento es local.
- Si detectas en los exports **información sensible** (números de cuenta, contraseñas, datos médicos, DNI) → **NO los incluyas en memoria**. Avisa al usuario: *"He detectado datos sensibles en X archivo. No los he guardado en memoria. Recomiendo que los borres manualmente del export antes de guardarlo."*

### Anti-alucinación

- Si un patrón aparece solo 1-2 veces, **no lo generalices** a memoria. Podría ser un tema único que no repetirá.
- Cita fechas y contextos cuando sea posible ("en una conversación de enero de 2026 mencionaste X").
- Si una memoria existente **contradice** algo del export, NO sobrescribas silenciosamente. Pregunta al usuario: *"En tu memoria tenías X, pero en exports aparece Y. ¿Actualizo?"*

### Volumen razonable

- No crees más de 5 memorias por ejecución sin preguntar. Si detectas 20 proyectos recurrentes, prioriza los 5 más frecuentes y reporta el resto para que el usuario decida.

### Duplicados

- Si una memoria ya existe (`project_<slug>.md`) y los exports la enriquecen → **fusionar contenido**, no crear duplicado.

## Cuándo NO usar esta skill

- Si la carpeta `onboarding-data/` está vacía → decirlo y parar.
- Si el usuario no tiene exports y quiere montar todo a mano → redirigir a `/onboarding`.
- Si es una re-ingesta (ya se procesó antes los mismos archivos) → avisar al usuario y preguntar si quiere sobrescribir o sumar.
