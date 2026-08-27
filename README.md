<p align="center">
  <img src="skills/tablero/assets/logo.svg" width="72" alt="tablero">
</p>

<h1 align="center">tablero</h1>

<p align="center">
  El formato con el que presento el estado de mis proyectos, empaquetado como skill de Claude Code.<br>
  Un artefacto vivo en vez de un informe muerto.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT"></a>
  <img src="https://img.shields.io/badge/Claude%20Code-plugin-5EA2FF" alt="Claude Code plugin">
</p>

---

## Qué hace

Le pides el estado de un proyecto y publica una página con:

- **Cuatro medidores** arriba: avance por esfuerzo y por tareas, horas hechas, horas por hacer con buffer, y el foco o bloqueo del momento.
- **Dónde vamos** en tres columnas: encendido · bloqueado · apagado, y la acción concreta que sigue.
- **Un diagrama** donde cada componente está literalmente encendido, parcial o apagado, con paquetes viajando por las conexiones vivas — y un toggle *Meta* que enciende todo para mostrar hacia dónde va.
- **Backlog** en fases plegables, con horas hechas y estimadas por tarea.
- **Costos** incrementales, con rango bajo/alto y la palanca que domina la factura.
- **Carta Gantt simulable**: el lector mueve horas por día, días hábiles y fecha de inicio, y ve moverse la fecha de cierre.
- **Ritmo de sesiones**: cuándo se trabajó de verdad, y qué recomienda ese dato.

Todo se calcula desde un único bloque de datos. No hay números escritos a mano en el HTML.

## Por qué existe

Un estado de proyecto suele ser una lista de bullets que envejece el mismo día. Este formato apuesta por lo contrario: cada afirmación lleva su evidencia (id de run, ruta, salida de comando), cada hora es de sesión real y no de reloj de pipeline, y el plan es un simulador — si el ritmo cambia, la fecha cambia a la vista de todos.

Las reglas duras de la skill son casi todas sobre honestidad del dato, no sobre diseño.

## Instalación

Como plugin, desde Claude Code:

```
/plugin marketplace add sergiohidalgo-pro/tablero
/plugin install tablero@tablero
```

O solo la skill:

```bash
git clone https://github.com/sergiohidalgo-pro/tablero.git
ln -s "$PWD/tablero/skills/tablero" ~/.claude/skills/tablero
```

## Uso

```
/tablero
```

O simplemente pídelo: *"hazme el tablero de este proyecto"*, *"¿dónde vamos con la migración?"*.

Para actualizarlo, vuelve a pedirlo: republica sobre la misma URL en vez de crear una segunda.

## Personalizar

| Qué | Dónde |
|---|---|
| Logo y créditos del footer | `skills/tablero/assets/logo.svg` y el bloque `.brand` del template |
| Paleta, tipografía, movimiento | CSS de `skills/tablero/assets/template.html` |
| Qué secciones aparecen | `skills/tablero/references/sections.md` |
| Forma de los datos y fórmulas | `skills/tablero/references/data-contract.md` |

El logo va inline en el footer del template, sin `<style>` interno (las clases de un SVG embebido se filtran al CSS de la página). Si lo reemplazas: viewBox cuadrado, colores como atributos `fill`, y un fondo propio para que funcione en tema claro y oscuro.

Si haces un fork con tu propia identidad: cambia el logo y la línea de crédito, y deja el enlace a este repo. Es lo único que pido.

## Estructura

```
skills/tablero/
├── SKILL.md                    # contrato de activación y reglas duras
├── assets/
│   ├── template.html           # el formato: CSS, estructura y motor de render
│   └── logo.svg                # slot de marca
└── references/
    ├── data-contract.md        # forma de los datos y métricas derivadas
    ├── sections.md             # catálogo de secciones y reglas del diagrama
    └── design-system.md        # tokens, componentes, movimiento, accesibilidad
```

## Créditos

Formato y skill: **[Sergio Hidalgo](https://github.com/sergiohidalgo-pro)**.
Construido con [Claude Code](https://claude.com/claude-code).

MIT — úsalo, fórkealo, hazlo tuyo.
