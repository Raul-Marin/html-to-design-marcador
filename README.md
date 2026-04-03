# html.to.design — marcador (bookmarklet)

Repo para **explicar y compartir** un flujo práctico: llevar una web a **Figma en capas** usando el mismo mecanismo que **html-to-design** (ventana de importación / captura), **sin pasar por la IA ni gastar tokens del MCP**.

## Idea en una frase

Reutilizas la lógica de la importación que antes mucha gente asociaba a flujos tipo **Cloud Code → Figma**: cargar `capture.js`, fijar el hash de captura y dejar que el servicio convierta la página en capas. Eso se puede hacer **desde el navegador** con un **bookmarklet** (un marcador que ejecuta JavaScript en la pestaña actual): **gratis** para pruebas rápidas o para traer **referencias** (tuyas o ajenas), sabiendo que el resultado no es perfecto (CSP, iframes, sitios muy dinámicos, etc.).

## Qué hay en el repo

| Elemento | Para qué sirve |
|----------|----------------|
| **`marcador-codigoJS`** | Código **minificado en una línea** listo para pegar como URL de un marcador (`javascript:…`). Carga `capture.js` de Figma, espera 500 ms, pone el hash de prueba `figmacapture&figmadelay=1000` y vuelve a cargar el script (patrón recomendado para localhost / pruebas). |
| **`.cursor/skills/figma-html-to-design-script-only/SKILL.md`** | Skill: por defecto solo preparar la página (script), sin disparar `generate_figma_design` ni poll — **ahorra tokens** con el MCP cuando no quieres captura automática. |
| **`.cursor/rules/figma-html-to-design-capture.mdc`** | Regla de proyecto + **tabla** de dónde replicar la misma idea en **Claude Code**, **Codex CLI** y **Copilot CLI** (con enlaces a su documentación). |

**La carpeta `.cursor/` va incluida a propósito** en el repositorio para que quien clone el proyecto pueda usarla tal cual o copiarla a otros repos locales.

Aunque la skill y la regla se **redactaron y enlazan desde Cursor** (rutas `.cursor/`), el **criterio es compatible** con otros asistentes por terminal o IDE: **Claude Code**, **Codex**, **Copilot CLI**, etc. Cada herramienta usa su propio fichero o carpeta de instrucciones; la regla del repo resume **dónde pegar** el equivalente. No hace falta usar Cursor para aprovechar el mismo comportamiento.

## Código del bookmarklet

**Una línea** para pegar en la URL del marcador (mismo contenido que `marcador-codigoJS`): inyecta `capture.js`, a los 500 ms asigna el hash `figmacapture&figmadelay=1000` y vuelve a cargar el script.

```javascript
javascript:(function(){var s=document.createElement('script');s.src='https://mcp.figma.com/mcp/html-to-design/capture.js';document.head.appendChild(s);setTimeout(function(){window.location.hash='figmacapture&figmadelay=1000';var s2=document.createElement('script');s2.src='https://mcp.figma.com/mcp/html-to-design/capture.js';document.head.appendChild(s2);},500);})();
```

## Cómo crear el marcador en el navegador

1. Copia **toda la línea** del bloque anterior.
2. Crea un marcador nuevo y en **URL** pega ese contenido (en Chrome: “Añadir marcador” → pegar en URL).
3. Entra en la página que quieras capturar (debe cargarse con **`http://` o `https://`**; `file://` no vale).
4. Pulsa el marcador. Sigue las indicaciones de la UI de captura de Figma (iniciar sesión si hace falta, elegir archivo destino, etc.).

Si Figma / el flujo MCP te da un hash concreto (`#figmacapture=<id>&figmaendpoint=…`), en esa misma línea cambia el literal `'figmacapture&figmadelay=1000'` por **exactamente** el fragmento que te indiquen (sin el `#` inicial). No inventes `captureId` ni endpoints.

## Tokens: bookmarklet vs MCP

- **Con el MCP** (`generate_figma_design`, etc.) el asistente recibe **instrucciones largas** y suele hacer **varias llamadas** (poll): eso **consume tokens** en el chat.
- **Con el bookmarklet** tú disparas la captura **solo en el navegador**; **no** necesitas que la IA ejecute ese flujo para “meter la web en Figma”. Ideal para **referencias** y **iteraciones rápidas** sin pagar ese coste en el modelo.

La skill y la regla ayudan cuando **sí** usas un asistente con el MCP de Figma: que el agente **no** lance captura MCP por defecto y solo **inyecte** script cuando eso es lo que quieres (misma idea en Cursor, Claude Code, Codex, Copilot, etc., según cómo importes las instrucciones).

## Aviso

Este flujo depende del **servicio html-to-design de Figma** y de tu **cuenta / permisos**. No es un producto oficial de este repo; aquí solo se documenta el truco y se comparten archivos de ejemplo.
