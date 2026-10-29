# HK Planner Hotel Monserrat — contexto del proyecto

Archivo de contexto para quien retome esta app (una persona o un asistente de IA). Leelo antes de tocar `index.html`.

## Qué es

App web de estudio de Housekeeping para alumnos de primer año de la materia **Departamento de Front Office y Housekeeping (2.6.106)**, turno noche, 2.º cuatrimestre 2026. La docente es Helena Havrylets.

Los alumnos la usan para estudiar las rutinas hoteleras según la bibliografía y como **modelo de referencia** de la app que ellos mismos construyen en clase con vibecoding (Antigravity, plan gratuito), una pestaña por clase del 22/10 al 12/11.

- Público: https://hk-planner.vercel.app
- Repositorio: https://github.com/hhavry/hk-planner (rama `main`)
- Copia en Claude (artifact privado de la docente): https://claude.ai/artifact/KW4jZGd44w7g9PBtbUBFhb

El «Hotel Monserrat» es **ficticio**: 24 habitaciones en 3 pisos (101–108, 201–208, 301–308) más áreas públicas.

## Cómo está hecha

- Un único archivo `index.html` con HTML, CSS y JavaScript adentro. Sin build, sin dependencias, sin backend.
- Única carga externa: Google Fonts (Bricolage Grotesque para títulos, Figtree para texto), con fuentes de respaldo.
- El avance de cada alumno se guarda en `localStorage`, clave `hkplanner.v1`, solo en su dispositivo. No hay nombres, cuentas ni seguimiento: fue una decisión de la docente.
- Todo el DOM se arma con la función `h(tag, attrs, ...hijos)`. El texto ingresado por el usuario se inserta siempre con `textContent`.
- Navegación por hash: `#rutina` (pantalla inicial), `#fondo`, `#frecuencias`, `#control`, `#hoy`.
- Debe verse bien en celular (400 px) y no tener scroll horizontal de página. Solo tablas y el calendario se deslizan dentro de su propio contenedor.

### Publicar un cambio

1. Editar `index.html`.
2. Probarlo abriéndolo en el navegador (y a 400 px de ancho).
3. Commit y push a `main`. Vercel publica solo en el mismo link en un minuto.

El artifact de Claude es el mismo contenido sin `<!DOCTYPE>`, `<html>`, `<head>` ni `<body>` (Claude los agrega al publicar). Si se actualiza uno, actualizar el otro.

## Estructura de la app

Cinco pestañas. **Hoy** va última, después de Control, en amarillo (`--hoy`) y en verde oscuro cuando está activa (`--hoy-on`): pedido expreso de la docente. Las otras cuatro tienen modo **Estudiar** y modo **Practicar** (preguntas de repaso con explicación; mejor marca guardada).

| Pestaña | Contenido | Funciones en el código |
| --- | --- | --- |
| Hoy | Qué toca hacer día por día, con flechas para cambiar de fecha y casillas para tildar. Selector de puesto: Mucama, Áreas Públicas, Gobernanta y **Mi casa**. Las tareas enlazan a los procedimientos de las otras pestañas; la Gobernanta ve además los bloqueos activos y los partes urgentes. En Mi casa el alumno carga sus propias tareas con frecuencia (diaria, día por medio, semanal, quincenal, mensual). | `vHoy`, `HOY`, `dueOn`, `goTo`, `calIdx`; estado en `S.hoy`, `S.casa`, `S.puesto` |
| Rutina | Habitación de salida (15 pasos), **habitación ocupada (10 pasos con subpasos, FEHGRA)**, baño (8), armado de cama, **apertura o turndown (11 pasos, FEHGRA)**, habitación no rentada (8), cada paso con su «Por qué» cuando la fuente lo da; «Los No» propios de la ocupada; paños y productos; Los SÍ / Los No; códigos de estado; cartel de No molestar con la práctica actual; tabla de equivalencias de los nombres de los puestos; prioridades según ocupación. En Practicar: juego de ordenar pasos (salida, ocupada, baño y apertura). | `vRutina`, `orderGame`, datos en `RUT`, `CODES` |
| A fondo | Limpieza profunda anual; plano de bloqueos con reglas según ocupación y check-list de precauciones; rotación de colchones con esquema; habitación no rentada. | `vFondo`, `matSVG`, `PREC` |
| Frecuencias | Matriz del lobby; proyección quincenal tipo calendario; recorrido de la brigada turno mañana; zonas nobles; tres planillas tipo que se abren con un clic. | `vFrec`, `calendar`, `FREQ`, `CAL_*`, `BRIG`, `NOBLES`, `PLAN` |
| Control | Parte de avería (no se guarda sin sus cinco datos); trabajos pendientes; control de calidad S/R/D/F que genera órdenes; mantenimiento preventivo; prácticas ambientales. | `vControl`, `QC` |

Las preguntas de repaso están en `QUIZ` (35 en total).

En `RUT`, cada paso es `[paso, porqué, subpasos]`; el tercer elemento es opcional. Una rutina puede tener además `note` (aviso en ámbar bajo la intro) y `losNo` (recuadro «Los No» al final).

## De dónde sale el contenido

Fuente única: el **Cuadernillo 2 – Housekeeping 2026** de la cátedra, **revisión de octubre de 2026** (la app se actualizó a esa versión el 7/10/2026), que sistematiza a Simón, M. A. (2004), *Housekeeping ama de llaves*, y a Olmo Garre, *Departamento de Gobernanta*. Apartados usados: Unidad 5 (Housekeeping–Mantenimiento), § 5.3 (nombres de los puestos), § 6.6, 6.9, 6.11 a 6.17, 6.29, 6.30, 7.1 a 7.3.

Segunda fuente, desde el 29/10/2026, solo para las rutinas de **habitación ocupada** y **apertura (turndown)**: FEHGRA, *Manual de Procedimientos para Mucamas* (2012), pp. 40–55 (diez pasos de la ocupada) y pp. 57–60 (apertura, cobertura o turn down). Criterios aplicados:

- Donde el manual dice «Supervisor», la app dice «gobernanta» (criterio de la cátedra).
- Los subpasos salen del texto numerado (1.1, 2.1…). Tres agregados salen de los epígrafes de las fotos: higienizarse las manos al terminar el baño y la habitación (pp. 51 y 54), retirar la colcha antes de abrir la cama y llevar las toallas a la bolsa del carro en la apertura (p. 59).
- El manual tiene dos diferencias entre texto y esquema: en la ocupada (paso 10, p. 55) y en la apertura (cerrar persianas: paso 4 en el esquema de la p. 57, paso 8 en el texto de la p. 58). Se sigue el texto y la app lo avisa.
- Los «Por qué» de estas dos rutinas solo aparecen donde el manual da la razón; el resto queda sin «Por qué».
- Los «Los No» de la ocupada reformulan prohibiciones del manual. Las seis preguntas de repaso nuevas están redactadas a partir del manual.

Regla de trabajo: **no inventar contenido hotelero**. Si algo no está en el cuadernillo, se marca en la app como «Práctica del sector» o «Modelo de ejemplo», o se consulta a la docente.

Cosas que **no** vienen del cuadernillo y están marcadas o pendientes de confirmar con la docente:

- Los colores asignados a cada paño (el cuadernillo pide un color por área, sin decir cuáles).
- Varios «Por qué» de armado de cama y de habitación no rentada.
- Las 35 preguntas de repaso (29 a partir del cuadernillo y 6 a partir del manual de FEHGRA, sin contrastar con parciales).
- Los días de la semana del calendario quincenal (el cuadernillo fija la frecuencia, no el día).
- El formato y los datos de las tres planillas tipo.
- La interpretación de la rotación de colchones: «giro de 180°» = dar vuelta de cara; «cambio cabeza-pies» = girar sobre la cama. **Pendiente de confirmación.**
- El aviso sobre colchones de una sola cara y sus ejemplos.
- Tres prácticas ambientales marcadas «Práctica del sector».
- En la pestaña Hoy: la distribución de las tareas de cada puesto a lo largo del turno (las tareas salen del cuadernillo, el orden y agrupación no) y las cinco tareas de ejemplo de «Mi casa».

El calendario quincenal y la pestaña Hoy comparten el mismo criterio de días: `calIdx(fecha)` devuelve la posición 0–13 dentro de un ciclo de dos semanas que empieza un lunes.

## Preferencias de la docente (respetarlas)

- Idioma: español rioplatense, con voseo («tocá», «elegí»).
- El recuadro de prohibiciones se titula **«Los No»** y cada ítem empieza con «Nunca…» o «Jamás…».
- La práctica se llama **«Preguntas de repaso»**, no «tipo parcial».
- **Gobernanta vs. Ama de Llaves (criterio del cuadernillo, revisión de octubre de 2026):** Ama de Llaves es la cabeza del departamento; gobernanta es la supervisora de pisos. La mucama **retira** el reporte, la llave de piso y las novedades en la oficina del Ama de Llaves (el reporte lo confecciona la gobernanta) y al terminar el turno los **entrega a la gobernanta**, a quien también reporta las habitaciones con No molestar, las averías y los objetos olvidados. Donde esto se aparta de Simón, el cuadernillo aclara «(en Simón: Ama de Llaves)».
- Las pestañas llevan **iconos de línea de un solo tono** (cama, calendario, reloj, planilla). No usar rectángulos de colores ahí: se confunden con el código de paños.
- Paleta tomada de las diapositivas de la cátedra: fondo crema `#f5f3ee`, marrón oscuro `#2a1b18` para texto y bandas de sección, terracota `#a44e33` como acento. En modo oscuro, fondo marrón y acento durazno `#e89d7f`.
- Alto contraste y secciones bien separadas: cada título de sección (`h3`) es una banda oscura de ancho completo.
- Los colores con significado no se tocan: paños, verde/rojo de respuesta, resaltado semanal y quincenal del calendario.
- Quiere visibilidad: tareas a la vista, modelos que se abren con un clic, imágenes cuando ayudan.

## Proyecto de clase relacionado

El diseño del proyecto de vibecoding de los alumnos (cronograma, 17 reglas, prompts por clase, rúbrica, uso de Antigravity gratuito) está en un documento aparte de Claude: «HK Planner: proyecto de vibecoding para Housekeeping». La docente quiere que, además de la materia, los alumnos vean para qué sirve el vibecoding y puedan usar la app en su casa: por eso la pestaña Hoy incluye «Mi casa».
