# claycl — Clay para desarrolladores y agentes de IA

Consulta contabilidad, bancos y obligaciones tributarias de Clay directamente desde Claude u otros agentes de IA. También disponible como CLI para scripts y automatizaciones.

## Conectar Clay a Claude (sin instalar nada)

**Paso 1** — En [claude.ai](https://claude.ai), ve a **Settings → Connectors → Add custom connector** e ingresa:

```
https://mcp.clay.cl
```

**Paso 2** — Claude te pedirá tu token de Clay. Lo encuentras en [app.clay.cl → Ajustes generales → Perfil](https://app.clay.cl/ajustes/perfil).

**Paso 3** — Ya está. Prueba escribir en el chat:

> "¿Cuáles son las empresas que tengo en Clay?"
> "Muéstrame los saldos bancarios de la empresa 76345678-9"
> "¿Qué DTE emitió mi empresa en enero?"

Claude consulta Clay en tiempo real y responde con tus datos.

---

## Qué puede hacer Claude con Clay

| Área | Ejemplos |
|------|---------|
| **Empresas** | Listar empresas, ver impuestos, estado de avance contable |
| **Bancos** | Saldos, movimientos, matches, tarjetas de crédito |
| **Obligaciones** | DTE emitidos/recibidos, honorarios, cesiones, invoices |
| **Contabilidad** | Balance, estado de resultados, libro diario, libro mayor |
| **Clientes** | Clientes y proveedores por empresa |
| **Conexiones** | Estado de sincronizaciones bancarias |

---

## Usar con Claude Desktop o Claude Code (con CLI)

Si usas **Claude Desktop** o **Claude Code** en tu editor, instala el CLI:

```bash
curl -sSL https://cli.clay.cl | bash
```

Agrega la configuración a Claude Desktop (`~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "claycl": {
      "command": "claycl",
      "args": ["mcp"],
      "env": {
        "CLAYCL_API_KEY": "tu_token"
      }
    }
  }
}
```

Para **Claude Code**, crea `.claude/mcp.json` en la raíz de tu proyecto con el mismo contenido.

Obtén tu token en [app.clay.cl → Ajustes generales → Perfil](https://app.clay.cl/ajustes/perfil). Luego reinicia Claude Desktop.

---

## CLI — uso directo desde la terminal

El CLI también funciona de forma independiente para scripts, automatizaciones o integración en tus propios sistemas.

### Instalación

```bash
# macOS / Linux
curl -sSL https://cli.clay.cl | bash

# Actualización
curl -sSL https://cli.clay.cl | bash
```

Windows: descarga el `.zip` desde la [última release](https://github.com/madeofclay/claycl-releases/releases/latest).

### Configuración

```bash
export CLAYCL_API_KEY=tu_token
```

O guárdalo en `~/.claycl/config.json`:

```json
{ "api_key": "tu_token" }
```

### Referencia de comandos

#### empresas

```bash
claycl empresas list
claycl empresas impuestos --rut 76345678-9
claycl empresas estado-avance --rut 76345678-9 --desde 2024-01-01 --info contabilidad
claycl empresas crear --nombre "Mi Empresa" --razon-social "Mi Empresa SpA" --rut 76345678 --dv 9 --rut-facturador 12345678 --dv-facturador K
claycl empresas webhook-activar --rut 76345678-9 --url https://mi-servidor.cl/webhook
claycl empresas webhook-delete --rut 76345678-9
```

#### bancos

```bash
claycl bancos saldos --rut 76345678-9
claycl bancos movimientos --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31
claycl bancos movimiento --id abc123
claycl bancos matches --rut 76345678-9
claycl bancos tarjetas --rut 76345678-9 --cuenta 1234 --periodo 2024-01
```

#### obligaciones

```bash
claycl obligaciones dte --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31
claycl obligaciones invoices --rut 76345678-9
claycl obligaciones pendientes --rut 76345678-9
claycl obligaciones honorarios --rut 76345678-9
claycl obligaciones cesiones --rut 76345678-9 --desde 2024-01-01
claycl obligaciones productos --rut 76345678-9
```

#### contabilidad

```bash
claycl contabilidad balance --rut 76345678-9
claycl contabilidad eerr --rut 76345678-9 --year 2024
claycl contabilidad libro-diario --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31
claycl contabilidad libro-mayor --rut 76345678-9 --cuenta 1101 --desde 2024-01-01 --hasta 2024-12-31
claycl contabilidad plan-cuentas --rut 76345678-9
```

#### clientes

```bash
claycl clientes list --rut 76345678-9
claycl clientes list --rut 76345678-9 --tipo cliente
claycl clientes list --rut 76345678-9 --tipo proveedor
```

#### conexiones

```bash
claycl conexiones list
claycl conexiones sync --proveedor banco_estado --rut 76345678 --dv 9 --categoria banco
claycl conexiones delete --id abc123
```

### Opciones globales

```
--format string   Formato de salida: json | table (default: json)
--help            Ayuda para cualquier comando
```

---

## Changelog

Ver [releases](https://github.com/madeofclay/claycl-releases/releases).
