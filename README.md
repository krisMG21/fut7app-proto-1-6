# Prototipo 1.6 — Registro en vivo

Instrumento de medida **desechable**. No entra en el repositorio de FUT7APP. Se tira cuando
haya dado su resultado.

Lo que se prueba y con qué umbrales está en `docs/ux/02-registro-en-vivo-hipotesis.md`.

## Cómo se publica

Igual que el spike A: repositorio público nuevo, por ejemplo `fut7app-proto-1-6`, ficheros en la
raíz, Ajustes → Pages → rama `main`, carpeta raíz. Necesita HTTPS.

**Repositorio distinto al del spike A.** Si comparten origen, el service worker de uno puede
servir ficheros cacheados del otro.

## Qué mide

| Métrica | Definición |
|---|---|
| **Segundos total** | Desde que aparece la instrucción hasta que la operación queda registrada. Incluye leer y localizar al jugador. Es el número al que se aplica la puerta de salida |
| **Segundos de interacción** | Desde el primer toque hasta el registro. La interacción pura, sin el tiempo de lectura |
| **Tasa de error** | Operaciones cuyo resultado no coincide con el guion: jugador equivocado, acción equivocada, cambio no pedido |
| **Toques** y **desperdiciados** | Toques totales y los que no hicieron avanzar la operación (selección y arrepentimiento) |
| **Deshacer** | Cuántas veces se corrigió una operación ya registrada |

## Decisiones metodológicas

1. **El guion es determinista y compartido.** Semilla fija: las dos disposiciones reciben
   exactamente la misma secuencia sobre la misma plantilla. Sin esto la comparación no vale.
2. **Tras cada operación, el estado se fuerza al esperado.** Si el sujeto se equivoca, el fallo
   queda registrado pero el estado se corrige, porque si no un error en la operación 6
   invalidaría las catorce siguientes.
3. **Proporción real:** 10 sustituciones, 4 goles, 3 cambios de posición, 2 tarjetas, 1 nota.
   Más cambios que goles, que es el dato de uso recogido en la Fase 1.
4. **Las operaciones 14 y 15 son el caso del portero lesionado**: primero la permuta, después la
   sustitución del lesionado. Cuatro toques en la disposición B.
5. **No hay tarjetas rojas.** Dejarían un hueco vacío que ninguna operación posterior del guion
   resuelve, y ese camino no forma parte de lo que se mide.

## Sesgo conocido, que hay que decir en el informe

El tiempo total incluye leer la instrucción en pantalla. En un partido real esa información no se
lee: se ve. Por tanto **el número total es pesimista**, y lo es por igual en las dos
disposiciones, así que la comparación entre A y B es limpia aunque el valor absoluto no lo sea.
Si la ganadora se queda cerca del umbral de 5 s, hay que mirar también la mediana de interacción
antes de dar la puerta por fallada.

## Protocolo de una sesión

Cada sujeto hace **cuatro pasadas**:

| Pasada | Disposición | Modalidad | Orden |
|---|---|---|---|
| 1 | A | Fútbol 7 | primera |
| 2 | B | Fútbol 7 | segunda |
| 3 | B | Fútbol 11 | primera |
| 4 | A | Fútbol 11 | segunda |

Con el **siguiente sujeto se invierte** cuál va primero, para que el aprendizaje no favorezca
siempre a la misma disposición. Con tres sujetos: uno empieza por A, otro por B, el tercero
por A.

Condiciones, no negociables o la prueba no mide lo que dice medir:

- **De pie, en la calle, con sol.** No sentado en casa.
- **Una mano.** El móvil se sujeta con la misma mano que toca.
- Alguien **lee el guion en voz alta a ritmo de partido**, sin esperar a que el sujeto termine.
- **Exportar el JSON después de cada pasada.** Son cuatro ficheros por sujeto.

## Anotaciones a mano

El JSON no captura esto y es la mitad del valor de la prueba:

- ¿Se puede de verdad con una mano, o hace falta la segunda?
- ¿Se lee al sol? ¿Qué elemento es el primero que deja de leerse?
- ¿El sujeto mira al campo o a la pantalla? El objetivo es dirigir, no registrar.
- En la disposición B con fútbol 11: ¿falla la puntería sobre las fichas?
- ¿Qué le sobra y qué le falta en la pantalla?

## Lo que el prototipo no es

No tiene reloj de partido real, ni persistencia, ni deshacer completo, ni estados de error, ni
nada del modelo de datos acordado. Es una superficie táctil con un cronómetro detrás. Cualquier
conclusión sobre otra cosa que no sean tiempos y errores de estas dos disposiciones está fuera
de lo que este instrumento puede sostener.
