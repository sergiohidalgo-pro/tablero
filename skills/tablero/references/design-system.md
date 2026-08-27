# Sistema visual

Estética de **panel de control encendido**: fondo oscuro con grilla técnica, LEDs, paquetes que viajan por las aristas. La metáfora es literal — un componente que funciona está encendido, uno que falta está apagado y punteado. Todo el CSS vive en `assets/template.html`; esto explica por qué es así.

## Tokens

Definidos como custom properties en `:root` (paleta oscura), y redefinidos dos veces: bajo `@media (prefers-color-scheme: light)` con guarda `:root:not([data-theme="dark"])`, y bajo `:root[data-theme="light"]`. Los tres bloques deben mantenerse en sync: un color definido en un solo lugar rompe uno de los dos temas.

| Grupo | Uso |
|---|---|
| `--bg` `--bg-2` `--surface` `--surface-2` | Fondo, tarjetas, rellenos |
| `--line` `--line-strong` | Bordes y separadores |
| `--ink` `--muted` `--faint` | Tres niveles de texto, nunca más |
| `--accent` (+ `-ink` `-soft` `-glow`) | Plan, proyección, vista Meta |
| `--on` | Hecho, encendido, trabajo real |
| `--warn` | En curso, parcial, nocturno |
| `--bad` | Bloqueado, hoy, corte |
| `--off` (+ `-soft` `-ink`) | Apagado, pendiente |

El semáforo es semántico: `--on` nunca decora, siempre significa "esto funciona hoy y hay evidencia".

## Tipografía

Bricolage Grotesque (display, 500/600/800) · IBM Plex Sans (cuerpo) · IBM Plex Mono (números, ids, rutas). Todas por Google Fonts, el único host externo permitido, y con fallback real en la pila.

Todo número comparable lleva `font-variant-numeric: tabular-nums`. Los `h2` cierran con una línea que se desvanece (`h2::after`).

## Componentes

- `.card` — superficie base. `.meter` para medidor, `.pad` para contenido, `.col` para columna de estado.
- `.chip` — LED con etiqueta: `done` `doing` `blocked` `todo` `opt`. `doing` pulsa, `blocked` parpadea, `todo` es un anillo hueco.
- `.bar` / `.spark` — progreso; ancho por `data-w` aplicado en el frame siguiente para que anime.
- `.ring` — anillo doble SVG: arco grueso esfuerzo, arco fino tareas.
- `details.phase` / `details.task` — plegables con barra lateral de estado (`lit` / `hot` / `dim`).
- `.callout` — una por sección como máximo: la advertencia que el lector no puede perderse.

## Movimiento

- `.rv` → `.in`: revelado al hacer scroll, con IntersectionObserver, fallback por scroll y un `setTimeout` de 4 s que enciende todo pase lo que pase. Nunca dejes contenido invisible si el observer falla.
- Números que cuentan hacia arriba con easing cúbico (~1,1 s).
- Barras del Gantt y del ritmo: `scaleX` desde el borde izquierdo, escalonadas.
- Paquetes en las aristas con `animateMotion` sobre el `path` real.
- `prefers-reduced-motion: reduce` apaga toda animación y transición, y muestra el estado final. Es obligatorio, no opcional.

## Accesibilidad

Foco visible en cada control (`:focus-visible` con outline de acento). El toggle usa `aria-pressed`. El diagrama lleva `role="img"` y un `aria-label` que describe lo que muestra. Contraste verificado en ambos temas: `--muted` sobre `--surface` es el par más ajustado, no lo bajes.

## Responsive

Grillas que colapsan a una columna bajo 900 px. Diagrama, Gantt y tablas anchas scrollean dentro de su contenedor (`overflow-x:auto`); el `body` nunca scrollea en horizontal. Bajo 700 px el backlog esconde la barra de progreso de fase y mantiene las horas.
