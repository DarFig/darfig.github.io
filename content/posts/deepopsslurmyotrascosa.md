---
title: "Deepops, slurm y otras cosas de herramientas de despliegue comunitarias"
date: 2021-01-17T11:00:29+01:00
draft: false
tags: "nvidia, linux, deepops, dgx, slurm"
---

Voy a contar el resumen de lo que he pasado los últimos días para desplegar [slurm](https://slurm.schedmd.com/documentation.html), utilizando la herramienta [deepops](https://github.com/NVIDIA/deepops). Pero lo voy a utilizar sólo como un ejemplo para hablar de los problemas que puedes encontrarte con otras herramientas similares.

<!--more-->

## El contexto

En resumen **slurm** es una herramienta para gestionar cargas de trabajo, basado en colas, por ejemplo en un clúster  de súper computación.  Que a su vez se ha incluido en la herramienta **deepops**, una serie de playbooks de [ansible](https://www.ansible.com/) y scripts pensados para ayudar en la configuración y despliegue de software en los servidores, dgx de nvidia, y similares. Esto es una gran idea, tienes un servidor y una serie de herramientas que te ayudan a desplegar software siguiendo lo que podríamos llamar “buenas prácticas”.

Así que me dispuse a utilizar **deepops** 20.11 para desplegar **slurm**, siguiendo las recomendaciones de la [documentación](https://github.com/NVIDIA/deepops/tree/master/docs/slurm-cluster) .

## Las sorpresas

Las primeras sorpresas que me encontré fueron enlaces rotos, y poca documentación sobre las variables que debía configurar si decidía modificar un poco el “guión” que viene pre-establecido.  Claro que si me leo los *playbooks* me voy a enterar de lo que hace cada tarea, pero se supone que el objetivo no es que yo haga ingeniería inversa, sino que  me lea una documentación de como utilizar la herramienta. 

Creo que aquí mi confusión es por venir enlazado desde la documentación de nvidia y ver que el repositorio estaba bajo el perfil de nvidia, cuando la herramienta claramente la mantiene la comunidad. A la que sin duda hay que estarle muy agradecidos porque hace un gran trabajo. Este tipo de situaciones se hacen cada vez más comunes. Es normal que si llegas a un repositorio, mantenido por la comunidad, en el cual hay mucho movimiento y se lanzan mejoras cada poco tiempo, la documentación no esté al día. Nos toca poner un poco de nuestra parte y respetar el trabajo que hacen, muchas veces de manera voluntaria, esas personas. Incluso si lo vemos claro podemos poner de nuestra parte actualizar la versión y enviar un *pull request*.

Una vez entendido esto, voy  a comentar errores comunes que podemos encontrarnos con este tipo de herramientas y que pueden darnos algún quebradero de cabeza. 


## Errores

Tras una serie de dudas, problemas y correos con el soporte, tenía todo listo para usar *deepops*. Tenía  la instalación del sistema operativo que recomendaba el fabricante y la configuración que general que había recomendado el socio distribuidor. Hice algunos cambios de rigor, y otras manías que se le crean a uno cuando lleva un tiempo en esto de los servidores y sus problemas. Me lancé al ataque.


### Al ataque

Configuré las variables tal como se recomendaba, me leí el resto por si las moscas, y desactivé un par de cosas que fuera no se mencionaban, pero que algún alma caritativa había dejado en los comentarios por si te pasabas. Cierto es que no afectaban al funcionamiento, pero eran opcionales y no lo quería.

Me lancé a la instalación, y tras ejecutarse las primeras tareas vino el primer error. Una variable que no existía o no tenía valor. Todos sabemos como va esto, buscas información, te vas a la documentación (que no dice nada de esa variable). Entonces aumentas el modo *verboso* al máximo *-vvv* y te dispones a buscar en que fichero falla. En algunas ocasiones esto no es suficiente, pero insistes y finalmente encuentras una pista y crees que has dado con el fichero en el que se define. En mi caso así lo encontramos, o eso creí, no se mencionaba en ningún otro sitio y parecía que era efectivamente una ejecución para dar valor a la variable y utilizarla, todo en el mismo fichero. Pasamos a buscar una solución, y para no hacerlo largo el error no estaba en este fichero &#128517; .
  
La causa original del error es más común de lo que parece, **el idioma del sistema operativo**. Cuando una comunidad crea una herramienta como esta, normalmente lo hace sobre instalaciones de sistemas en inglés. Pero en ningún sitio han puesto los *requisitos del sistema*, ni han mencionado que el idioma sea relevante. Leyendo entre los fichero, descubrí una serie de scripts que obtienen información de comandos como *lscpu*, *free -m*, etc, utilizando *grep*. El caso es que estos comandos muestran salidas traducidas cuando tu sistema operativo no está en inglés. El resultado es que *grep* no devuelve nada, no se obtiene la información y por tanto las variables no tienen valor, provocando luego un error en tiempo de ejecución.

### Ya está todo instalado, pero no en funcionamiento

Una vez corriges tu problema, que gracias al código abierto puedes tocar y arreglar por ti mismo, piensas “o que bien todo funciona y es muy fácil de usar”, pero no. Siempre siempre siempre, que se despliega un entorno hay que hacer pruebas. Las herramientas, como *deepops* suelen tener algunas pruebas  automáticas, que lanzamos y nos devuelven resultados, gráficos, etc. Pero confiarte es un error, porque nunca son suficientes. La comunidad no ha pensado en todos los casos, y menos si han añadido versiones nuevas que a su vez usan versiones nuevas de otros programas.

En este caso estoy usando *slurm*, para lanzar tareas, pero por debajo también está *docker*, etc. Menos de 5 minutos después de terminar el despliegue y las pruebas automáticas, se  le dejó a los usuarios *beta* probar un uso cotidiano, y inmediatamente descubrieron el primer bug en el que yo no hubiera pensado. 

En casos como este siempre viene bien mirar las *issues* del repositorio, y efectivamente ahí estaba [issues](https://github.com/NVIDIA/deepops/issues/783). El dns interno de *docker* se va de fiesta, y luego tus contenedores no tienen conexión con ningún sitio, pero tampoco puedes construir imágenes en local y añadir nuevo software.  Pongo el error, en el que de paso el usuario ha dado una “solución” temporal, por así decirlo, a la vez que lo notifica, y lo pongo porque es un error que puede ocurrir con muchas otras herramientas que usan *docker* por debajo. Vale la pena recordar que cuando desplegamos cualquier entorno de contenedores hay que probar la conectividad y la resolución de nombres, tanto en la red interna como con el exterior.

## En resumen

Creo que si hay algo que aprender de esto es que cuando recurrimos a herramientas mantenidas de manera desinteresadas, vamos a tener que poner de nuestra parte un poquito más. En cambio obtendremos también las facilidades y el conocimiento para resolvernos nuestros propios problemas sobre la marcha o incluso ayudar a otros.

Cuando hay poca documentación, toca trabajar un poco más, y hacer más ensayos, pero tal vez descubramos algo útil que probablemente tampoco estaría en la documentación.

Finalmente, hay que recordar que las pruebas automáticas están muy bien, pero como cada entorno es diferente y complejo. Siempre tendremos que probar por nuestra parte, además si cuentas con usuarios que comúnmente utilicen entornos similares hay que aprovecharlos y dejarlos probar. Nada como la experiencia para saber si algo no está funcionando como debería.


Saludos
[DarFig](https://github.com/DarFig)