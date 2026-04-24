# Segundo Cerebro Starter

> Tu asistente IA que **ya te conoce**. Arranca en 30 minutos sobre Claude Code + Obsidian.

## ¿Qué es esto?

Una plantilla universal para construir tu propio sistema de memoria personal con IA. Resuelve el problema real de trabajar con Claude, ChatGPT o cualquier asistente: **cada chat empieza de cero y pierdes tiempo repitiendo contexto**.

Con este starter:

- Claude **te conoce**: sabe quién eres, en qué proyectos andas, con quién trabajas, cómo te gusta que te hable.
- Cada conversación **enriquece su memoria** automáticamente.
- Puedes cargar contexto ya existente: tus historiales de Claude/ChatGPT, tu CV, tu Gmail, tu calendario.
- Vives el día a día con un asistente que recuerda TODO.

No es un producto SaaS. Es una plantilla gratuita y open source que montas en tu PC.

## ¿Para quién?

- Trabajas con IA a diario y pierdes horas repitiendo contexto.
- No eres programador pero estás cómodo instalando software.
- Quieres un sistema que crezca contigo durante años, no una herramienta más.
- Valoras tu privacidad: **todos tus datos quedan en tu ordenador** (no subidos a nadie).

Ejemplos reales de usuarios:

- Arquitecto gestionando obras, clientes y presupuestos
- Abogado con decenas de casos activos
- Profesor preparando cursos y apuntes
- Freelance con varios proyectos paralelos
- Cualquiera que use Claude/ChatGPT >1h al día

## Lo que necesitas

| Requisito | Coste |
|---|---|
| **Claude Pro** (suscripción) | ~20€/mes (obligatorio) |
| **Obsidian** (app de notas) | Gratis |
| **Claude Code** (agente IA local) | Gratis (incluido en Claude Pro) |
| **Git** (control de versiones) | Gratis |
| Un ordenador (Windows/Mac/Linux) | el que ya tienes |

## Instalación — paso a paso

### 1. Instalar lo básico (15 min)

1. **Obsidian** → https://obsidian.md/download — descarga e instala la versión gratuita.
2. **Claude Code** → https://docs.claude.com/claude-code — sigue las instrucciones de tu sistema operativo.
3. **Git** → https://git-scm.com/downloads — si no lo tienes ya.

### 2. Clonar este starter (2 min)

Abre una terminal en la carpeta donde quieres tener tu segundo cerebro (ej: `~/Documentos/`):

```bash
git clone https://github.com/<TU_USUARIO>/segundo-cerebro-starter mi-segundo-cerebro
cd mi-segundo-cerebro
```

### 3. Abrir Obsidian sobre la carpeta `Boveda/` (1 min)

- Abre Obsidian.
- "Open folder as vault" → selecciona la carpeta `Boveda/` dentro del repo clonado.
- Ya tienes tu vault con la estructura lista.

### 4. Abrir Claude Code en el repo (1 min)

En la terminal, dentro de la carpeta del repo, escribe:

```bash
claude
```

Claude Code arrancará y leerá automáticamente el `CLAUDE.md`.

### 5. Primera conversación — onboarding (15 min)

Escribe a Claude:

```
/onboarding
```

Te hará preguntas sobre quién eres, a qué te dedicas, qué proyectos tienes, cómo te gusta trabajar. Responde normal, como si hablaras con alguien. En 15 minutos tu sistema queda personalizado con tu perfil, tus proyectos y tus preferencias.

### 6. (Opcional pero muy recomendado) Cargar contexto externo (45-60 min)

Si ya usas Claude/ChatGPT desde hace meses, no pierdas ese contexto. Antes de empezar:

1. **Export de Claude:** entra en claude.ai → Settings → Export data. Te mandan un email con un archivo en 24-48h.
2. **Export de ChatGPT:** entra en chat.openai.com → Settings → Data controls → Export data. Idem.
3. **Tu CV en PDF** — descárgalo si tienes.

Cuando lleguen los archivos, colócalos en `Boveda/onboarding-data/` y pídele a Claude:

```
/ingesta-contexto
```

Claude leerá todo, identificará tus proyectos recurrentes, contactos habituales, preferencias, y rellenará tu memoria con **meses de contexto** sin que tengas que contárselo.

## Cómo funciona en el día a día

- **Usas Claude Code normal** (preguntas, pides ayuda, trabajas).
- Cuando Claude aprende algo nuevo sobre ti o tu trabajo, **lo guarda en memoria automáticamente**.
- Al terminar la sesión, escribes:
  ```
  /cierre
  ```
  Claude persiste decisiones, avances y contexto nuevo. La siguiente sesión arranca sabiendo todo lo de la anterior.
- Si capturas ideas sueltas o enlaces durante el día, van a `Boveda/pendientes-claude/`. Cuando quieras, le dices:
  ```
  /pendientes
  ```
  Claude los clasifica y los manda a su sitio final.
- Para auditar el estado de la bóveda (archivos olvidados, notas huérfanas):
  ```
  /lint-vault
  ```

## Skills incluidas

| Skill | Para qué |
|---|---|
| `onboarding` | Primera sesión — genera tu perfil inicial. |
| `ingesta-contexto` | Procesa exports Claude/ChatGPT/CV → memoria rica desde el día 1. |
| `cierra-sesion` | Persiste aprendizaje al cerrar una sesión. |
| `procesa-pendientes` | Vacía el buzón `pendientes-claude/`. |
| `lint-vault` | Audita la bóveda: huérfanos, olvidados, duplicados. |

## Privacidad

- **Todo se queda en tu ordenador.** No subimos tus datos a ningún servidor externo.
- El repo Git es opcional (para hacer backup en GitHub o sincronizar entre dispositivos). Puedes usarlo solo local.
- Claude API sí procesa tus mensajes cuando conversas (eso va por su servidor). Pero tu memoria, notas y archivos quedan en tu disco.

## Amplía cuando quieras

Este starter es la **base cognitiva**. Si más adelante quieres:

- Captura desde el móvil vía Telegram
- Bot con tu contexto que te responda lejos del PC
- Procesamiento automático de vídeos de YouTube
- Conectores con Gmail, Calendar, Drive

Todo eso se puede añadir después. El starter está pensado para que empieces simple y crezca contigo.

## Licencia

MIT — úsalo libremente, modifícalo, compártelo. Si te sirve, menciona el origen.

## Soporte

- **Issues en GitHub** para problemas técnicos.
- **Discussions** para ideas, casos de uso, preguntas.
- Si encuentras algo que te ha servido y crees que puede ayudar a otros → manda un PR.

---

**Construido tras meses de uso real. Sin humo, sin magia. Solo una forma mejor de trabajar con IA.**
