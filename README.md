# claycl

CLI para la [API de Clay](https://api.clay.cl) — contabilidad, bancos, obligaciones tributarias y más, desde la línea de comandos.

## Instalación

### macOS / Linux

```bash
curl -sSL https://github.com/madeofclay/claycl-releases/releases/latest/download/claycl_$(uname -s | tr '[:upper:]' '[:lower:]')_$(uname -m | sed 's/x86_64/amd64/;s/aarch64/arm64/').tar.gz | tar xz
sudo mv claycl /usr/local/bin/
```

### Homebrew (macOS)

Próximamente.

### Windows

Descarga el `.zip` desde la [última release](https://github.com/madeofclay/claycl-releases/releases/latest) y agrega el ejecutable a tu PATH.

## Configuración

```bash
export CLAYCL_API_KEY=tu_api_key
```

O crea el archivo `~/.claycl/config.json`:

```json
{ "api_key": "tu_api_key" }
```

## Uso

```bash
claycl empresas list
claycl empresas impuestos --rut 76345678-9

claycl bancos saldos --rut 76345678-9
claycl bancos movimientos --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31

claycl obligaciones dte --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31
claycl obligaciones pendientes --rut 76345678-9
claycl obligaciones honorarios --rut 76345678-9

claycl contabilidad balance --rut 76345678-9
claycl contabilidad eerr --rut 76345678-9 --year 2024
claycl contabilidad libro-diario --rut 76345678-9 --desde 2024-01-01 --hasta 2024-12-31

claycl clientes list --rut 76345678-9 --tipo cliente

claycl conexiones list
claycl conexiones sync --id abc123
```

## Changelog

Ver [releases](https://github.com/madeofclay/claycl-releases/releases) para el historial de versiones.
