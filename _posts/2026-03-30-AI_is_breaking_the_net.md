---
title: "La IA está rompiendo internet (y nadie quiere admitirlo)"
published: true
layout: single
tags: [AI, software]
---

Soy pro IA. Lo digo antes de empezar porque si no, esto parece un artículo de alguien que 
le tiene miedo al futuro. No es el caso. Uso IA a diario, me parece una herramienta 
increible. Pero lo que estamos viendo en los últimos años en las empresas roza la 
irresponsabilidad manifiesta.

Vamos a los datos.

## El código que genera la IA llega roto a producción

CodeRabbit analizó 470 pull requests, 320 por IA, 150 por humanos, y publicó sus conclusiones. El código generado por IA tiene **1,7 veces más errores** que el escrito por 
humanos, y sus errores son más graves que los creados por huamanos. Los fallos de lógica de negocio fueron un 75% más frecuentes. La claridad del código, 3 veces peor.

No es que la IA sea mala escribiendo código. Es que escribe código que *parece* correcto, 
pasa los tests básicos, y explota tres semanas después en producción cuando nadie recuerda 
de dónde salió.


## Y encima es código que no se puede mantener

OX Security fue un paso más allá. Su estudio muestra que la IA tiende a generar código 
ad-hoc: soluciona el problema inmediato, pero evita crear abstracciones reutilizables. El resultado es una codebase que crece en superficie, (y por tanto en superficie de error) sin crecer en estructura.

Si alguna vez has tenido que hacer code review de código generado por IA, sabes de qué 
hablo. Funciona, más o menos, pero apenas puedes tocar mucho o la magia se disipa. Y eso es un problema.


## El caso más ilustrativo: AWS se rompió a sí mismo

Este es el que más me llama la atención. AWS, la empresa que literalmente sostiene buena 
parte de internet, implementó un sistema de IA que desplegaba cambios a producción sin 
revisión humana. El resultado: caídas constantes a lo largo del último año.

La infraestructura que usan millones de servicios web fue degradada 
por la propia automatización de IA de su propietario. Una empresa tan convencida de su propia tecnología que elimina hasta el último checkpoint.


## El resultado: internet se rompe más

El 84% de los negocios han reportado un incremento en cortes de servicio, según el último informe de CodeRabbit.

Esta es la nueva tendencia, mucho código "barato" que se transforma rápidamente en pérdidas millonarias.

## ¿Y qué hacemos con esto?

La Solución es simple, añade fricción, crear software de calidad no se hace con 4 prompts y un padre nuestro.

Toca mancharse las manos, toca pelear linea a linea, con ayuda de herramientas que te ayuden a entender antes, que te generen boilerplate, pero si quieres saber porque una instrucción tiene que existir, necesitas ser tú el que maneje el dilema de crearla o no.

La fricción es buena, es un indicador de señal, de que algo no está bien, no desmontes los frenos del coche porque tu solo quieres acelerar, aprende a saber cuándo y cómo frenar, porque eso indica que algo necesita cambiar.

## Fuentes.

1. [CodeRabbit](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report)
2. [Ox-report](https://www.prnewswire.com/news-releases/ox-report-ai-generated-code-violates-engineering-best-practices-undermining-software-security-at-scale-302592642.html)
3. [AWS](https://medium.com/@Reiki32/aws-ai-outages-explained-when-the-clouds-own-ai-broke-the-cloud-426c0789c470)
4. [CodeRabbit](https://www.coderabbit.ai/blog/why-2025-was-the-year-the-internet-kept-breaking-studies-show-increased-incidents-due-to-ai)


