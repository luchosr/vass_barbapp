# Como crear una aplicación de recuento de votos en Hyperledger Fabric usando IBM Blockchain Platform

> **NOTA**: este patron de aplicación crea una red blockchain en *IBM Blockchain Platform version **2.5*** usando *Hyperledger Fabric version **1.4***.

Nuestro objetivo es crear una aplicación web como alternativa al recuendo manual de votos, en la que el votante pueda registrarse con su licencia de conducir, obtener un voterId único que se utiliza para iniciar sesión en la aplicación y emitir su voto. El voto se contabiliza en la una de bloques y la aplicación web muestra la clasificación actual de las encuestas.

<br>
### Votación mediante infraestructura de clave pública.

Al inicio de la aplicación, el usuario se registra para votar proporcionando su número de licencia de conducir (o número de ID, 9 dígitos), distrito de registro y nombre y apellido. En este paso, podemos comprobar si la licencia de conducir es válida y no ha sido registrada previamente. Si todo va bien, creamos una clave pública y privada para el votante con nuestra autoridad de certificación que se ejecuta en la nube y agregamos esas claves a la billetera. Para leer más sobre la infraestructura de clave pública y cómo Hyperledger Fabric implementa la identidad con esta tecnología, vaya [aquí](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html).


Después de eso, usamos nuestro número de licencia de conducir para enviar nuestro voto, durante el cual la aplicación verifica si este número de licencia de conducir ha votado antes y le dice al usuario que ya ha enviado un voto si es así. Si todo va bien, se vota la opción elegida por el votante y se actualiza el estado global. Luego, la aplicación actualiza nuestra clasificación actual de las votaciones para mostrar cuántos votos tiene cada opción en tiempo real.

Dado que cada transacción que se envía al servicio de pedidos debe tener una firma de un par de claves pública-privada válida, podemos rastrear cada transacción hasta un votante registrado de la aplicación, en el caso de una auditoría.

En conclusión, aunque esta es una aplicación simple, el desarrollador puede ver cómo pueden implementar una aplicación web Hyperledger Fabric para disminuir la posibilidad de intromisión electoral y mejorar una aplicación de votación mediante el uso de la tecnología blockchain.Cuando el lector haya completado este patrón de código, comprenderá cómo:

* Crear, compilar y utilizar el servicio IBM Blockchain Platform.
* Crear un back-end de blockchain utilizando la API de Hyperledger Fabric
* Crear y usar un clúster de Kubernetes (gratuito) para implementar y monitorear nuestros nodos de Hyperledger Fabric.
* Implementar una aplicación Node.js que interactuará con nuestro contrato inteligente instanciado.
<br>


#Diagrama de flujo:
<br>
<p align="center">
  <img src="docs/app-architecture.png">
</p>
<br>

#Descripción de flujo:
1. El operador de blockchain configura el servicio IBM Blockchain Platform 2.5.
2. IBM Blockchain Platform 2.5 crea una red Hyperledger Fabric en un servicio IBM Kubernetes, y el operador instala y crea una instancia del contrato inteligente en la red.
3. El servidor de aplicaciones Node.js utiliza Fabric SDK para interactuar con la red desplegada en IBM Blockchain Platform 2.5 y crea API para un cliente web.
4. El cliente Vue.js utiliza la API de la aplicación Node.js para interactuar con la red.
5. El usuario interactúa con la interfaz web de Vue.js para emitir su voto y consultar el estado global para ver la clasificación actual de las encuestas.


