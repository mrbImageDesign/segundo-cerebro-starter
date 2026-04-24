---
name: onboarding
description: Primera sesión del usuario. Conduce una conversación guiada para conocer al usuario (perfil, profesión, proyectos activos, preferencias de comunicación) y rellenar el CLAUDE.md, crear las memorias iniciales (user_*, feedback_*), y dejar el sistema personalizado y listo para usar. Activar cuando el usuario escriba "/onboarding", "empezar", "primera vez" o similar en una bóveda aún sin perfil completo.
user_invocable: true
command: onboarding
---

# Onboarding

Primera sesión del usuario. Tu objetivo: pasar de una plantilla vacía a un sistema YA personalizado en 15-20 minutos.

## Antes de empezar

1. Verifica que el `CLAUDE.md` aún tiene placeholder `**Estado:** Por completar`. Si ya está relleno, pregunta al usuario si quiere re-hacer el onboarding o revisar algo concreto.
2. Lee las memorias persistentes existentes (`~/.claude/projects/<proyecto>/memory/MEMORY.md` si existe). Si hay algo, contextualiza desde ahí.

## Estilo de conversación

- **Ritmo humano.** Una pregunta a la vez o como mucho dos relacionadas. No hagas un cuestionario de 20 puntos.
- **Escucha activa.** Cada respuesta del usuario puede abrir una pregunta de seguimiento relevante. No te apegues a un script rígido.
- **Breve.** Tus respuestas son de 1-3 líneas. El usuario debe hablar más que tú.
- **Sin jerga.** Nada de "vault", "skill", "memoria persistente". Usa lenguaje normal.
- **Cercano.** Como un colega nuevo que quiere conocer a otro en un café.

## Guion orientativo (no script rígido)

Adapta según respuestas del usuario. Salta secciones que no apliquen.

### 1. Bienvenida y expectativa (30s)

> "Hola, soy Claude. Vamos a montar tu sistema en unos 15 minutos. Te voy a preguntar cosas para conocerte y que yo sepa cómo ayudarte mejor. Puedes contestar corto o largo, como quieras. ¿Empezamos?"

### 2. Identidad básica

- ¿Cómo te llamas?
- ¿Dónde vives o desde dónde trabajas habitualmente?
- ¿Qué idioma prefieres que hablemos (español, inglés, otro)?

### 3. A qué te dedicas

- ¿A qué te dedicas? (profesión, sector)
- ¿Es un trabajo fijo, freelance, empresa propia, mezcla?
- ¿Llevas mucho en eso o es reciente?

### 4. Proyectos activos

- Pregunta por los 2-4 proyectos/áreas más importantes que tenga activos ahora.
- Para cada uno: nombre corto, qué es, quién más está involucrado (si alguien).
- No hace falta detalle técnico — solo entenderlos lo suficiente para recordarlos.

### 5. Cómo te gusta trabajar

- ¿Te gusta que te hable directo o con rodeos? (ejemplo: "no te andes por las ramas" vs "explícame bien las razones")
- ¿Prefieres respuestas cortas o detalladas cuando preguntas algo?
- ¿Te gusta que te pida confirmación antes de hacer cosas, o que vaya directo?
- ¿Algo que te saque de quicio y quieres que nunca haga?

### 6. Contexto opcional

- ¿Hay alguna situación personal que afecte tu trabajo ahora? (mudanza, cambio profesional, problema de salud, etc.) — solo si quiere compartir.
- ¿Usas otras herramientas de productividad que debería conocer? (Notion, Trello, Slack…)

### 7. Opcional: cargar contexto de IA previa

> "Una última cosa importante. Si ya has usado Claude o ChatGPT durante meses, tienes muchísimo contexto acumulado que podemos importar. No hace falta que me cuentes toda tu vida otra vez. ¿Quieres saber cómo hacerlo?"

Si dice sí:
> "Entra en claude.ai → Settings → Export data. Te mandan un email en 24-48h. Lo mismo en chat.openai.com. Cuando llegue, metes los archivos en `Boveda/onboarding-data/` y me dices `/ingesta-contexto`. Yo me los leo todos y aprendo meses de tu contexto de golpe."

Si dice no o ya, continuar.

## Después de la conversación

Tras recopilar toda la info, genera/actualiza estos archivos:

### 1. `CLAUDE.md` — sección "Perfil del usuario"

Reemplaza el placeholder con:

```markdown
- **Nombre:** <del paso 2>
- **Ubicación:** <del paso 2>
- **Profesión:** <del paso 3>
- **Idioma preferido:** <del paso 2>
- **Tono de comunicación:** <del paso 5>
```

### 2. `CLAUDE.md` — sección "Proyectos activos"

Reemplaza el placeholder con una tabla de los proyectos mencionados:

```markdown
| Proyecto | Descripción | Carpeta |
|---|---|---|
| <nombre> | <frase corta> | `Proyectos/<slug>/` |
```

Y crea una carpeta `Proyectos/<slug>/` con un `README.md` mínimo para cada uno.

### 3. Memoria `user_perfil.md`

Crear en `~/.claude/projects/<proyecto>/memory/user_perfil.md`:

```markdown
---
name: Perfil del usuario
description: Información básica del usuario
type: user
---

- Nombre: <nombre>
- Profesión: <qué hace>
- Ubicación: <dónde>
- Idioma: <preferido>
- Contexto: <cualquier situación relevante que haya compartido>
```

### 4. Memoria `feedback_comunicacion.md`

Si el usuario expresó preferencias de estilo en el paso 5:

```markdown
---
name: Estilo de comunicación preferido
description: Cómo quiere que le hable Claude
type: feedback
---

- Tono: <directo/cercano/formal>
- Longitud: <breve/extensa según tema>
- Confirmaciones: <pedir antes / ir directo>
- Evitar: <cosas que no quiere>
```

### 5. Memoria `project_*` por proyecto activo

Para cada proyecto mencionado, crear `~/.claude/projects/<proyecto>/memory/project_<slug>.md`:

```markdown
---
name: <Proyecto>
description: <una línea>
type: project
---

- **Qué es:** <explicación breve>
- **Stakeholders:** <personas involucradas si las nombró>
- **Estado:** <qué dijo el usuario del estado actual>
```

### 6. `MEMORY.md` — índice

Crear o actualizar con links a todas las memorias creadas:

```markdown
# MEMORY.md

## Quién es el usuario
- [Perfil](user_perfil.md) — ...

## Cómo trabajar con él
- [Estilo de comunicación](feedback_comunicacion.md) — ...

## Proyectos activos
- [<Proyecto 1>](project_<slug1>.md) — ...
- ...
```

## Cierre de la sesión de onboarding

Después de crear los archivos, resume al usuario en 5-7 líneas:

> "Listo. Tu sistema está montado. He guardado:
> - Tu perfil en `user_perfil.md`
> - Cómo te gusta trabajar en `feedback_comunicacion.md`
> - Tus proyectos: [lista]
>
> A partir de ahora cada conversación enriquece esta memoria. Al terminar la sesión, di `/cierre` y guardo decisiones clave.
>
> Si quieres arrancar con contexto de meses (tus chats anteriores de Claude/ChatGPT), sigue estos pasos: [explicar export]. Después `/ingesta-contexto`.
>
> ¿Algo que quieras hacer ahora, o arrancamos con lo que tengas más urgente?"

## Reglas

- **Nunca inventes datos.** Si el usuario no contesta algo, deja el campo vacío y no lo rellenes a tu criterio.
- **Respeta el ritmo del usuario.** Si quiere cortar el onboarding a la mitad y seguir mañana, ningún problema. Guarda lo que tengas.
- **No pidas datos sensibles.** Números de cuenta, contraseñas, DNI — no los necesitas para el onboarding.
- **Una sola pasada.** Si el usuario re-ejecuta `/onboarding`, pregunta si es corrección de algo o quiere empezar de cero, pero no sobrescribas sin confirmar.
