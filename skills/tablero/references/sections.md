# Catálogo de secciones

El orden no es decorativo: responde en cascada a "¿cómo vamos?" → "¿dónde estamos parados?" → "¿qué falta?" → "¿cuánto cuesta?" → "¿cuándo termina?" → "¿a qué ritmo?". Sacar una sección está bien; reordenarlas rompe la lectura.

## 1. Header

Eyebrow (contexto: sub-proyecto, cliente, área) · H1 (nombre corto, no una frase) · subtítulo con qué es, dónde corre y **qué NO toca** · `stamp` con fecha-hora del corte, hito vivo y fuentes.

El `stamp` es la firma del dato: sin fecha y fuentes, el tablero no se publica.

## 2. Banda de medidores (4, orden fijo)

1. Anillo doble: avance por esfuerzo (arco grueso) y por tareas (arco fino).
2. Horas hechas.
3. Horas por hacer, con buffer.
4. Foco o riesgo. Si hay un bloqueo real: `class="card meter now rv"` y `<span class="led bad">` — la tarjeta escanea en rojo. Úsalo solo cuando algo está de verdad detenido; si siempre está encendido, deja de significar algo.

## 3. Dónde vamos — tres columnas

`col on` encendido · `col bad` bloqueado · `col off` apagado. Bullets con **negrita al frente** (el hito) y evidencia detrás. Cierra con `.next`: la acción concreta (archivo, comando o URL exacta) y qué necesita aprobación humana.

Si nada está bloqueado, elimina la columna roja y deja dos: no inventes un bloqueo para llenar la grilla.

## 4. Arquitectura (o su reemplazo)

Diagrama SVG con estado por componente y toggle **Hoy / Meta**. Reglas:

- Un `<g data-s="done|doing|todo">` por componente, con `<rect class="box">` + `<text class="h">` + hasta tres `<text class="t">`.
- El LED lo inyecta el JS en la esquina del rect. No lo dibujes.
- Aristas **antes** de los nodos: `<path id="eN" class="edge" data-s="…" d="…"/>`. `done` y `doing` reciben paquetes animados; `todo` queda punteada.
- Zonas lógicas con `<rect class="zone">`; lo que no cambia con `<rect class="ext">` (sin estado, sin LED).
- `viewBox` ~1100×560 y `min-width:760px`: en móvil scrollea en su contenedor, la página nunca.
- **Aire**: cajas de 64 px de alto con 56 px entre ellas, y al menos 56 px entre la última caja y el borde de la zona. Apretadas se leen como una lista, no como un sistema.
- **`data-title` + `data-info` en cada `<g>`** hacen la caja consultable: al tocarla (clic, Enter o Espacio) se abre el detalle bajo el diagrama. `data-info` responde qué es y **por qué está en ese estado**, no repite el subtítulo; una o dos frases, con cifras y de qué depende. Sin `data-info`, la caja no es interactiva.
- La vista **Meta** enciende todo en el color de acento con un barrido de arriba hacia abajo. Es el argumento visual de "hacia dónde vamos"; no la saques. Si agregas reglas de estado, recuerda que `.diagram .edge.live[data-s="doing"]` pesa más que `.diagram.meta .edge`: la regla de meta tiene que repetir el selector completo o el ámbar sobrevive al cambio de vista.

Debajo, tabla `comp` de tres columnas: Componente · Estado (chip) · **Evidencia verificable** (id de run, fecha, salida). Sin adjetivos.

Cuando no hay topología (investigación, auditoría, migración de datos), reemplaza el diagrama por una tabla de cobertura con los mismos chips.

## 5. Backlog

Fases plegables; tareas plegables dentro. La fase con trabajo vivo se abre sola. Las horas van a la derecha: `hechas · por hacer`. Nota fija arriba: horas de sesión, no de pipeline; opcionales fuera del porcentaje.

## 6. Costos

Izquierda dinero (rango bajo/alto con barrita de peso relativo, total, `COST_NOTE`, `.callout` con la palanca). Derecha horas por fase, buffer y tarifa opcional — con tarifa en 0 no se muestra dinero de horas.

Sin dinero involucrado: deja solo la columna de horas.

## 7. Gantt

Verde = hecho (`DONE_BARS`). Azul = proyección desde el backlog; el tramo claro es el buffer. Línea roja punteada = `TODAY`. Cuatro controles: horas/día, días hábiles, inicio, hora del bloque. Es un simulador: el lector cambia el ritmo y ve la fecha de cierre moverse.

## 8. Ritmo de sesiones — o Riesgos vivos

**Ritmo** cuando el trabajo son bloques de sesión: franjas de 24 h por día, ticks a las 06/12/18, fila "Propuesta" con el bloque sugerido, y dos columnas (qué muestran los datos / recomendación).

**Riesgos vivos** cuando el proyecto es de calendario y varias personas: tabla riesgo · impacto · disparador · mitigación, con los mismos chips de estado.

## 9. Footer

Fuentes con rutas, ids de runs y tickets — todo perseguible. Después, el bloque `.brand`: logo, autoría y enlace a la skill. No lo quites al republicar.
