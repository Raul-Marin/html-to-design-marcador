---
name: figma-html-to-design-script-only
description: >-
  Solo inyecta capture.js / bootstrap html-to-design en localhost. No ejecutar
  generate_figma_design ni poll salvo que el usuario pida explícitamente
  capturar o enviar la página a Figma. Triggers: solo script, solo capture,
  sin mandar a Figma, sin captura, preparar html-to-design.
---

# html-to-design — solo script

## Por defecto

1. **La página debe abrirse por `http://` o `https://`** (p. ej. `http://localhost:…`). **`file://` no sirve** para html-to-design.
2. **No levantar servidor automáticamente** salvo que el usuario lo pida (“arráncalo”, “quiero verlo”, “captura lista”) o que sea **solo HTML estático** sin `package.json` / script de dev obvio: entonces **sí** puedes proponer o ejecutar **un** servidor simple (p. ej. `python3 -m http.server <puerto>`) **comprobando antes** que el puerto no esté en uso; si ya hay dev server en el repo, **usar ese** y no duplicar.
3. Añadir solo `https://mcp.figma.com/mcp/html-to-design/capture.js` (async) o el IIFE bootstrap si el usuario lo pide.
4. **No** `generate_figma_design`, **no** abrir URLs `#figmacapture`, **no** poll de `captureId`.
5. No duplicar: buscar `capture.js` / `figmacapture` antes.

## Excepción

Si el usuario pide **capturar / enviar / importar a Figma** → entonces sí el flujo MCP completo.

## Snippet mínimo

```html
<script src="https://mcp.figma.com/mcp/html-to-design/capture.js" async></script>
```

Detalle (IIFE, hash real desde MCP, etc.): pedir ampliación o ver historial del repo.
