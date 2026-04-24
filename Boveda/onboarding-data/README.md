# Onboarding data

Carpeta **temporal** para los archivos que vas a cargar durante el onboarding: exports de IA, CVs, etc.

## Qué meter aquí

- `export_claude.zip` o el archivo que te mandaron desde claude.ai
- `conversations.json` o el archivo de ChatGPT
- `CV.pdf` — tu currículum o perfil LinkedIn descargado
- Cualquier otro archivo que quieras que Claude procese una sola vez para destilarlo en memoria

## Cómo descargar los exports

### Claude
1. Entra en claude.ai → avatar arriba derecha → **Settings**
2. Pestaña **Privacy** → **Export data**
3. Te llega un email con un enlace de descarga en 24-48h
4. Descarga el archivo y déjalo aquí

### ChatGPT
1. Entra en chat.openai.com → avatar arriba derecha → **Settings**
2. **Data controls** → **Export data**
3. Te llega un email con un enlace en 1-24h
4. Descarga, descomprime el `.zip` y trae `conversations.json` aquí

## Después de procesarlos

Cuando tengas archivos aquí, dile a Claude:

```
/ingesta-contexto
```

Él lee todo, destila lo útil en memoria, y te propone mover los archivos a `_archivo_onboarding/` (o borrarlos, tú decides).

## Privacidad

**Estos archivos NO se suben a ningún sitio.** Todo el procesamiento es local en tu máquina. Además, están excluidos del `.gitignore` por defecto — no se suben a GitHub por error.
