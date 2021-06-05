---
title: "Tiling Windows Managers"
date: 2021-06-04T22:49:25+02:00
draft: false
tags: "linux, i3, qtile, leftwm, tiling window manager, tiling wm"
---

Llevo un tiempo probando diferentes "tiling windows manager", principalmente porque en el portátil se me hace incómodo depender del ratón para muchas cosas. Durante este proceso he tenido la oportunidad de probar y modificar varios. Durante este proceso he llegado a algunas conclusiones.

<!--more-->

## tiling

Los "tiling windows manager" o gestores de mosaico, u otros nombres similares que se le dan en español, tienen una gran *fan-base* entre los usuarios *hardcore* del teclado o la gente que hace ascos al ratón xD. Ciertamente una vez los dominas se puede llegan a un nivel de eficiencia verdaderamente increíble.

Su principal ventaja es que lo puedes hacer todo con el teclado, y sí siempre se puede "hacer todo" con el teclado, pero en este caso de una forma más fácil, intuitiva y personalizada. Por así decirlo es lo mismo que utilizar vim, o los atajos de vim en vscode. Sabes que todos los editores tienen atajos, pero nada que ver los atajos de eclipse con los de vim xD.

## Complejidad

Por más que muchos usuarios "hardcore" intenten convencernos de que son complejos, para gente dura de linux, y usuarios avanzados. Realmente cualquiera puede manejarse en ellos con un par de atajos de teclado, porque te haces a ellos en muy poco tiempo, mucho más rápido y fácil de recordar que los atajos de vim por ejemplo.

La verdadera complejidad de estos es como la que se encontraba un usuario de windows a  principios de los 2000 si le decías que se instalara un linux; o la que se puede encontrar cualquier novato si le sueltas a instalar gentoo, arch o similares (salvando las distancias).

Diría que es complejo casi a posta, porque no hay paquetes conjuntos para instalar todo lo que necesitas de una vez. Sí, tienes tu "windows manager", pero te falta el "compositor", la barra, controladores, una aplicación para los fondos, etc. Y no, en muchas ocasiones no se molestan en documentar que más necesitaremos, así que al final siempre nos falta algo.

Esto además va en capas, porque necesitas una barra, pero cuando te la instalas descubres que te faltan los componentes. Si ya se que es para personalizarlo, pero perdone todo el mundo usa el mismo componente para el volumen, para la lan, la wifi, etc (o uno similar, no hay mucho donde escoger). ¿Por qué no están como dependencias? ¿O con una configuración básica? Luego yo ya escogeré si los uso o no en mi barra.

Pueden haber diferentes opiniones sobre esto, pero creo que lo he comprobado perfectamente al instalar distribuciones comunitarias como la de "manjaro i3", que ya viene de base con una configuración básica perfectamente funcional. Que nos permite poder seguir trabajando mientras, en tiempos libres, podemos retocar y personalizar.

Porque sí, creo que ese es uno de los problemas principales, dices que guay voy a instalar X, y descubres que estarás un buen tiempo configurando todo antes de poder seguir trabajando. No todo el mundo puede parar a hacerse su propio linux from scratch y permitirse no usar el pc mientras. Por eso la mayoría de la gente se pone una distribución y luego la modifica poco a poco.

Así que sí, no creo que sean difíciles de utilizar, ni hechos para usuarios avanzados (en el día a día ni siquiera necesito otra cosa que no sea una consola porque me paso el día entre servidores). Creo que actualmente la mayoría están orientados a usuarios con conocimientos, pero sobre todo tiempo libre. Hasta tal punto que esos mismos usuarios deciden que como no les gusta el lenguaje con el que se hace (y tienen mucho tiempo), crean una copia con la misma funcionalidad pero utilizando lenguajes diferentes, como haskell, rust, python (personalmente lo siento, he utilizado muchísimo python, pero creo que sólo deberían hacerse este tipo de componentes de un sistema en rust, c, y c++, todo lo demás es realmente ineficiente).

## Pruebas

Gracias a que en linux puedes tener varios "windows manager", y que como ya comenté el resto de componentes se suelen compartir (picom, comptom, feh, nitrogen, etc) puedes tener varios instalados probarlos y escoger. Mi formula favorita es tener gnome o kde, y además un entorno "tiling" para poder parar a medias si tienes que seguir trabajando y tu "nuevo" entorno está incompleto.

Durante los últimos meses he probado unos cuantos, entre ellos: qtile, leftwm, i3wm, bspwm, etc. Personalmente me quedaría con estos a largo plazo, y principalmente con i3wm y bspwm ahora mismo. Creo que son los más "maduros", con una comunidad más amplia (lo cual es muy importante) y mejor estabilidad.

Estuve utilizando leftwm por unas semanas pero nunca conseguía estar satisfecho con mi propia configuración y cada vez me daba más pereza arreglarlo, que consistía en deshacer toda mi configuración y empezar desde cero. Me gusta mucho que esté hecho en Rust, pero aun es muy "joven", versión 0.2.7 en este momento, he incluso la probé en una anterior. Creo que tiene mucho futuro pero ahora mismo no está listo.

El caso de qtile: es también muy joven v0.17, y me dio algún problema más de inestabilidad. Aunque al principio me llamó la atención que estuviera escrito en python porque eso me simplificaba la personalización luego fue parte del problema. Estaba programando mis cosas, con una documentación que no me terminaba de gustar mucho, y dedicando una cantidad de tiempo sin sentido para hacerlo a mi gusto. Sí, es muy personalizable, pero también es cierto que al final te vas a quedar con lo mismo que conseguirás con cualquier otro. Personalmente no me merecía la pena invertir tiempo en esto.

Para los que quieran dedicar menos tiempo, o no se crean que esto se puede hacer en cualquier "windows manager", hay guías para configurar los atajos y el comportamiento de KDE como un "tiling windows manager". Lo probé, es realmente cómodo, simplifica mucho la curva de entrada, es muy rápido de configurar, pero un poco menos personalizable. A día de hoy lo sigo teniendo configurado en una máquina que utilizo comúnmente para personalizar y testear contenedores.

Se que ahora mismo este es un nicho de entusiastas, como lo podría ser el “tuning” en los coches. Pero es que cuando quieres tener un coche proyecto rara vez escoges uno sin motor, ni ruedas, ni puertas, etc; porque eso es hacerte el coche desde cero no personalizar tu coche. Que no digo que una cosa esté bien y otra mal, sino que comprarte todo por separado y montarlo lleva más tiempo y mientras tanto no tienes coche para ir a trabajar.  


## Conclusión

En general me encanta la idea de los “tiling windows managers”, me gusta mucho trabajar con ellos, y su personalización. Pero creo que sufren de segmentación innecesaria, como ocurre con muchas distribuciones de gnu/linux. También de una barrera de entrada basada en el tiempo que puedas invertir en documentarte, mirar tutoriales, modificar y probar tus configuraciones.

Ya se que existe la posibilidad de copiarte la configuración de otra persona, y lo he ignorado todo este tiempo, porque no considero que se pueda llamar “personalizar” si te copias lo de otro. Además, que muchas veces no es ni buena idea, ni funciona muy bien el copiarse los *dotfiles* de otra persona; ni ejecutar un script random que te encuentres en el repositorio de alguien en github.

Tampoco quiero tener que elegir mi distribución basándome en que exista en ella una versión que ya venga preparada con, por ejemplo, i3wm. 

Creo que deberías tener todas las opciones, al fin y al cabo es la gracia del software libre. Poder tener los componentes por separado está bien, incluso si te los quieres compilar uno a uno está bien. Por otro lado si quieres instalar una distribución con todo ya preparado pues también está bien. Pero si quieres tener la distribución que te de la gana y por otra parte instalarte el “windows manager”que te de la gana, también estaría bien que viniera con una configuración básica funcional un poquito más avanzada en la que no tuvieras que ir instalando uno a uno cada componente básico que necesitas. Así realmente dedicarías más tiempo a “personalizar” y menos a preguntarte como hacer que el componente de control de audio funcione correctamente.

Saludos
[DarFig](https://github.com/DarFig)


