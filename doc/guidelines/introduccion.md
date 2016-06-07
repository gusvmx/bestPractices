# Introducción.

Este documento define los lineamientos de codificación para los desarrollos internos y externos de GNP. Los lineamientos establecidos en este documento están basados en mejores prácticas de código limpio que funcionan en proyectos reales definidos por autores con una gran experiencia que nos ayudarán a realizar un mejor trabajo profesional.

## ¿Por qué código limpio?
El código está limpio si es que puede ser entendido fácilmente, por todos y cada uno de los integrantes del equipo. Tener un buen entendimiento trae consigo más beneficios como la legibilidad, mejoras en el mantenimiento y en la extensibilidad o la aplicación de cambios.

Todas las cosas necesarias para mantener un proyecto corriendo durante mucho tiempo sin acumular una gran cantidad de deuda técnica. 

### Teniendo código limpio, los bugs no se pueden esconder.
La mayoría de los defectos de software son introducidos durante el mantenimiento del sistema, al modificar o cambiar el código existente. La razón detrás de esto es que el ingeniero de software al cambiar el código no puede entender completamente las implicaciones de los cambios realizados al código. Si bien es cierto que dependiendo del cambio y los requerimientos puedan derivar en efectos secundarios, el código limpio ayuda a minimizar el riesgo de introducir defectos al hacer que el código sea tan fácil de entender como sea posible.

## Organización del documento.
Este documento se encuentra organizado de la siguiente manera: La sección 2 presenta los lineamientos, las buenas prácticas a seguir y las malas prácticas a evitar. La sección 3 define algunos tipos de prueba recomendados a seguir así como lineamientos o reglas generales para la codificación de las pruebas del sistema. Posteriormente, la sección 4 muestra un enfoque para la migración o refactorización de código legado hacía código limpio. La sección 5 muestra técnicas para aprender a realizar código limpio con el fin de hacer del código limpio un hábito. Finalmente la sección 6 muestra algunas de las métricas que se estarán utilizando junto con herramientas para establecer un punto de referencia. Si bien es importante medir en cierta forma el código para poder determinar si este está limpio o no, es una obligación remarcar que las métricas no deben ser tomadas dogmáticamente, es decir, habrán ocasiones en las que las métricas establecidas no podrán ser cumplidas por alguna razón muy particular.
