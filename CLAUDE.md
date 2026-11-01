# HK Planner Hotel Monserrat — contexto del proyecto

Archivo de contexto para quien retome esta app (una persona o un asistente de IA). Leelo antes de tocar `index.html`.

## Qué es

App web de estudio de Housekeeping para alumnos de primer año de la materia **Departamento de Front Office y Housekeeping (2.6.106)**, turno noche, 2.º cuatrimestre 2026. La docente es Helena Havrylets.

Nació como planner (modelo de referencia del proyecto de vibecoding de los alumnos). **Desde el 1/11/2026 cambió de rumbo a pedido de la docente: es un reflejo de las presentaciones de clase de la Unidad 6.** Áreas Públicas y lavandería van a otra app. No incluye la Unidad 5.

- Público: https://hk-planner.vercel.app
- Repositorio: https://github.com/hhavry/hk-planner (rama `main`)
- Copia en Claude (artifact de la docente): https://claude.ai/artifact/KW4jZGd44w7g9PBtbUBFhb

## Cómo está hecha

- Un único archivo `index.html` con HTML, CSS y JavaScript adentro. Sin build, sin dependencias, sin backend.
- Única carga externa: Google Fonts (Bricolage Grotesque para títulos, Figtree para texto), con fuentes de respaldo.
- El avance de cada alumno (pasos tildados, mejores marcas, partes de avería) se guarda en `localStorage`, clave `hkplanner.v1`, solo en su dispositivo. No hay nombres, cuentas ni seguimiento.
- Todo el DOM se arma con la función `h(tag, attrs, ...hijos)`. El texto ingresado por el usuario se inserta siempre con `textContent`.
- Navegación por hash: `#gestion` (pantalla inicial), `#operaciones`, `#repaso`. Los hash viejos `#rutina` y `#gobernanta` redirigen; los demás caen en la inicial. `goTo(pestaña, idDeSección)` abre una pestaña y baja hasta la sección; los ids son `s6-1` … `s6-19` (`sid("6.14")`).
- Debe verse bien en celular (360–400 px) y no tener scroll horizontal de página. Solo las tablas anchas se deslizan dentro de su propio contenedor.

### Publicar un cambio

1. Editar `index.html`.
2. Probarlo abriéndolo en el navegador (y a 400 px de ancho).
3. Commit y push a `main`. Vercel publica solo en el mismo link en un minuto.

El artifact de Claude es el mismo contenido sin `<!DOCTYPE>`, `<html>`, `<head>` ni `<body>` (Claude los agrega al publicar). Si se actualiza uno, actualizar el otro.

## Estructura de la app

Tres pestañas, una por clase más el repaso. Cada pestaña abre con la etiqueta de la clase, el título y la bibliografía de la portada de la PPT, y un índice de secciones. Cada sección es una banda oscura con su número (`secHead`) y debajo la línea de subtemas de su portada.

| Pestaña | Contenido | Funciones en el código |
| --- | --- | --- |
| Gestión de pisos · Unidad 6 (I) | Clase 12, § 6.1 a 6.10: la gobernanta; estados y códigos (matriz, cuatro tablas con inglés, «para no confundir», práctica de lectura con lecturas ocultas); prioridades; Reporte General; Reporte de Pisos (modelo Piso 6, circuito, discrepancias con ejemplo Piso 5, control de seguridad, comparación); organización y cálculo de personal (sin calculadora: los alumnos ya la tienen); chequeo; Lost & Found; formularios; llaves; síntesis. | `vU1`, `U1`, datos en `CODES`, `NOMEN` |
| Personal y operaciones · Unidad 6 (II) | Clase 13, § 6.11 a 6.19: la mucama; carro, productos y office; tarjetas y situaciones al ingresar, con parte de avería interactivo y trabajos pendientes; limpieza de la habitación (principio, ocupada / salida / turn down con casillas, cuadro comparativo); baño; no rentada y rotación de colchones con esquema; armado de la cama (sistemas, esquinas, calidad, prevención); dotación estándar; runner y valet; síntesis. | `vU2`, `U2`, `rutBlock`, `parteAveria`, `matSVG`, datos en `RUT`, `PROD`, `CMP`, `SYS`, `VIDS` |
| Repaso | Preguntas de repaso filtrables por parte (toda la unidad, 6 I, 6 II) o por sección, y «Ordená los pasos» (ocupada, salida, turn down, baño, cama). | `vRep`, `quiz(lista, clave)`, `orderGame`; `QUIZ` es una lista plana con `s:"6.x"` |

`QUIZ` tiene 54 preguntas, cada una con su sección en `s`. La mejor marca se guarda por filtro (`S.best["parte-u1"]`, `S.best["sec-6.14"]`).

En `RUT`, cada paso es `[paso, porqué, subpasos, alTerminar]`; del segundo en adelante son opcionales. Los subpasos se numeran solos (1.1, 1.2…). Una rutina puede tener además `note`, `losNo`, `boxes` (`[título, ítems, numerado]`), `warn`, `vids` y `sys`. El avance tildado se guarda por rutina y cantidad de pasos (`S.done["ocupada@10"]`).

**Qué se sacó el 1/11/2026, a pedido de la docente:** las pestañas Hoy (con «Mi casa»), A fondo (limpieza profunda y plano de bloqueos), Frecuencias (Áreas Públicas, va a otra app) y Control (control de calidad S/R/D/F, prácticas ambientales); la calculadora de efectivos; la Unidad 5 (nombres de los puestos); y todas las notas de actualización y «Hotelería hoy» (plazo del No molestar, dotación en desuso, libres de humo, papel → PMS, RFID, convenios, tickets digitales, llaves digitales, limpieza a pedido). No volver a agregarlos sin consultarla.

## De dónde sale el contenido

Fuentes, en este orden de prioridad:

1. **Las presentaciones de clase de la Unidad 6** (PDF en la carpeta «HK/Subido» de la docente): parte I, Clase 12, «Gestión de Pisos: supervisión, reportes y control», § 6.1 a 6.10 (dos PDF: 6.1–6.3 y 6.4–6.10); parte II, Clase 13, «El personal de limpieza y los procesos de operaciones», § 6.11 a 6.19 (dos PDF: 6.11–6.13 y 6.14–6.19, versión del 1/11/2026). Numeración: 6.14 limpieza de la habitación (ocupada, salida, comparativo y turn down), 6.15 baño, 6.16 no rentada y colchones, 6.17 armado de cama, 6.18 dotación, 6.19 runner y valet. La app tiene que ser fiel a ellas: secciones, títulos, procedimientos, nombres y ejemplos salen de ahí. Varias diapositivas son imágenes (rotación de colchones, esquinas, rutina diaria, cálculo de efectivos): renderizarlas antes de decir que algo no está.
2. Lo que no está en las presentaciones no entra, salvo que la docente lo pida.

Historial: el 29/10/2026 la app se alineó con la presentación de la Unidad 6 (II) y el 1/11/2026 pasó a la estructura actual.

Cambios del 29/10/2026 para alinear la app con la presentación:

- La habitación de salida dejó de ser la rutina de 15 pasos del cuadernillo: ahora son los 8 pasos de FEHGRA (pp. 60-64), y la ocupada es el procedimiento base de 10 pasos.
- El baño dejó de ser la secuencia de 8 pasos de Simón: ahora son los 7 de FEHGRA (cerámicos, sanitarios, accesorios, puertas y espejos, grifería, piso, examinar). Amenities y toallas se reponen en el paso 8 de la habitación. La pregunta «¿por qué el inodoro es lo último?» se reemplazó por «¿por qué cepillo y paño exclusivos?».
- El armado de cama son las 6 capas de la diapositiva de § 6.16. Las fundas van con la abertura hacia el centro de la cama (Simón); «pie de cama» en FEHGRA es la alfombrita descartable del turn down.
- Los productos básicos son ocho, no siete; paños: «Muebles y superficies» y «Vasos y vidrios», con los colores de la diapositiva 15 (rojo `#c0473a`, ámbar `#d9a43a`, azul `#4d7ea8`, coral `#d9785e`; en modo oscuro, versiones más claras). Pedido de la docente: la presentación ya está publicada con esos colores.
- Donde FEHGRA dice «Supervisor», la app dice «el supervisor (la gobernanta)», como la clase.
- A pedido de la docente, la salida suma subpasos de clase que FEHGRA no trae: vajilla de Room Service al pasillo, del cesto solo los papeles de adentro, retirar incluso las toallas que parecen sin usar, contar la ropa blanca y revisar daños (paso 3); aspirar desde el fondo hacia la puerta (paso 4); rociar el baño y dejar actuar mientras se arma la cama (paso 5); aromatizar, cerrar, reporte de piso con hora de entrada y salida y volver al office (paso 8).

Cambios del 1/11/2026 (PPT nuevas de la carpeta «Subido»), primera pasada; ese mismo día la app se reestructuró en las tres pestañas actuales y se sacaron las actualizaciones:

- Pestaña nueva Gobernanta con la Unidad 6 (I). Estados de habitación y prioridades se mudaron ahí desde Rutina, actualizados: códigos con equivalencia en inglés, LT y Cu/Cx; con baja ocupación, VL → VIP → ocupadas que continúan → vacías sucias y salidas, más la lista «sin tarea».
- Referencias de § renumeradas según la PPT nueva (ver arriba), también en las preguntas.
- Ocupada: 41 subpasos (el 7.7 «higienizarse y controlar» pasó a ser subpaso).
- Nuevos: cuadro comparativo ocupada vs. check out; cuatro sistemas de ropa de cama (nórdica y mixta = práctica del sector); esquinas en sobre (Simón p. 69) y en inglete (industria); complementos de dotación «Hoy»; convenio de la mucama con la redacción nueva (asistencia 10 %, complemento 12 %, CCT 362/03 «para los hoteles de su ámbito»); matices de runner y valet hoy.
- La diapositiva de rotación de colchones confirma la interpretación de la app (180° = dar vuelta de cara; cabeza-pies = girar sobre la cama).
- Videos: links encontrados por título y pendientes de confirmar por la docente: FEHGRA 1.4 «Proceso de limpieza de baño» (youtube 2qOMBlwt3KU), L-gic esquinas (zekqwevi9lo), UGT «03 Buenas prácticas… Hacer la cama» (Qhv_gROwu3M). FEHGRA 1.1, 1.2 y 1.3 están cargados en `VIDS` sin link: no se muestran hasta tener la URL.

Diferencias dentro de la propia presentación, **pendientes de que la docente las corrija** (la app sigue el contenido de las diapositivas, no las portadas ni el cierre):

- Portada de 6.12: todavía dice «Los siete productos»; la diapositiva de productos dice ocho.
- Síntesis de la Unidad 6 (II): «El inodoro, al final y con su paño». En la secuencia de FEHGRA los sanitarios son el paso 2.
- 6.13, No molestar: una línea dice que a las 13 h se informa «a la ama de llaves» y la misma diapositiva dice «informa a la gobernanta». La app usa gobernanta (criterio del cuadernillo).
- La diapositiva de prevención de riesgos está en el PDF de 6.11–6.13 pero rotulada «6.16».

Regla de trabajo: **no inventar contenido hotelero**. Si algo no está en el cuadernillo, se marca en la app como «Práctica del sector» o «Modelo de ejemplo», o se consulta a la docente.

Cosas que **no** salen textuales de las presentaciones y están marcadas o pendientes de confirmar con la docente:

- Varios «Por qué» de los procedimientos (armado de cama, habitación no rentada).
- Las 54 preguntas de repaso (redactadas a partir de las presentaciones y del manual de FEHGRA, sin contrastar con parciales).
- Los videos: links encontrados por título (ver arriba), pendientes de confirmar; FEHGRA 1.1, 1.2 y 1.3 sin link.
- Nórdica y mixta (ropa de cama) y la columna «Hoy, además» de la dotación: así figuran en la PPT, como práctica del sector o industria.

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

El diseño del proyecto de vibecoding de los alumnos (cronograma, 17 reglas, prompts por clase, rúbrica, uso de Antigravity gratuito) está en un documento aparte de Claude: «HK Planner: proyecto de vibecoding para Housekeeping». Desde el 1/11/2026 esta app ya no tiene la pestaña Hoy ni «Mi casa»: es material de estudio de la Unidad 6.
