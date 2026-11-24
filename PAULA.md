# Paula: personaje y estado del mod

## El personaje

Paula tiene 35 años, trabaja como stripper y es divertida y carismática.
Quiere cambiar su vida y reencontrarse con su hija, que vive con su padre.
Está pensada como una **pasajera que hace dedo**, no como el personaje
conductor seleccionado al empezar la partida.

Su definición está en [hitchhikers.kdr](./hitchhikers.kdr). Paula apareció
durante varias partidas. En las primeras pruebas hubo referencias de sprites
inválidas y algunas imágenes se veían demasiado grandes. En la prueba más
reciente, las capturas muestran a Paula en el auto, en el menú y en la ciudad;
Leonardo confirmó que los iconos tienen buen tamaño y que pudo jugar eventos
sin errores. Esta prueba es un avance: aún falta comprobar por separado las
tres animaciones nuevas y reemplazar los recursos gráficos provisionales.

## Archivos del mod

| Archivo | Contenido actual |
| --- | --- |
| [info.kdr](./info.kdr) | Metadatos del mod. |
| [hitchhikers.kdr](./hitchhikers.kdr) | Datos de Paula y referencias a habilidades, diálogos, misiones y sprites. |
| [skills.kdr](./skills.kdr) | Tres definiciones de habilidades activas. |
| [dialogue.kdr](./dialogue.kdr) | Textos de saludos y conversaciones, en español. |
| [banter.kdr](./banter.kdr) | Conversaciones automáticas de Paula durante el viaje. |
| [quests.kdr](./quests.kdr) | La misión única de Paula (versión 1). |
| [items.kdr](./items.kdr) | El Corpiño de Paula, definido pero sin misión que lo entregue. |
| [images.kdr](./images.kdr) | Configuración de sprites, como posición y velocidad. |
| [images](./images/) | Sprites de Paula, iconos de habilidades y del objeto de recompensa. |

Las carpetas [Keep Driving](./Keep%20Driving/) y [3810584840](./3810584840/)
contienen archivos de referencia: respectivamente, los datos originales
aportados y el mod de ejemplo. No son archivos propios de Paula.

## Habilidades y pasivas

La progresión y los IDs nativos seleccionados quedan así:

| Nivel | Habilidad activa | Costo | Usos | Efecto/patrón | Pasiva |
| --- | --- | --- | --- | --- | --- |
| 1 | `distraction_dance` — Baile distractor | Sin costo; TURBO (`swift: 1`) | 3 | Otorga 4 unidades de dinero | `street_musician` — Siempre consigue una propina |
| 2 | `seduction_charm` — Seducción | 1 energía | 3 | Elimina dos iconos de energía (`EE`) | `loan_sharks` — Una vida complicada |
| 3 | `night_owl` — Búho nocturno | 2 energía | 2 | Comodín (`AA`) | `flee_easy` — Instinto de supervivencia |

La documentación oficial define `D/E/G/M/X/A` como durabilidad, energía,
gasolina, dinero, ninguno y todos los tipos, respectivamente. Baile distractor
usa el tipo nativo `resource` para otorgar dinero y `swift: 1` para que sea
TURBO. Seducción es una habilidad `basic` que elimina dos iconos `E` y cuesta
una energía. Se probaron combinaciones previas de habilidades `basic` y
`resource`, pero el juego ignoró el efecto de recurso; por eso los dos efectos
actuales están separados en sus respectivas habilidades. Falta validar en
juego las cuatro unidades de dinero, el efecto TURBO y el patrón `EE`.
`uses` establece la cantidad de usos. La recarga al dormir es un requisito de
diseño; el formato KDR consultado no documenta una opción de recarga por
habilidad, así que debe confirmarse en el juego.

Los IDs de pasivas apuntan a mecánicas nativas parecidas, no idénticas a los
nombres y efectos propuestos:

| Pasiva de Paula | ID nativo | Mecánica de referencia / efecto deseado |
| --- | --- | --- |
| Siempre consigue una propina | `street_musician` | Da algo de dinero tras pasar una noche en un destino; no especifica ciudad ni el rango de 3 a 13. |
| Una vida complicada | `loan_sharks` | Puede provocar un encuentro con prestamistas al salir de un destino; no especifica un evento de carretera aleatorio ni 50%. |
| Instinto de supervivencia | `flee_easy` | La descripción original indica menos daño al escapar; Paula recibe 1 de armadura al huir, según la aclaración de Leonardo. |

Aunque la documentación permite redefinir datos por ID, esos overrides son
globales: `street_musician` también lo usa Songwriter y `loan_sharks` Stranger.
Para no cambiar a otros pasajeros, se mantienen los nombres e iconos nativos;
los iconos personalizados de Paula quedan sin conectar hasta encontrar una
forma de limitar el cambio a ella.

## Aparición durante el viaje

Paula apareció durante un viaje en una prueba. No se configuró una fecha,
ubicación ni probabilidad de aparición, y no está confirmado cómo forzarla.

## Diálogos y misiones

**Versión 1: una sola misión.** La definición está en [quests.kdr](./quests.kdr)
y empieza al recoger a Paula (`pick_up_quest: paula_lawyer_trip`). La cadena
de tres misiones no funcionó: tras completar la primera nunca volvía a
aparecer **Hablar** ni se ofrecía la segunda, ni siquiera con la estructura
de June, tras subir de nivel y con una misión `main` como la de la pareja.
Como la documentación oficial de misiones y diálogos aún está sin completar
("TBA"), la cadena queda para una **versión 2**.

**Misión "Un abogado para Emma"** (`quest_type: hh`, `quest_days: 20`).
Los objetivos son tres y el juego no obliga a cumplirlos en orden:

1. Conseguir comida (`obj_item_tag: food`).
2. Conseguir una bebida (`obj_item_tag: beverage`).
3. Llevar a Paula con el abogado, en la ciudad del este (`obj_dest: hidden`,
   `obj_new_dest_id: city_east`). Con `friends_home` la misión funcionó bien
   en las pruebas.

Recompensa: 50 de dinero y 1 punto de habilidad. Al completarla, Paula dice
que habló con el abogado, que el acuerdo de custodia llevará tiempo y pide
seguir en el auto mientras espera (`paula_lawyer_trip_complete`). Las charlas
sueltas son `paula_talk` y `paula_case_wait`.

**Pendiente para la versión 2** (textos ya escritos en
[dialogue.kdr](./dialogue.kdr), sin conectar): `paula_request_fee` y
`paula_lawyer_fee_complete` (última noche en el escenario), `paula_custody_news`,
`paula_final_trip` y `paula_reunion_complete` (reencuentro con Emma), y la
recompensa del objeto `paula_stage_bustier`, hoy sin misión que lo entregue.

Las conversaciones automáticas durante el viaje están en [banter.kdr](./banter.kdr).
Se añadieron cinco intercambios: uno general y otros condicionados por lluvia,
noche, música y cielo despejado. El formato observado no permite asociarlos
de forma confirmada a la finalización de una misión; pueden aparecer cuando
se cumpla su condición y Paula viaje en el auto.

El objeto de combate **Corpiño de Paula** (15 usos, patrón `AA`) sigue
definido. Su efecto experimental de 5 unidades de dinero por uso
(`cons_stat: money`, `cons_amount: 5`) no está confirmado por la
documentación. No se entrega todavía.

En la captura más reciente, los retratos del retrovisor se ven difuminados
cuando no están seleccionados. Se deja este comportamiento para comparar más
adelante con June; no se modificó la imagen estática de Paula.

## Sprites

El inventario incluye **3 animaciones preparadas y 6 iconos de perk**
(3 activos y 3 pasivos). Los tres iconos activos están enlazados en
`skills.kdr` y ahora incluyen un marco blanco tipo Polaroid. Los pasivos
siguen sin conectar para evitar cambiar globalmente a otros pasajeros.
Las dimensiones son ancho × alto; en las tiras se indica también el tamaño
por cuadro.

### Animaciones preparadas

| Archivo | Tamaño total | Cuadros | Uso y estado |
| --- | --- | --- | --- |
| `spr_paula_idle_strip8.png` | 312 × 64 | 8 de 39 × 64 | Reposo/espera en la ruta; el arte se redujo proporcionalmente dentro de una caja de 35 × 57 px por cuadro y quedó alineado por los pies. Falta validar el nuevo tamaño en el juego. |
| `spr_paula_hitch_strip6.png` | 234 × 64 | 6 de 39 × 64 | Paula haciendo dedo. Ya está asignada a `sprite_hitch`; falta probarla en el juego. |
| `spr_paula_sitting_strip7.png` | 329 × 36 | 7 de 47 × 36 | Paula sentada al bajarse. Ya está asignada a `sprite_kicked`; falta probarla en el juego. |

Las imágenes anteriores `spr_paula_idle.png` y `spr_paula_hitch.png` se
conservaron con la extensión `.disabled` para que no compitan con sus tiras.
La tira vieja `spr_paula_sitting_strip12.png` también se conservó como
`.png.disabled`.

### Imágenes y estado de los iconos

Los retratos y los seis iconos de perk se prepararon en pixel art. Los iconos
activos están referenciados en `skills.kdr`; los pasivos siguen sin conectar
para evitar cambios globales. Falta validar en el juego el icono del nuevo
objeto de recompensa.

| Archivo final esperado | Tamaño esperado | Estado actual |
| --- | --- | --- |
| `spr_paula_portrait.png` | 40 × 40 | Asignado a `sprite_portrait`; el fondo crema quedó al 50 % de opacidad para que se vea el asiento, mientras que el personaje y el marco siguen opacos. La versión anterior se conservó como `spr_paula_portrait_before_seat_transparency.png.disabled`; falta comprobarlo en el juego. |
| `spr_paula_rw_big.png` | 64 × 64 | Asignado a `sprite_rw_big`; la figura se amplió a 50 × 62 px, conservando el fondo transparente. La versión anterior se conservó como `spr_paula_rw_big_before_resize.png.disabled`; falta comprobarlo en el juego. |
| `spr_paula_rw_small.png` | 42 × 42 | Asignado a `sprite_rw_small`; la figura se amplió a 35 × 40 px, conservando el fondo transparente. La versión anterior se conservó como `spr_paula_rw_small_before_resize.png.disabled`; falta comprobarlo en el juego. |
| `spr_paula_portrait_hitchermenu.png` | 71 × 71 | Asignado a `sprite_menu_portrait`; el fondo se dejó al 50 % de opacidad para que se vea el asiento. La versión anterior se conservó como `spr_paula_portrait_hitchermenu_before_seat_transparency.png.disabled`; falta comprobarlo en el juego. |
| `spr_paula_portrait_full_sleep.png` | 64 × 64 | Generado en pixel art y asignado a `sleep_sprite_full`; se limpió el alfa para dejar el personaje opaco y el fondo transparente, como en los retratos del retrovisor. Falta comprobar la nitidez en el juego; la imagen original se conservó como `.png.disabled`. |
| `spr_paula_portrait_small_sleep.png` | 42 × 42 | Derivado del retrato grande para mantener la misma pose; se aplicó la misma limpieza de alfa. Falta comprobar la nitidez en el juego; la versión provisional se conservó como `.png.disabled`. |
| `spr_paula_button_strip2.png` | 24 × 12 | Dos cuadros de 12 × 12; el icono se rehízo a partir del retrato de Paula y ocupa 11 × 12 px por cuadro. `sprite_button` ya referencia la base `spr_paula_button`; falta comprobarlo en el juego. Las versiones anteriores se conservaron como archivos `.png.disabled`. |
| `spr_paula_incar_strip3.png` | 468 × 64 total (3 cuadros de 156 × 64) | Tira de busto sentado reducida a 15 × 21 px por cuadro y centrada dentro del auto. `sprite_car` conserva la misma referencia y offset. La tira original de cuerpo entero se preservó como `spr_paula_incar_strip3.png.disabled`; falta probar este tamaño ajustado en el juego. |
| `spr_ui_skill_seduction_charm.png` | 32 × 38 | Icono sobre tarjeta blanca tipo Polaroid; enlazado a `seduction_charm`. La versión sin tarjeta se conservó como `spr_ui_skill_seduction_charm_icon_only.png.disabled`; falta probarlo en juego. |
| `spr_ui_skill_distraction_dance.png` | 32 × 38 | Icono sobre tarjeta blanca tipo Polaroid; enlazado a `distraction_dance`. La versión sin tarjeta se conservó como `spr_ui_skill_distraction_dance_icon_only.png.disabled`; falta probarlo en juego. |
| `spr_ui_skill_night_owl.png` | 32 × 38 | Icono sobre tarjeta blanca tipo Polaroid; enlazado a `night_owl`. La versión sin tarjeta se conservó como `spr_ui_skill_night_owl_icon_only.png.disabled`; falta probarlo en juego. |
| `spr_paula_stage_bustier_strip1.png` | 16 × 16 | Icono pixel art del objeto de recompensa, enlazado a `paula_stage_bustier`; falta comprobarlo en el inventario y los eventos. |
| `spr_paula_passive_street_musician.png` | 32 × 38 | Generado con fondo transparente; no está conectado porque `street_musician` también lo usa otro pasajero. |
| `spr_paula_passive_loan_sharks.png` | 32 × 38 | Generado con fondo transparente; no está conectado porque `loan_sharks` también lo usa otro pasajero. |
| `spr_paula_passive_flee_easy.png` | 32 × 38 | Generado con fondo transparente; no está conectado para evitar un override global. |

Los tres iconos activos personalizados ya están referenciados por sus nombres
base en `skills.kdr`, con fondos de tarjeta blancos. Los iconos pasivos siguen
sin enlazarse porque sus IDs nativos se comparten con otros pasajeros. Falta
probar en el juego los iconos activos y confirmar que cargan con el tamaño y
la transparencia esperados.

## Próximos pasos

1. Probar en el juego las parejas de habilidades por nivel y sus efectos
   pasivos.
2. Confirmar que los usos de las habilidades activas se recargan al dormir.
3. Revisar las misiones, los sprites e iconos faltantes y la tira animada.
4. Probar la misión única de Paula (comida, bebida y destino) y confirmar
   si se completa al llegar al destino antes de cumplir los otros objetivos.
   Versión 2: cadena de misiones cuando haya más documentación.
5. Comprobar la compatibilidad de habilidades, diálogos y demás referencias.

Para crear otros pasajeros, ver [Guía general](./GUIA_CREAR_PERSONAJES.md).
Para referencias y pruebas, ver [Referencias y pruebas](./REFERENCIA_Y_PRUEBAS.md).
