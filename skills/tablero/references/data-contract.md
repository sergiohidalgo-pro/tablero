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

## EMBLEMA · CONTEXTO

```js
const EMBLEMA = {emoji:'🧩', titulo:'La pieza que sale', porque:'<p>…</p>'};
const CONTEXTO = {
  estado:'amarillo',              // rojo | amarillo | verde
  titulo:'Avanza con reservas',
  resumen:'Una frase con el porqué del color',
  falta:['Qué falta y de quién depende'],
  hay:['Qué ya está resuelto y no hay que volver a discutir'],
};
```

`emoji` es el mismo del favicon del artefacto, para que la pestaña y la página digan lo mismo. `porque` acepta HTML y responde una sola pregunta: por qué ese emoji representa a este proyecto.

`estado` no es el avance: es si se puede seguir sin depender de nadie. Verde solo si lo único que falta son horas. Cada entrada de `falta` nombra al dueño o al insumo; si no lo tiene, no es accionable y no va.

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

## Actualizar un tablero que ya existe

La URL es única por proyecto, así que actualizar es republicar encima. Antes de hacerlo:

1. **Lee la versión viva**, no la copia que bajaste al empezar la sesión. Entre medio pudo publicar otra sesión, otro agente o la propia página.
2. **Contrasta su bloque de datos con este contrato.** Un tablero hecho con una versión anterior del skill puede no tener bloques enteros (`EMBLEMA` y `CONTEXTO` llegaron en 1.1.0). Lo que falte se completa desde evidencia; el hueco no se hereda.
3. **Compara contra tu evidencia.** Lo que la versión viva afirme y tú no hayas re-verificado, se conserva tal cual.
4. **Si hay contradicción, vuelve a la fuente primaria** antes de sobrescribir. Que un tablero se haya publicado después no lo hace correcto: heredar en silencio una cifra equivocada es peor que dejar una vieja con su fecha.
5. Publica sólo lo que agregas y lo que corriges, y deja constancia en el `stamp` y en las fuentes de qué se re-verificó y a qué hora.

## Persistencia

Los seis controles (`hpd`, `days`, `start`, `blockstart`, `buffer`, `rate`) se guardan en `localStorage` con prefijo `tablero.`. Envolver siempre en `try/catch`: el visor puede tener el almacenamiento bloqueado.
