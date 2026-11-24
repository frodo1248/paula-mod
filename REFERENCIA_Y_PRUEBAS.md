# Referencias y pruebas

## Documentación de este proyecto

- [Paula](./PAULA.md): historia, archivos y pendientes concretos.
- [Guía general](./GUIA_CREAR_PERSONAJES.md): cómo organizar un pasajero.
- Este documento: fuentes y lista para verificar el mod.

## Referencias

- [Documentación oficial de mods](https://whycjwhy.com/keepdriving/mods/)
- [Formato KDR](https://whycjwhy.com/keepdriving/mods/kdr/)
- [Pasajeros](https://whycjwhy.com/keepdriving/mods/kdr/hitch-hikers/)
- [Habilidades](https://whycjwhy.com/keepdriving/mods/kdr/skills/)
- [Diálogos](https://whycjwhy.com/keepdriving/mods/kdr/dialogue/)
- [Misiones](https://whycjwhy.com/keepdriving/mods/kdr/quests/)
- [Imágenes](https://whycjwhy.com/keepdriving/mods/kdr/images/)
- [Guía comunitaria sobre encuentros con pasajeros](https://steamcommunity.com/app/2756920/discussions/0/510700860420924884/):
  describe encuentros generalmente aleatorios; no documenta específicamente
  la selección de pasajeros de mods.

Los [archivos originales aportados](./Keep%20Driving/) y el
[mod de ejemplo](./3810584840/) sirven para comparar estructuras, pero no
sustituyen una prueba de Paula en el juego.

## Lista antes de probar Paula

- [ ] Verificar las parejas de habilidades activas y pasivas asignadas por
      nivel.
- [ ] Comprobar que la misión `paula_lawyer_trip` se inicie al recoger a
      Paula y que los tres objetivos (comida, bebida, destino) se completen.
- [ ] Verificar si llegar al destino antes de conseguir la comida y la bebida
      completa la misión o espera los otros objetivos.
- [ ] Confirmar que `quest_days` en la misión evita el error de índice en
      `scr_quest_add` al levantar a Paula (en pruebas anteriores, con 8, ya
      lo corregía).
- [ ] Verificar el diálogo de cierre, la recompensa de 50 de dinero y 1 punto
      de habilidad, y que el generador de destinos devuelva uno alcanzable.
- [ ] Versión 2: cadena de misiones (ver [Paula](./PAULA.md)); pendiente de
      más documentación oficial. No funcionó la oferta de la segunda misión
      por charla tras completar la primera.
- [ ] Probar los diálogos automáticos de `banter.kdr`, tanto el general como
      los condicionados por lluvia, noche, música y cielo despejado.
- [ ] Probar el objeto `paula_stage_bustier`: icono, 15 usos y patrón `AA`
      (hoy no se entrega por misión).
- [ ] Confirmar si `cons_stat: money` y `cons_amount: 5` otorgan dinero por
      uso en un objeto de combate.
- [ ] Probar los tres iconos activos personalizados indicados en
      [Paula](./PAULA.md), ahora enlazados en `skills.kdr` y presentados en
      tarjetas blancas tipo Polaroid.
- [ ] Investigar cómo personalizar nombre e icono de las pasivas solo para
      Paula; evitar overrides globales de `street_musician` y `loan_sharks`.
- [ ] Probar en el juego `spr_paula_portrait.png` (40 × 40), verificando que
      el asiento se vea a través del fondo crema semitransparente.
- [ ] Probar en el juego `spr_paula_rw_big.png` (64 × 64) y
      `spr_paula_rw_small.png` (42 × 42), verificando que la figura se vea
      más grande, opaca y con el fondo transparente.
- [ ] Probar en el juego `spr_paula_portrait_hitchermenu.png` (71 × 71),
      verificando que el fondo semitransparente deje ver el asiento.
- [ ] Probar en el juego `spr_paula_portrait_full_sleep.png` (64 × 64) y
      `spr_paula_portrait_small_sleep.png` (42 × 42), comprobando que se vean
      nítidos con el personaje opaco y el fondo transparente.
- [ ] Probar en el juego `spr_paula_button_strip2.png` (dos cuadros de
      12 × 12, con el icono rehecho y ampliado a 11 × 12 px).
- [x] Los sprites pequeños permitieron iniciar el juego en pruebas posteriores.
- [x] El retrato de menú y los iconos provisionales permitieron abrir el
      menú de Paula en la última prueba.
- [x] En la prueba más reciente, Paula se vio en el menú, dentro del auto y
      en la ciudad; los iconos se vieron bien de tamaño.
- [x] Se pudieron jugar eventos sin errores en la prueba más reciente. El
      error anterior de `obj_skill_ui` no se reprodujo.
- [x] La animación de reposo se vio bien en la ruta antes del último ajuste.
- [ ] Probar el nuevo tamaño de `spr_paula_idle_strip8.png` (arte reescalado
      dentro de una caja de 35 × 57 px por cuadro, con los pies alineados).
- [x] La animación `spr_paula_hitch_strip6.png` se vio al aparecer Paula y
      funciona correctamente.
- [ ] Verificar la nueva animación `spr_paula_sitting_strip7.png` en la
      situación correspondiente a `sprite_kicked`.
- [x] En la prueba más reciente, Paula se vio proporcionada dentro del auto
      usando el sprite provisional.
- [ ] Probar la tira ajustada `spr_paula_incar_strip3.png` de busto (15 × 21
      px por cuadro); confirmar que el tamaño y la ubicación se vean bien.
- [ ] Comparar con June el difuminado de los retratos en el retrovisor cuando
      no están seleccionados; no modificar la imagen estática por ahora.
- [ ] Al sustituir los sprites provisionales por los finales, comprobar que
      cada referencia de KDR carga el archivo correcto y conserva su tamaño,
      transparencia y alineación.
- [ ] Comprobar las referencias y el comportamiento esperado de habilidades
      y diálogos.
- [ ] Probar Baile distractor como habilidad inicial de nivel 1: que sea
      TURBO (`swift: 1`), no tenga costo, otorgue 4 unidades de dinero y
      tenga 3 usos.
- [ ] Probar Seducción al llegar al nivel 2: que cueste 1 energía, elimine
      dos iconos de energía (`EE`) y tenga 3 usos.
- [ ] Comprobar el costo, patrón, efecto y recarga de Búho nocturno.
- [ ] Confirmar que los usos se recargan al dormir y que Instinto de
      supervivencia otorga 1 armadura al huir.

## Prueba en el juego

Cuando los pendientes estén resueltos:

1. Iniciar Keep Driving; los mods locales se cargan automáticamente.
2. Iniciar una partida y buscar a Paula como pasajera durante el viaje; no
   buscarla como conductora seleccionable.
3. Si aparece, verificar el encuentro, los diálogos y los sprites en las
   distintas situaciones.
4. Comprobar las habilidades activas, las pasivas y su progresión por nivel,
   así como las misiones vinculadas.
5. Si algo falla, guardar el mensaje o captura, anotar qué ocurrió y revisar
   si otros mods están activos.

Los encuentros pueden depender del azar; no verla enseguida no confirma por
sí solo un fallo. La integración y el balance todavía no están verificados.
