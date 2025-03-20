# Guía para crear un servidor MCP (Model Context Protocol)

Este tutorial te guiará a través del proceso de configuración y ejecución de un servidor MCP usando UV y el SDK de Python para MCP, tanto en Windows como en macOS.

## Índice

1. [Introducción](#introducción)
2. [Requisitos previos](#requisitos-previos)
3. [Instalación en Windows](#instalación-en-windows)
4. [Instalación en macOS](#instalación-en-macos)
5. [Configuración del entorno virtual](#configuración-del-entorno-virtual)
6. [Instalación de dependencias](#instalación-de-dependencias)
7. [Creación de un servidor MCP básico](#creación-de-un-servidor-mcp-básico)
8. [Ejecución del servidor MCP](#ejecución-del-servidor-mcp)
9. [Recursos adicionales](#recursos-adicionales)

## Introducción

El Model Context Protocol (MCP) es un protocolo que permite a los modelos de lenguaje interactuar con contexto externo, como archivos, herramientas y APIs. En esta guía, aprenderás a configurar un servidor MCP básico utilizando el SDK de Python.

## Requisitos previos

- Python 3.8 o superior
- Node.js (la última versión)
- Terminal o línea de comandos

## Instalación en Windows

1. **Instalar UV (gestor de paquetes de Python ultrarrápido)**

   Abre PowerShell como administrador y ejecuta:

   ```powershell
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

2. **Instalar Node.js**

   Descarga e instala Node.js desde [la página oficial de Node.js](https://nodejs.org/en/download/current)
   
   O usa un gestor de paquetes como Chocolatey:
   
   ```powershell
   choco install nodejs
   ```

## Instalación en macOS

1. **Instalar UV**

   Abre Terminal y ejecuta:

   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Instalar Node.js**

   Utilizando Homebrew:
   
   ```bash
   brew install node
   ```
   
   O descarga el instalador desde [la página oficial de Node.js](https://nodejs.org/en/download/current)

## Configuración del entorno virtual

### Windows

```powershell
# Crear un entorno virtual
py -m venv .venv

# Activar el entorno virtual
./.venv/scripts/activate
```

### macOS

```bash
# Crear un entorno virtual
python3 -m venv .venv

# Activar el entorno virtual
source .venv/bin/activate
```

## Instalación de dependencias

Con el entorno virtual activado, instala las dependencias necesarias:

```bash
# Instalar UV dentro del entorno virtual (opcional pero recomendado)
pip install uv

# Instalar el SDK de MCP
pip install mcp

# Instalar la interfaz de línea de comandos de MCP
pip install mcp[cli]
```

## Creación de un servidor MCP básico

Crea un archivo `main.py` con el siguiente contenido:

```python
from mcp.server.fastmcp import FastMCP

# Create server
mcp = FastMCP("Echo Server")


@mcp.tool()
def echo_tool(text: str) -> str:
    """Echo the input text"""
    return text


@mcp.resource("echo://static")
def echo_resource() -> str:
    return "Echo!"


@mcp.resource("echo://{text}")
def echo_template(text: str) -> str:
    """Echo the input text"""
    return f"Echo: {text}"


@mcp.prompt("echo")
def echo_prompt(text: str) -> str:
    return text
```

## Ejecución del servidor MCP

Una vez que hayas creado tu servidor MCP, puedes ejecutarlo usando el comando `mcp dev`:

```bash
# Ejecutar el servidor MCP en modo desarrollo
mcp dev main.py
```

Este comando iniciará tu servidor MCP en modo de desarrollo, permitiéndote interactuar con él a través de la interfaz de línea de comandos o conectar un cliente MCP.

## Recursos adicionales

- **Documentación oficial de MCP**: [Model Context Protocol](https://github.com/modelcontextprotocol/python-sdk)
- **Documentación de UV**: [UV Documentation](https://docs.astral.sh/uv/)
- **Node.js**: [Node.js Documentation](https://nodejs.org/en/docs/)
