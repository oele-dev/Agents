# Chrome DevTools MCP Agent

## Purpose
Agente que usa el **Chrome DevTools MCP server** para auditar performance, inspeccionar red/consola, correr Lighthouse, capturar trazas y depurar páginas reales en Chrome. Complementa a Playwright MCP: **Playwright = automatización E2E, Chrome DevTools = DevTools en vivo**.

## When to Use

| Usa Chrome DevTools MCP cuando... | Ejemplos |
|-----------------------------------|----------|
| Auditar performance de una página | Core Web Vitals (LCP, CLS, INP), traces, insights |
| Correr Lighthouse | Accessibility, SEO, best practices, PWA |
| Depurar requests HTTP reales | Inspeccionar headers, status codes, payloads |
| Detectar errores de consola | Revisar `console.error`, warnings, logs |
| Buscar memory leaks | Heap snapshots en SPA/React/Livewire |
| Emular dispositivo/red/geolocalización | Throttling 3G, dark mode, mobile |
| Inspeccionar una página ya abierta | Conectar a Chrome existente vía `--browser-url` |

**No lo uses para** flujos E2E con asserts, login repetitivo, scraping → usa Playwright MCP (`agent-e2e-test.md`).

## Prerequisites

- Node.js v20.19+ (LTS reciente)
- Chrome estable o superior instalado
- MCP servers **solo se cargan desde `.mcp.json` por proyecto** — no desde `~/.claude/settings.json`

## Setup: `.mcp.json` por proyecto

Copia este snippet al root de cada proyecto donde lo necesites:

### Configuración estándar (headless + isolated)
```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest",
        "--headless=true",
        "--isolated=true"
      ]
    }
  }
}
```

### Combinado con Playwright (ambos en el mismo proyecto)
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest", "--browser", "chromium", "--headless"],
      "env": {
        "PLAYWRIGHT_BROWSERS_PATH": "/Users/oele/.cache/ms-playwright"
      }
    },
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest", "--headless=true", "--isolated=true"]
    }
  }
}
```

### Conectar a un Chrome ya abierto (para debugging manual en vivo)
```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest",
        "--browser-url=http://127.0.0.1:9222"
      ]
    }
  }
}
```
> Lanza Chrome con: `/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222`

## CLI Flags clave

| Flag | Descripción |
|------|-------------|
| `--headless` | Sin UI (recomendado en CI / auditorías batch) |
| `--isolated` | User-data-dir temporal, se limpia al cerrar |
| `--user-data-dir` | Perfil persistente (login guardado) |
| `--channel` | `stable` / `beta` / `dev` / `canary` |
| `--executable-path`, `-e` | Path custom a Chrome |
| `--browser-url`, `-u` | Conectar a Chrome en `http://host:puerto` |
| `--ws-endpoint`, `-w` | WebSocket endpoint de Chrome |
| `--viewport` | Tamaño inicial, ej. `1280x720` |
| `--proxy-server` | Proxy para Chrome |
| `--accept-insecure-certs` | Ignorar errores TLS (dev local) |
| `--log-file` | Path de logs debug |
| `--chrome-arg` | Args adicionales a Chrome (array) |
| `--slim` | Solo 3 tools básicos (reduce contexto) |

## Tools disponibles (29 total)

### Input (9) — interacción DOM
`click`, `drag`, `fill`, `fill_form`, `handle_dialog`, `hover`, `press_key`, `type_text`, `upload_file`

### Navigation (6)
`close_page`, `list_pages`, `navigate_page`, `new_page`, `select_page`, `wait_for`

### Emulation (2)
`emulate` (color scheme, throttling, geo), `resize_page`

### Performance (4) — **la razón principal de usar este MCP**
`performance_start_trace`, `performance_stop_trace`, `performance_analyze_insight`, `take_memory_snapshot`

### Network (2)
`list_network_requests`, `get_network_request`

### Debugging (6)
`evaluate_script`, `list_console_messages`, `get_console_message`, `take_screenshot`, `take_snapshot`, `lighthouse_audit`

## Workflows típicos

### 1. Auditoría de performance de una página
```
1. navigate_page → URL
2. performance_start_trace
3. (interactuar o esperar load)
4. performance_stop_trace
5. performance_analyze_insight → LCP / CLS / INP
```

### 2. Lighthouse audit completo
```
1. navigate_page → URL
2. lighthouse_audit → a11y, SEO, best practices
3. Reportar categorías con score < 90
```

### 3. Debug de errores en producción
```
1. navigate_page → URL problemática
2. list_console_messages → filtrar errors
3. list_network_requests → buscar 4xx/5xx
4. get_network_request {id} → revisar payload/headers
```

### 4. Memory leak en SPA
```
1. take_memory_snapshot (baseline)
2. (ejecutar flujo que se sospecha leak)
3. take_memory_snapshot (después)
4. Comparar retained size
```

## Verificación rápida post-install

Con el `.mcp.json` en el proyecto, abre Claude Code en ese directorio y pide:
```
Usa chrome-devtools para auditar la performance de https://developers.chrome.com
```

El servidor lanza Chrome automáticamente cuando se invoca cualquier tool.

## Instalación alternativa vía Claude CLI (user scope)

Solo si quieres que esté disponible globalmente sin tocar `.mcp.json`:
```bash
claude mcp add chrome-devtools --scope user -- npx -y chrome-devtools-mcp@latest --headless=true --isolated=true
```
> **Nota**: el scope `user` persiste en `~/.claude.json`. El scope `project` escribe `.mcp.json` en el CWD.

## Troubleshooting

- **"Node version < 20.19"** → actualiza Node LTS
- **Chrome no abre** → agrega `--executable-path=/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome`
- **Sesión perdida entre corridas** → quita `--isolated`, usa `--user-data-dir=/tmp/chrome-mcp-profile`
- **Contexto muy largo** → usa `--slim` para reducir el set de tools expuestos

---
*Complementa a `agent-e2e-test.md` (Playwright). Referencia raíz: `CLAUDE.md`.*
