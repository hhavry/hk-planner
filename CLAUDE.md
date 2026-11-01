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
- Navegación por hash: `#rutina` (pantalla inicial), `#gobernanta`, `#fondo`, `#frecuencias`, `#control`, `#hoy`. `goTo([pestaña, rutina, idDeSección])` abre una pestaña y, si se le pasa el tercer elemento, baja hasta esa sección (ids en Gobernanta: `codigos`, `prioridades`, `reporte-general`, `reporte-pisos`, `organizacion`, `calculo`, `chequeo`, `lost-found`, `formularios`, `llaves`; en Rutina: `nm`).
- Debe verse bien en celular (400 px) y no tener scroll horizontal de página. Solo tablas y el calendario se deslizan dentro de su propio contenedor.

### Publicar un cambio

1. Editar `index.html`.
2. Probarlo abriéndolo en el navegador (y a 400 px de ancho).
3. Commit y push a `main`. Vercel publica solo en el mismo link en un minuto.

El artifact de Claude es el mismo contenido sin `<!DOCTYPE>`, `<html>`, `<head>` ni `<body>` (Claude los agrega al publicar). Si se actualiza uno, actualizar el otro.

## Estructura de la app

Seis pestañas. **Hoy** va última, después de Control, en amarillo (`--hoy`) y en verde oscuro cuando está activa (`--hoy-on`): pedido expreso de la docente. Las otras cinco tienen modo **Estudiar** y modo **Practicar** (preguntas de repaso con explicación; mejor marca guardada).

| Pestaña | Contenido | Funciones en el código |
| --- | --- | --- |
| Hoy | Qué toca hacer día por día, con flechas para cambiar de fecha y casillas para tildar. Selector de puesto: Mucama, Áreas Públicas, Gobernanta y **Mi casa**. Las tareas enlazan a los procedimientos de las otras pestañas; la Gobernanta ve además los bloqueos activos y los partes urgentes. En Mi casa el alumno carga sus propias tareas con frecuencia (diaria, día por medio, semanal, quincenal, mensual). | `vHoy`, `HOY`, `dueOn`, `goTo`, `calIdx`; estado en `S.hoy`, `S.casa`, `S.puesto` |
| Rutina | Seis procedimientos, en el orden de la clase: habitación ocupada (FEHGRA, 10 pasos y 41 subpasos numerados, con cuadro comparativo ocupada vs. check out), habitación de salida (FEHGRA, 8 pasos), baño (FEHGRA, 7 pasos), armado de cama (6 capas, cuatro sistemas de ropa de cama y esquinas en sobre y en inglete), turn down (FEHGRA, 11 pasos, con técnica del triángulo y tres niveles) y habitación no rentada (Simón, 8 pasos). Debajo, siguiendo la PPT de la Unidad 6 (II): la mucama (presentación personal, tareas, rutina diaria, por qué revisar las libres, Olmo Garre); carro, tríada y office; paños, herramientas, los 8 productos con dónde y cómo, Los SÍ / Los No, escala de pH, abrasivos, ceras y lustramuebles; No molestar con la actualización; tarjetas, situaciones al ingresar y novedades obligatorias; dotación estándar con los complementos «Hoy» de la industria; nombres de los puestos, convenio, runner y valet; limpieza a pedido. Los estados y las prioridades se mudaron a Gobernanta (queda un recuadro con enlaces). Botones «Videos de la clase» en baño y armado de cama (`VIDS`, `vidRow`). En Practicar: ordenar pasos (ocupada, salida, baño, cama y turn down). | `vRutina`, `orderGame`, `box`, `wide`, `vidRow`, datos en `RUT`, `PROD`, `CMP`, `SYS`, `VIDS` |
| Gobernanta | Unidad 6 (I), § 6.1 a 6.10, en el orden de la PPT: la gobernanta (áreas, turno, dotación y tiempo, imprevistos, exigencias y carrera); estados y códigos (matriz, cuatro tablas con inglés, práctica de lectura con lecturas ocultas); prioridades; Reporte General; Reporte de Pisos (modelo Piso 6, circuito, discrepancias con ejemplo Piso 5, control de seguridad, comparación); organización del trabajo, topes por convenio, factores, créditos, schedule; calculadora «¿Cuántas mucamas necesita el hotel?» con el ejemplo de clase (600 hab., 75 % → 55 mucamas); chequeo; Lost & Found; formularios; llaves; síntesis. | `vGob`, `calcEf`, `sec`, `sheetToggle`, datos en `CODES`, `NOMEN`, `TOPES`, `CALC0` |
| A fondo | Limpieza profunda anual; plano de bloqueos con reglas según ocupación y check-list de precauciones; rotación de colchones con esquema; habitación no rentada. | `vFondo`, `matSVG`, `PREC` |
| Frecuencias | Matriz del lobby; proyección quincenal tipo calendario; recorrido de la brigada turno mañana; zonas nobles; tres planillas tipo que se abren con un clic. | `vFrec`, `calendar`, `FREQ`, `CAL_*`, `BRIG`, `NOBLES`, `PLAN` |
| Control | Parte de avería (no se guarda sin sus cinco datos); trabajos pendientes; control de calidad S/R/D/F que genera órdenes; mantenimiento preventivo; prácticas ambientales. | `vControl`, `QC` |

Las preguntas de repaso están en `QUIZ` (64 en total: 27 de rutina, 18 de gobernanta, 6 de fondo, 7 de frecuencias, 6 de control).

En `RUT`, cada paso es `[paso, porqué, subpasos, alTerminar]`; del segundo en adelante son opcionales. Los subpasos se numeran solos (1.1, 1.2…). Una rutina puede tener además `note` (aviso en ámbar bajo la intro), `losNo` (recuadro «Los No»), `boxes` (recuadros al final: `[título, ítems, numerado]`) y `warn` (aviso en ámbar al final). El avance tildado se guarda por rutina y cantidad de pasos (`S.done["ocupada@10"]`), para que un cambio de pasos no herede tildes viejas.

## De dónde sale el contenido

Fuentes, en este orden de prioridad:

1. **Las presentaciones de clase** (PDF en la carpeta «HK/Subido» de la docente): Unidad 5 completa; Unidad 6 (I), Clase 12, «Gestión de Pisos: supervisión, reportes y control», § 6.1 a 6.10, que alimenta la pestaña Gobernanta; y Unidad 6 (II), Clase 13, «El personal de limpieza y los procesos de operaciones», en dos PDF: § 6.11 a 6.13 y § 6.14 a 6.19 (versión del 1/11/2026). La numeración nueva: 6.14 limpieza de la habitación (ocupada, salida, comparativo y turn down), 6.15 baño, 6.16 no rentada y colchones, 6.17 armado de cama, 6.18 dotación, 6.19 runner y valet. La pestaña Rutina tiene que ser fiel a ella: procedimientos, nombres, ejemplos y actualizaciones salen de ahí. A su vez sigue a Simón (pp. 51-64 y 81-83), a Olmo Garre (Caps. 2 y 7) y a FEHGRA (2012), *Manual de procedimientos para mucamas*.
2. El **Cuadernillo 2 – Housekeeping 2026** de la cátedra, **revisión de octubre de 2026**, para todo lo que la presentación no cubre: Unidad 5 (Housekeeping–Mantenimiento), § 5.3 (nombres de los puestos), § 6.6, 6.9, 6.29, 6.30 y 7.1 a 7.3.

Cambios del 29/10/2026 para alinear la app con la presentación:

- La habitación de salida dejó de ser la rutina de 15 pasos del cuadernillo: ahora son los 8 pasos de FEHGRA (pp. 60-64), y la ocupada es el procedimiento base de 10 pasos.
- El baño dejó de ser la secuencia de 8 pasos de Simón: ahora son los 7 de FEHGRA (cerámicos, sanitarios, accesorios, puertas y espejos, grifería, piso, examinar). Amenities y toallas se reponen en el paso 8 de la habitación. La pregunta «¿por qué el inodoro es lo último?» se reemplazó por «¿por qué cepillo y paño exclusivos?».
- El armado de cama son las 6 capas de la diapositiva de § 6.16. Las fundas van con la abertura hacia el centro de la cama (Simón); «pie de cama» en FEHGRA es la alfombrita descartable del turn down.
- Los productos básicos son ocho, no siete; paños: «Muebles y superficies» y «Vasos y vidrios», con los colores de la diapositiva 15 (rojo `#c0473a`, ámbar `#d9a43a`, azul `#4d7ea8`, coral `#d9785e`; en modo oscuro, versiones más claras). Pedido de la docente: la presentación ya está publicada con esos colores.
- Donde FEHGRA dice «Supervisor», la app dice «el supervisor (la gobernanta)», como la clase.
- A pedido de la docente, la salida suma subpasos de clase que FEHGRA no trae: vajilla de Room Service al pasillo, del cesto solo los papeles de adentro, retirar incluso las toallas que parecen sin usar, contar la ropa blanca y revisar daños (paso 3); aspirar desde el fondo hacia la puerta (paso 4); rociar el baño y dejar actuar mientras se arma la cama (paso 5); aromatizar, cerrar, reporte de piso con hora de entrada y salida y volver al office (paso 8).

Cambios del 1/11/2026 (PPT nuevas de la carpeta «Subido»):

- Pestaña nueva Gobernanta con la Unidad 6 (I). Estados de habitación y prioridades se mudaron ahí desde Rutina, actualizados: códigos con equivalencia en inglés, LT y Cu/Cx; con baja ocupación, VL → VIP → ocupadas que continúan → vacías sucias y salidas, más la lista «sin tarea».
- Referencias de § renumeradas según la PPT nueva (ver arriba), también en las preguntas.
- Ocupada: 41 subpasos (el 7.7 «higienizarse y controlar» pasó a ser subpaso).
- Nuevos: cuadro comparativo ocupada vs. check out; cuatro sistemas de ropa de cama (nórdica y mixta = práctica del sector); esquinas en sobre (Simón p. 69) y en inglete (industria); complementos de dotación «Hoy»; nota de hoteles libres de humo en el baño; convenio de la mucama con la redacción nueva (asistencia 10 %, complemento 12 %, CCT 362/03 «para los hoteles de su ámbito»); matices de runner y valet hoy.
- La diapositiva de rotación de colchones confirma la interpretación de la app (180° = dar vuelta de cara; cabeza-pies = girar sobre la cama).
- Videos: links encontrados por título y pendientes de confirmar por la docente: FEHGRA 1.4 «Proceso de limpieza de baño» (youtube 2qOMBlwt3KU), L-gic esquinas (zekqwevi9lo), UGT «03 Buenas prácticas… Hacer la cama» (Qhv_gROwu3M). FEHGRA 1.1, 1.2 y 1.3 están cargados en `VIDS` sin link: no se muestran hasta tener la URL.

Diferencias dentro de la propia presentación, **pendientes de que la docente las corrija** (la app sigue el contenido de las diapositivas, no las portadas ni el cierre):

- Portada de 6.12: todavía dice «Los siete productos»; la diapositiva de productos dice ocho.
- Síntesis de la Unidad 6 (II): «El inodoro, al final y con su paño». En la secuencia de FEHGRA los sanitarios son el paso 2.
- 6.13, No molestar: una línea dice que a las 13 h se informa «a la ama de llaves» y la misma diapositiva dice «informa a la gobernanta». La app usa gobernanta (criterio del cuadernillo).
- La diapositiva de prevención de riesgos está en el PDF de 6.11–6.13 pero rotulada «6.16».

Regla de trabajo: **no inventar contenido hotelero**. Si algo no está en el cuadernillo, se marca en la app como «Práctica del sector» o «Modelo de ejemplo», o se consulta a la docente.

Cosas que **no** vienen del cuadernillo y están marcadas o pendientes de confirmar con la docente:

- Varios «Por qué» de armado de cama y de habitación no rentada.
- Las 64 preguntas de repaso (redactadas a partir del cuadernillo, del manual de FEHGRA y de las presentaciones de la Unidad 6, sin contrastar con parciales).
- En la calculadora de efectivos: la opción «2 francos (40 h)» y el margen de licencias aplicado como porcentaje sobre la plantilla (el ejemplo de clase usa 1 franco y 0 %).
- Los días de la semana del calendario quincenal (el cuadernillo fija la frecuencia, no el día).
- El formato y los datos de las tres planillas tipo.
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
