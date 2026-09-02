# Changelog

Formato: [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/). Versionado semántico.

## [1.1.2] - 2026-09-02

### Corregido

- El `stamp` del encabezado se apilaba en varias líneas cuando el texto crecía o llevaba `<b>`/`<code>`: era un `inline-flex` sin `flex-wrap` y el texto iba suelto, así que cada etiqueta y cada trozo de texto era un ítem flex aparte. Ahora es `flex` con `flex-wrap`, el punto no se encoge y el texto vive en un solo `<span>` (`assets/template.html`, demo).

### Cambiado

- Regla de contenido del `stamp`: una línea corta en texto plano (fecha-hora · hito en ≤ 8 palabras · fuentes, ≤ 120 caracteres). El relato de avance va en «Siguiente bloque» y en `CONTEXTO` (`SKILL.md`, `references/sections.md`).

## [1.1.1] - 2026-09-02

### Eliminado

- `commands/tablero.md`: el skill ya se invoca con `/tablero` en Claude Code y en Cowork, y el comando hacía que el plugin listara el skill dos veces.

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

[1.1.1]: https://github.com/sergiohidalgo-pro/tablero/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/sergiohidalgo-pro/tablero/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/sergiohidalgo-pro/tablero/releases/tag/v1.0.0
