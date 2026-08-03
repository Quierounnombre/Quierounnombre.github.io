---
title: "Email system analysis"
published: true
layout: single
tags: [mvp, tech, system design]
---

Cambie mi sistema de enviar emails de mi plantilla, he aquí el porque, como, y sus nuevos limites.

# ¿Por qué?


Ineficiencia, el sistema previo era simple, y aunque esto me permitió sacar una V1 bastante rápido, al trabajar sobre ella y testear me he dado cuenta de un par de cosas:


- El cliente no va a esperar a que tu endpoint devuelva, era simple, pero a veces los correos tardaban en llegar.
- Ineficiencia y desperdicio de recursos, cada vez los recursos informáticos son más caros, y escalar es algo cada vez más importante.


# ¿Cuál era el sistema previo?


Antes de hacer un cambio es crítico entender que había antes, el proceso previo era sencillo:


1. uno de los endpoints pedía enviar un email.
2. la función Send, iniciaba una conexión con el SMTP, y enviaba el mensaje.
3. Se cerraba la conexión y mensaje enviado


Obviando los puntos de fallo, este era el sistema previo.


Cuales son los problemas:


1. Añades un delay a cada email enviado, tienes que esperar a que se conecten al proveedor y luego ya si eso envias, A día de hoy la gente es muy impaciente.
2. Varios usuarios a la vez solo acerba este problema.


Esto funcionaba bien, para unos 20 emails/min, pero vamos a ser sinceros, esto es una estimación muy generosa, ya que depende enormemente del tiempo de conexión al proveedor.


# ¿Y el nuevo sistema?


Se trata de un Worker + Manager, donde una subrutina gestiona cuántos trabajadores deberían existir en función del volumen de mensajes a enviar.


Cada trabajador mantiene una conexión abierta, o la reabre si se cierra, por lo que evitas esa pérdida de tiempo hasta que te conectes.


El Gestor mantiene un volumen de trabajadores acorde a la carga de trabajo, ahorrando recursos y adaptándose a la necesidad.


# POV de un email


![](/assets/images/Email life cycle.svg)


Cómo ves el mensaje entra en un buffer de mensajes pendientes, tras eso un trabajador lo recoge y trata de enviarlo, en caso de fallar lo vuelve a intentar con 
segunda cola de reintentos, donde otro trabajador lo recoge y lo vuelve a intentar, si vuelve a fallar, fuera, asumimos que el mensaje puede estar mal formateado, se loguea el error pero tratarlo
requeriría añadir una capa de complejidad que ahora mismo no es necesaria.


Esto puede ser molesto para algunos clientes a los que no les llegue el mensaje, pero si al tratar de enviar 2 dos veces un email este no se puede, es muy probable que este mal declarado, o que el proveedor este
roto, nada que realmente podamos hacer sin complicar mucho este sistema.


# POV del manager


![](/assets/images/Worker & Manager.svg)


En este caso este es el ciclo de vida del manager, este cada cierto tiempo comprueba la cantidad de emails pendientes y el número de trabajadores, en función de eso, genera un nuevo trabajador o envía un mensaje que mata
a uno de los trabajadores.


# Límites


Aunque esto es una mejora, no implica que ahora puedes enviar 10.000 usuarios, ahora el limite a cambiado a tu proveedor de SMTP, si te capan a 500 emails/hora pues eso es lo que puedes a día de hoy.


Otro límite es la capacidad de guardar los datos en una base de datos temporal, si el servicio crashea o se reinicia, adiós a los emails en la cola.


Por último, dropear emails es una manera de liberar presión cuando las cosas no están funcionando, pero un sistema más completo tendría un mejor sistema de rescate en caso de error, por ejemplo si el proveedor se rompe, que cambie de proveedor a tiempo real.


# Conclusión


Como ves, detrás de un sistema aparentemente simple como es enviar emails, hay una complejidad enorme, y esto solo se acrecenta según incrementas las capacidades de tu sistema, pero también tienes que tener en cuenta las horas que tienes disponibles, y saber que priorizar.

Por mi parte se va a quedar así, al menos un tiempo, hasta que vuelva a notar que es uno de mis cuellos de botella. A día de hoy el cuello de botella es mi proveedor de emails, ya que haciendo stress test he llegado en un santiamén al límite de emails.

[Y oye, si estás trabajando en un proyecto similar de Auth, y quieres una auditoría de tus sistemas puedes escribirme](https://www.linkedin.com/in/vicente-garcia-andrade/?locale=en)
