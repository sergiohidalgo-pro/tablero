# Contrato de datos

Todo el tablero se calcula desde el bloque `// ---------- data ----------` del template. No hay otra fuente. Si un número no sale de aquí, está hardcodeado y es un bug.

## PHASES

```js
{id:'F1', name:'Nombre corto', sub:'qué agrupa, en una línea', tasks:[
  {n:'Título de la tarea', s:'done', act:0.25, d:'Evidencia en HTML corto: <code>run 13814</code>, ruta, salida.'},
  {n:'…',                  s:'doing',  est:1.5, d:'…'},
  {n:'…',                  s:'blocked',est:1,   d:'Quién o qué la destraba.'},
  {n:'…',                  s:'todo',   est:2,   d:'…'},
  {n:'…',                  s:'opt',    est:1,   d:'No cuenta para el porcentaje.'},
]}
```

| Campo | Regla |
|---|---|
| `id` | Corto y estable (`F0`…`F5`). Aparece en el Gantt y en el desglose de horas. |
| `s` | `done` cierra la tarea y cuenta para el porcentaje. |
| `act` | Horas reales ya gastadas. Va en cualquier estado: una tarea en curso que consumió 2 h las declara. |
| `est` | Horas restantes estimadas. No las pongas en `done`. |
| `opt` | Fuera del porcentaje y fuera del total; se reporta aparte. |
| `d` | Evidencia, no adjetivos. Acepta `<code>`, `<b>`, enlaces. |

Una fase con alguna tarea `doing` o `blocked` se abre sola y se marca en rojo (`hot`).

## SESSIONS

```js
{label:'lun 24-ago', date:'2026-08-24', blocks:[
  {s:'19:52', e:'22:50', k:'late', t:'qué se hizo en el bloque'},
]}
```

`k`: `on` horario normal · `late` nocturno · `cut` interrumpido. `date` alimenta el rango de calendario; `label` es lo que se ve.

Corta pausas mayores a 20 min: dos bloques, no uno largo. La honestidad del tablero vive acá.

## COSTS

```js
{n:'Recurso', sub:'SKU · supuesto de consumo', lo:120, hi:340}
```

Solo lo **incremental**. `COST_NOTE` dice qué queda fuera y por qué. La `.callout` de la sección nombra la palanca: qué línea domina y cómo se acota.

## DONE_BARS · TODAY · PHASE_ORDER

- `DONE_BARS`: barras verdes del Gantt, trabajo ya hecho. `{name, s:'AAAA-MM-DD', e:'AAAA-MM-DD'}`.
- `TODAY`: la línea roja. Fecha del corte, no `new Date()` — el tablero es una foto fechada.
- `PHASE_ORDER`: orden de proyección de lo pendiente. Por defecto el de `PHASES`; ajústalo si el plan no es secuencial.

## EVAL_RECO · EVAL_EXTRA

`EVAL_RECO` = recomendaciones accionables de ritmo. `EVAL_EXTRA` = observaciones con hora y resultado ("el bloque 19:52–22:50 cerró plan + IaC + 12 repos"), nunca impresiones.

## Métricas derivadas

| Métrica | Fórmula |
|---|---|
| Avance por esfuerzo | `Σ act / (Σ act + Σ est × buffer)` — `act` de todas las tareas, `est` solo de las abiertas |
| Avance por tareas | `done / count`, excluyendo `opt` |
| Horas por hacer | `Σ est × buffer` |
| Cierre estimado | proyección secuencial de `PHASE_ORDER` a `horas/día` sobre días hábiles |
| Días de trabajo | días hábiles entre inicio y cierre según el modo elegido |
| Tareas por hora | `done / Σ horas de SESSIONS` |

El buffer es un control del lector (0 / 30 / 50 %), no un número tuyo: 30 % por defecto para primeros rollouts.

## Chequeo de consistencia

Antes de publicar: **`Σ act` de `PHASES` debe cuadrar con las horas de `SESSIONS`** (± 0,5 h). Si no cuadra, hay trabajo hecho que ninguna tarea declara, o una tarea se atribuyó horas que no ocurrieron. Cuadra el dato, no lo maquilles.

## Persistencia

Los seis controles (`hpd`, `days`, `start`, `blockstart`, `buffer`, `rate`) se guardan en `localStorage` con prefijo `tablero.`. Envolver siempre en `try/catch`: el visor puede tener el almacenamiento bloqueado.
