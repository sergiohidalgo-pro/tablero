# Changelog

Formato: [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/). Versionado semántico.

## [1.1.0] - 2026-09-01 — beta pública

### Añadido

- Emblema del proyecto con su porqué y semáforo de contexto en el encabezado (`EMBLEMA`, `CONTEXTO`).
- Detalle por componente en el diagrama: clic o Enter sobre una caja explica qué es y por qué está en ese estado.
- Demo en vivo en GitHub Pages y capturas en el README.
- Instrucciones para Claude Cowork, y regla para hosts sin la herramienta de artefactos: el archivo local es la salida.
- Snapshot semanal del tráfico del repo en la rama `traffic`.

### Cambiado

- Salida local por defecto; se publica solo cuando se pide en la sesión.
- Al actualizar un tablero existente: leer la versión viva y mergear encima, y contrastar el bloque de datos contra el contrato completo.
- Verificación visual del archivo antes de entregarlo.

### Corregido

- Diagrama: marcadores ámbar que sobrevivían en la vista Meta, escalonado no reversible al volver a Hoy, paquetes encimados en aristas cortas, SMIL desfasado al reentrar al viewport.
- Rendimiento: animaciones en pausa fuera del viewport, grilla en capa fija, re-render sin re-animación, sin `drop-shadow` en elementos en movimiento.
- GitHub Pages: índice en la raíz y título único en la demo.

## [1.0.0] - 2026-08-27

- Primera publicación: el formato de estado de proyecto como skill y plugin de Claude Code.

[1.1.0]: https://github.com/sergiohidalgo-pro/tablero/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/sergiohidalgo-pro/tablero/releases/tag/v1.0.0
