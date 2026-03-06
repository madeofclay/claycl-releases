# claycl

CLI para la [API de Clay](https://api.clay.cl) — contabilidad, bancos, obligaciones tributarias y más, desde la línea de comandos o desde agentes de IA.

## Instalación

### macOS / Linux

```bash
curl -sSL https://cli.clay.cl | bash
```

### Windows

Descarga el `.zip` desde la [última release](https://github.com/madeofclay/claycl-releases/releases/latest), descomprime y agrega el ejecutable a tu PATH.

## Actualización

Vuelve a correr el mismo comando de instalación — sobreescribe el binario con la última versión:

```bash
curl -sSL https://cli.clay.cl | bash
```

---

## Configuración

**Opción 1 — Variable de entorno:**
```bash
export CLAYCL_API_KEY=tu_api_key
```

**Opción 2 — Archivo de configuración:**
```bash
mkdir -p ~/.claycl
echo '{"api_key": "tu_api_key"}' > ~/.claycl/config.json
```

---

## Referencia de comandos

### empresas

```bash
# Listar todas las empresas
claycl empresas list

# Información tributaria
claycl empresas impuestos --rut 76345678-9

# Estado de avance
claycl empresas estado-avance --rut 76345678-9 --desde 2024-01-01 --info contabilidad
claycl empresas estado-avance --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31 --info contabilidad

# Crear empresa
claycl empresas crear \
  --nombre "Mi Empresa" \
  --razon-social "Mi Empresa SpA" \
  --rut 76345678 \
  --dv 9 \
  --rut-facturador 12345678 \
  --dv-facturador K

# Webhooks
claycl empresas webhook-activar --rut 76345678-9 --url https://mi-servidor.cl/webhook
claycl empresas webhook-activar --rut 76345678-9 --url https://mi-servidor.cl/webhook --token mi_token
claycl empresas webhook-delete --rut 76345678-9
```

### bancos

```bash
# Saldos
claycl bancos saldos --rut 76345678-9

# Movimientos (lista)
claycl bancos movimientos --rut 76345678-9
claycl bancos movimientos --rut 76345678-9 --cuenta 1234567890
claycl bancos movimientos --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31

# Movimiento por ID
claycl bancos movimiento --id abc123

# Matches
claycl bancos matches --rut 76345678-9
claycl bancos matches --rut 76345678-9 --cuenta 1234567890

# Tarjetas de crédito
claycl bancos tarjetas --rut 76345678-9
claycl bancos tarjetas --rut 76345678-9 --cuenta 1234 --periodo 2024-01
```

### obligaciones

```bash
# Documentos tributarios (DTE)
claycl obligaciones dte --rut 76345678-9 --desde 2024-01-01
claycl obligaciones dte --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31

# Invoices
claycl obligaciones invoices --rut 76345678-9

# Documentos pendientes (RCV SII, actualiza cada 4h)
claycl obligaciones pendientes --rut 76345678-9

# Boletas de honorarios
claycl obligaciones honorarios --rut 76345678-9

# Cesiones DTE
claycl obligaciones cesiones --rut 76345678-9 --desde 2024-01-01
claycl obligaciones cesiones --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31

# Productos de DTE y honorarios
claycl obligaciones productos --rut 76345678-9

# Crear obligación (DTE)
claycl obligaciones crear --json '{
  "organization_rut": "76345678-9",
  "oficina_partes": false,
  "dte": {
    "fecha_emision": "2024-01-15",
    "codigo_sii": "33",
    "folio": "1234",
    "emisor": { "rut_emisor": "76345678", "razon_social": "Mi Empresa SpA", "giro": "Servicios", "direccion": "Av. Principal 123", "comuna": "Santiago", "ciudad": "Santiago" },
    "receptor": { "rut_receptor": "12345678", "razon_social": "Cliente SpA", "giro": "Comercio", "direccion": "Calle 456", "comuna": "Providencia", "ciudad": "Santiago" },
    "totales": { "monto_afecto": 100000, "monto_exento": 0, "tasa_iva": "19", "iva": 19000, "otros_impuestos": 0, "monto_total": 119000 },
    "detalle": [{ "numero_linea": 1, "nombre_item": "Servicio", "descripcion": "Descripción", "cantidad": 1, "precio_unitario": 100000, "precio_total": 100000 }]
  }
}'

# Crear notas de venta
claycl obligaciones nota-venta --json '{"rut_empresa":"76345678","dv_empresa":"9","notas":[...]}'

# Crear órdenes de compra
claycl obligaciones orden-compra --json '{"rut_empresa":"76345678","dv_empresa":"9","ordenes_compra":[...]}'
```

### contabilidad

```bash
# Balance de 8 columnas
claycl contabilidad balance --rut 76345678-9

# Estado de resultados
claycl contabilidad eerr --rut 76345678-9 --year 2024

# Libro diario
claycl contabilidad libro-diario --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31

# Libro mayor
claycl contabilidad libro-mayor --rut 76345678-9 --cuenta 1101 --desde 2024-01-01 --hasta 2024-12-31

# Plan de cuentas
claycl contabilidad plan-cuentas --rut 76345678-9
```

### clientes

```bash
# Listar clientes y proveedores
claycl clientes list --rut 76345678-9
claycl clientes list --rut 76345678-9 --tipo cliente
claycl clientes list --rut 76345678-9 --tipo proveedor
```

### conexiones

```bash
# Listar conexiones
claycl conexiones list

# Crear conexión
claycl conexiones crear \
  --proveedor banco_estado \
  --rut 76345678 --dv 9 \
  --rut-usuario 12345678 --dv-usuario K \
  --clave mipassword \
  --cuenta 1234567890 \
  --banco banco_estado

# Editar conexión
claycl conexiones editar \
  --id abc123 \
  --proveedor banco_estado \
  --rut 76345678 --dv 9 \
  --rut-usuario 12345678 --dv-usuario K \
  --clave nuevapassword \
  --fecha-inicio 2024-01-01 \
  --cuenta 1234567890 \
  --banco banco_estado

# Sincronizar (actualizar datos)
claycl conexiones sync --proveedor banco_estado --rut 76345678 --dv 9 --categoria banco

# Validar conexión (onboarding)
claycl conexiones onboarding \
  --proveedor banco_estado \
  --rut 76345678 --dv 9 \
  --categoria banco

# Eliminar conexión
claycl conexiones delete --id abc123
```

---

## Opciones globales

```
--format string   Formato de salida: json | table (default: json)
--help            Ayuda para cualquier comando
```

---

## Changelog

Ver [releases](https://github.com/madeofclay/claycl-releases/releases) para el historial de versiones.

---

## MCP (Model Context Protocol)

claycl puede usarse como servidor MCP para conectarlo a Claude Desktop o Claude Code.

```json
{
  "mcpServers": {
    "claycl": {
      "command": "claycl",
      "args": ["mcp"],
      "env": {"CLAYCL_API_KEY": "tu_key"}
    }
  }
}
```

Guarda esa configuración en `~/.claude/claude_desktop_config.json` (Claude Desktop) o `.claude/mcp.json` en la raíz de tu proyecto (Claude Code).

Tools disponibles: `clay_empresas`, `clay_obligaciones`, `clay_bancos`, `clay_contabilidad`, `clay_clientes`, `clay_conexiones`.

---

## Telemetría

Por defecto, cada invocación queda registrada localmente en `~/.claycl/usage.log` (JSON lines). No se envía nada a ningún servidor.

```bash
# Ver actividad en tiempo real
tail -f ~/.claycl/usage.log | jq .

# Resumen por comando
cat ~/.claycl/usage.log | jq -s 'group_by(.tool) | map({tool: .[0].tool, count: length}) | sort_by(-.count)'
```

**Opt-in CloudWatch** (para equipos con CloudWatch Agent):

```bash
export CLAYCL_TELEMETRY=1
```

Con esta variable activa, además del log local se escribe el evento en formato EMF a stderr para que CloudWatch Agent lo envíe al log group `/claycl/usage`.
