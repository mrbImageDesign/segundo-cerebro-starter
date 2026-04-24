# Proyectos

Un subdirectorio por cada proyecto activo. Cada proyecto tiene su propio `README.md` con:

- **Qué es** (1-2 líneas)
- **Estado** (arrancando / en curso / en pausa / cerrado)
- **Stakeholders** (si hay más gente involucrada)
- **Tareas** (lista con `- [ ]`)
- **Notas** (cualquier cosa relevante)

Ejemplo de estructura de un proyecto:

```
Proyectos/
└── mi-proyecto-x/
    ├── README.md
    ├── reuniones/
    ├── documentos/
    └── ideas.md
```

## Crear un proyecto nuevo

Díle a Claude: *"Quiero empezar un proyecto nuevo llamado X"*. Él crea la carpeta, el README inicial, y la memoria persistente correspondiente (`project_<slug>.md`).
