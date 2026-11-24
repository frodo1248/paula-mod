# Guía general: crear un pasajero para Keep Driving

Esta guía resume cómo organizar los datos de un personaje que hace dedo.
Para el estado concreto de Paula, consultá [Paula](./PAULA.md). La
[documentación oficial](https://whycjwhy.com/keepdriving/mods/) es la
referencia principal; algunas partes aún están incompletas.

## 1. Preparar el mod

En Windows, los mods locales se guardan en `%localappdata%\KeepDriving\mods\`.
Creá una carpeta propia para tu mod y colocá allí sus archivos. Un esquema
habitual es:

```text
mi-mod/
  info.kdr
  hitchhikers.kdr
  skills.kdr
  dialogue.kdr
  images.kdr
  images/
```

Los archivos KDR son parecidos a JSON: contienen bloques entre llaves,
permiten comentarios `//` y algunas cadenas sin comillas. A diferencia de
JSON, el formato de los ejemplos no requiere comas entre propiedades.
`info.kdr` describe el mod; los demás archivos contienen las definiciones
que necesites. Consultá [la estructura KDR oficial](https://whycjwhy.com/keepdriving/mods/kdr/).

Usá identificadores internos distinguibles para evitar conflictos con el
juego y otros mods. Para consultar la forma de los metadatos, compará
[info.kdr de Paula](./info.kdr) y la
[documentación de info](https://whycjwhy.com/keepdriving/mods/kdr/info/).

## 2. Definir el pasajero

En `hitchhikers.kdr` se declara cada pasajero: identificador (`dev_name`),
nombre visible (`hh_name`), descripción, edad, habilidad inicial y
referencias a diálogos, sprites y perks. Los ejemplos completos están en
[los pasajeros originales](./Keep%20Driving/hitchhikers.kdr) y
[el mod de ejemplo](./3810584840/hitchhikers.kdr).

El pasajero se plantea como alguien que el conductor puede encontrar
durante el viaje. No hay un campo de ubicación o probabilidad de aparición
en la [lista oficial de propiedades](https://whycjwhy.com/keepdriving/mods/kdr/hitch-hikers/);
el mecanismo exacto de selección para mods necesita verificación.

## 3. Habilidades activas y pasivas

En los ejemplos, cada perk se declara con `perk_id`, `perk_type`, `perk_dia`
y `perk_end`. El tipo puede ser `skill` para una habilidad activa o
`passive` para una pasiva. La progresión deseada para Paula es una activa
y una pasiva por cada uno de tres niveles; confirmá cómo interpreta el juego
el orden de las entradas antes de asumir que el orden las asigna a un nivel.

Las habilidades activas personalizadas se definen en `skills.kdr`; sus
identificadores deben coincidir con las referencias del pasajero. Compará
[las habilidades de ejemplo](./3810584840/skills.kdr) y la
[documentación oficial](https://whycjwhy.com/keepdriving/mods/kdr/skills/).

Las pasivas originales se pueden consultar en
[passive_skills.kdr del juego](./Keep%20Driving/passive_skills.kdr). Antes
de añadir una pasiva nueva, verificá si el sistema de mods permite
implementar el efecto deseado: un identificador o una descripción por sí
solos no demuestran que exista una mecánica funcional.

## 4. Diálogos y misiones

En `dialogue.kdr`, cada bloque tiene un `id` y un `text`. Usá el mismo
identificador al referenciarlo desde el pasajero y verificá que cada
referencia tenga su definición. No supongas que un texto adicional se mostrará
si no está conectado a una interacción.

Para una misión, compará [quests.kdr del ejemplo](./3810584840/quests.kdr)
con [quests.kdr original](./Keep%20Driving/quests.kdr) y revisá la
[documentación de misiones](https://whycjwhy.com/keepdriving/mods/kdr/quests/),
que está incompleta. Si un pasajero referencia una misión, comprobá que esta
exista y que sus diálogos y objetivos estén correctamente vinculados.

## 5. Imágenes

### Formato y ubicación

- Guardá los archivos en la carpeta `images/` del mod. El juego admite PNG,
  JPG y JPEG; para personajes conviene **PNG con canal alfa y fondo
  transparente**, para que no aparezca un rectángulo alrededor.
- Usá imágenes pequeñas de estilo pixel art, sin escenarios dibujados detrás
  del personaje. Preparar cada sprite para su uso es mejor que simplemente
  reducir una ilustración completa.
- Las medidas siguientes se tomaron del
  [personaje del mod de ejemplo](./3810584840/hitchhikers.kdr) y sus
  [imágenes](./3810584840/images/). Son **referencias**, no medidas
  oficiales obligatorias para todos los personajes. Comprobá posición,
  escala y apariencia en el juego.

### Lista de sprites del personaje

En los nombres de ejemplo, reemplazá `nombre` por el identificador de tu
personaje. Las medidas son **ancho × alto en píxeles por cuadro**.
Los nombres son una convención; deben coincidir con las referencias KDR.

| Campo en `hitchhikers.kdr` | Archivo de ejemplo | Uso | Medida por cuadro |
| --- | --- | --- | --- |
| `sprite_portrait` | `spr_nombre_portrait.png` | Retrato del pasajero. | 40 × 40 |
| `sprite_rw_big` | `spr_nombre_rw_big.png` | Retrato grande. | 64 × 64 |
| `sprite_rw_small` | `spr_nombre_rw_small.png` | Retrato pequeño. | 42 × 42 |
| `sprite_menu_portrait` | `spr_nombre_portrait_hitchermenu.png` | Retrato del menú de encuentro. | 71 × 71 |
| `sleep_sprite_full` | `spr_nombre_portrait_full_sleep.png` | Retrato grande durmiendo. | 64 × 64 |
| `sleep_sprite_small` | `spr_nombre_portrait_small_sleep.png` | Retrato pequeño durmiendo. | 42 × 42 |
| `sprite_button` | `spr_nombre_button_strip2.png` | Botón/icono del pasajero. | 12 × 12 |
| `sprite_idle` | `spr_nombre_idle_strip8.png` | Personaje en reposo. | 39 × 64 |
| `sprite_hitch` | `spr_nombre_hitch_strip6.png` | Personaje haciendo dedo. | 39 × 64; Paula: 6 cuadros, 234 × 64 total. |
| `sprite_car` | `spr_nombre_incar_strip3.png` | Personaje dentro del auto. | 156 × 64 |
| `sprite_kicked` | `spr_nombre_sitting_strip7.png` | Personaje sentado al bajarse. | 47 × 36 |

El ejemplo incluye también un segundo `sprite_idle`, con 24 cuadros de
39 × 64; es una variante de reposo, no necesariamente otro sprite para todos
los personajes. Algunas propiedades tienen sprites predeterminados del juego:
no todas exigen un PNG personalizado. Si se referencia uno propio, debe
existir y ser apropiado para ese uso.

**En el auto:** los 156 × 64 son el lienzo completo de cada cuadro, no el
tamaño del cuerpo. El personaje ocupa una zona pequeña y debe alinearse
con el asiento. El ejemplo usa `xoffset: 75` e `yoffset: 58` para ese
sprite; conviene comprobarlo con el auto y no copiar esos valores a todos
los retratos o botones.

### Iconos de habilidades y pasivas

Cada habilidad activa personalizada puede referenciar su imagen mediante
`sprite` en `skills.kdr`. Los tres iconos activos del mod de ejemplo miden
**32 × 38** cada uno. Podés usar un sprite existente del juego en vez de
crear uno propio; verificá que la referencia sea válida.

Si creás pasivas personalizadas, revisá también su campo de imagen y el
formato específico que necesiten. No se verificó aquí una medida universal
para sus iconos ni cómo implementar sus efectos.

### Tiras de animación

Una tira `_stripN` contiene `N` cuadros **horizontales, del mismo tamaño**.
Por ejemplo, `spr_nombre_idle_strip8.png` contiene ocho cuadros de 39 × 64:
el PNG completo mide **312 × 64**. En KDR se escribe el nombre base, sin
extensión ni sufijo:

```kdr
sprite_idle: spr_nombre_idle
```

En el ejemplo, el botón tiene 2 cuadros (24 × 12 en total), el dedo 6
(234 × 64), el auto 3 (468 × 64) y sentado 7 (329 × 36). Son ejemplos,
no una cantidad de cuadros obligatoria. No alcanza con renombrar una imagen
estática: debe contener realmente los cuadros indicados.

Evitá tener simultáneamente `spr_nombre_idle.png` y
`spr_nombre_idle_strip8.png` como archivos activos: ambos se referencian como
`spr_nombre_idle` y podrían entrar en conflicto. El
[formato oficial de imágenes](https://whycjwhy.com/keepdriving/mods/modding-api/adding-images-and-sounds/)
explica cómo se elimina el sufijo al cargar.

### Configuración y comprobación

`images.kdr` configura propiedades de imágenes que ya existen: **no crea el
PNG**. Permite, por ejemplo, indicar `speed_type` y `speed` para animaciones,
o ajustar `xoffset` e `yoffset`. Ver la
[documentación de imágenes](https://whycjwhy.com/keepdriving/mods/kdr/images/).

Antes de probar, verificá el archivo o sprite existente de cada referencia,
sus dimensiones, transparencia, número de cuadros y ausencia de nombres
duplicados. Un sprite faltante puede provocar un error con `sprite -1`.
Después verificá en el juego la apariencia y cada situación en la que se
utiliza.

## 6. Validar y probar

Antes de probar, verificá que cada referencia a habilidad, pasiva, diálogo,
misión y sprite exista. Compará la estructura con el juego original y con el
mod de ejemplo. Los mods locales se cargan automáticamente; probá un viaje: la aparición del
pasajero, sus diálogos, imágenes y progresión. La ausencia inmediata de un
pasajero no demuestra por sí sola que el mod haya fallado.

Si algo falla, anotá el momento, el mensaje de error y los mods activos.
Ver [Referencias y pruebas](./REFERENCIA_Y_PRUEBAS.md).
