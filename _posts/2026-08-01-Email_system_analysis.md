---
title: "Email system analysis"
published: false
layout: single
tags: [mvp, tech, system design]
---

# ¿Por que?

Ineficiencia, el sistema previo era simple, y aunque esto me permitio sacar una V1 bastante rápido, al trabajr sobre ella y testearla me he dado cuenta de un par de cosas:

- El cliente no va a esperar a que tu endpoint devuelva, era simple, pero a veces los correos tardaban en llegar.
- Ineficiencia y desperdicio de recursos, cada vez los recursos informaticos son más caros, y escalar es algo cada vez más importante.

# ¿Cual era el sistema previo?

Antes de hacer un cambio es critico entender que había antes, el proceso previo era sencillo:

1. uno de los endpoints pedia enviar un email.
2. la función Send, iniciaba una conexión con el SMTP, y enviaba el mensaje.
3. Se cerraba la conexión y mensaje enviado

Obviando los puntos de fallo, este era el sistema previo.

Cuales son los problemas:

1. Añades un delay a cada email enviado, tienes que esperar a que se conecten y luego ya si eso envias, y la gente es muy impaciente.
2. Varios usuarios a la vez solo acerban este problema.

Esto funcionaba bien, para unos 20 emails/min, pero vamos a ser sinceros, esto es una estimación muy generosa.

# ¿Y el nuevo sistema?

Se trata de un Worker + Manager, donde una subrutina gestiona cuantos trabajadores deberían existir y gestionar los emails.

Cada trabajador mantiene una conexión abierta, o la reabre si se ciera, por lo que evitas esa perdida de tiempo hasta que te conectes.

El Gestor mantiene un volumen de trabajadores acorde a la carga de trabajo, ahorrando recursos y adaptandose a la necesidad.

# POV de un email

![](/assets/images/Email life cycle.svg)
 <img src="/assets/images/Worker & Manager.svg" alt="">

Como ves el mensaje entra en un buffer de mensajes pendientes, tras eso un trabajador lo recoje y trata de enviarlo, en caso de fallar lo envia a una 
segunda cola de reintentos, donde otro trabajador lo recoje y lo vuelve a intentar, si vuelve a fallar, fuera, asumimos que el mensaje puede estar mal formateado, se loguea el error pero tratarlo
requeriria uñadir una capa de complejidad que ahora mismo no es necesaria.

# POV del manager

![](/assets/images/Worker & Manager.svg)

En este caso este es el ciclo de vida del manager, este cada cierto tiempo comprueba la cantidad de emails pendientes y el numero de trabajadores, en función de eso, genera un nuevo trabajador o envia un mensaje que mata
a uno de los trabajadores.

# Limites

Aunque esto es una mejora, no implica que ahora puedes enviar 10.000 usuarios, ahora el limite a cambiado a tu provedor de SMTP, si te capan a 500 emails/hora pues eso es lo que puedes a día de hoy.

Otro limite es la capacidad de guardar los datos en una base de datos temporal, si el servicio crashea o se reinicia, adios a los emails en la cola.

Por ultimo, droppear emails es una manera de liberar presión cuando las cosas no están funcionando, pero un sistema más completo tendría un mejor sistema de rescate en caso de error, por ejemplo si el proveedor revienta
cambie de proovedor a tiempo real.

# Conclusion

Como ves, detras de un sistema simple de enviar emails, hay una complejidad enorme, y esto solo se acrecenta según incrementas las capacidades de tu sistema, pero tambien tienes que tener en cuenta las horas que tienes disponibles,
y saber priorizar.


