---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: "Iteración #5: crear unidad comprueba (VB) | Documentos de Microsoft"
author: microsoft
description: "En la iteración quinto, hacemos nuestra aplicación más fácil de mantener y modificar mediante la adición de pruebas unitarias. Hemos simular nuestro clases del modelo de datos y generar pruebas unitarias para o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: ab9ff5629cb468b785f5b82178f9f6247a55cacb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="iteration-5--create-unit-tests-vb"></a>Iteración #5: crear pruebas unitarias (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> En la iteración quinto, hacemos nuestra aplicación más fácil de mantener y modificar mediante la adición de pruebas unitarias. Hemos simular nuestro clases del modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de MVC de ASP.NET (VB) de administración de contactos

En esta serie de tutoriales, creamos una aplicación de administración de contactos toda desde el principio para finalizar. La aplicación de ponerse en contacto con el administrador permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Se compile la aplicación en varias iteraciones. Con cada iteración, se mejora gradualmente la aplicación. El objetivo de este enfoque de iteración varias es para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el póngase en contacto con el Administrador de la manera más sencilla posible. Se agrega compatibilidad para las operaciones de base de datos básicos: crear, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación sea bonito. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de vista de MVC de ASP.NET predeterminada y en cascada hoja de estilos.

- Iteración #3: agregar validación del formulario. En la tercera iteración, agregamos la validación del formulario básico. Es evitar que los usuarios enviar un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación de acoplamiento flexible. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que resulten más fáciles de mantener y modificar la aplicación póngase en contacto con el administrador. Por ejemplo, se refactoriza nuestra aplicación para que use el modelo de repositorio y el modelo de inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinto, hacemos nuestra aplicación más fácil de mantener y modificar mediante la adición de pruebas unitarias. Hemos simular nuestro clases del modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración &#6;: Use desarrollo controlado por pruebas. En esta iteración sexto, se agregan nuevas funciones a nuestra aplicación escribiendo pruebas unitarias en primer lugar y escribir código frente a las pruebas unitarias. En esta iteración, agregamos grupos de contactos.

- Iteración #7 - agregar funcionalidad de Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad para Ajax.


## <a name="this-iteration"></a>Esta iteración

En las iteraciones anteriores de la aplicación Administrador de contacto, hemos vuelto a diseñar la aplicación para ser de acoplamiento más flexible. Se separa de la aplicación en el controlador distinto, el servicio y las capas de repositorio. Cada capa interactúa con el nivel por debajo de él a través de interfaces.

Hemos vuelto a diseñar la aplicación para que la aplicación sea más fácil mantener y modificar. Por ejemplo, si es necesario usar una nueva tecnología de acceso a datos, simplemente podemos cambiar la capa de repositorio sin tocar el controlador o el nivel de servicio. Al hacer que el Administrador de contacto de acoplamiento flexible, se ha realizado la aplicación más resistente a cambiar.

Pero, ¿qué ocurre cuando es necesario agregar una nueva característica para la aplicación Administrador de contacto? O bien, ¿qué ocurre cuando se corrige un error? Una verdad sad, pero también comprobada, escritura de código es que cada vez que tocar el código que se creó el riesgo de introducir nuevos errores.

Por ejemplo, un día de un problema, el administrador podría pedirle que agregar una nueva característica para el Administrador de contacto. Desea agregar compatibilidad para grupos de contactos. Que desea permitir a los usuarios organizar sus contactos en grupos como amigos, Business y así sucesivamente.

Para implementar esta nueva característica, debe modificar todos los tres niveles de la aplicación Administrador de contacto. Debe agregar nuevas funcionalidades a los controladores, el nivel de servicio y el repositorio. En cuanto se inicia a modificar el código, corre el riesgo de interrumpir la funcionalidad que antes funcionaba.

Refactorización de nuestra aplicación en niveles independientes, como hicimos en las iteraciones anteriores, era bueno. Bueno, era porque nos permite realizar cambios en capas completas sin tocar el resto de la aplicación. Sin embargo, si desea que sea más fácil de mantener y modificar el código del mismo nivel, debe crear pruebas unitarias para el código.

Se usa una unidad de prueba a una sola unidad de código. Estas unidades de código son más pequeñas que las capas de toda la aplicación. Por lo general, utilice una prueba unitaria para comprobar si un método concreto en el código se comporta de la manera que se espera. Por ejemplo, podría crear una prueba unitaria para el método CreateContact() expuesta la clase ContactManagerService.

Las pruebas unitarias para un trabajo de la aplicación, como una red de seguridad. Cada vez que modifica el código en una aplicación, puede ejecutar un conjunto de pruebas unitarias para comprobar si la modificación interrumpe la funcionalidad existente. Pruebas unitarias que el código seguro modificar. Pruebas unitarias que todo el código de la aplicación más resistente a cambiar.

En esta iteración, se agregan pruebas unitarias a nuestra aplicación póngase en contacto con el administrador. De este modo, en la siguiente iteración, podemos agregar grupos de contactos para nuestra aplicación sin preocuparse por romper la funcionalidad existente.

> [!NOTE] 
> 
> Hay una variedad de marcos como NUnit, xUnit.net y MbUnit de pruebas unitarias. En este tutorial, se utiliza el marco de trabajo que se incluye con Visual Studio de pruebas unitarias. Sin embargo, podría igual de sencillo usar uno de estos marcos alternativos.


## <a name="what-gets-tested"></a>¿Qué Obtiene probado

En el mundo perfecto, todo el código se cubrirán mediante pruebas unitarias. En el mundo perfecto, tendría la red de seguridad perfecta. Podrán modificar cualquier línea de código de la aplicación y sepa al instante, mediante la ejecución de las pruebas unitarias, si el cambio interrumpió la funcionalidad existente.

Sin embargo, no queremos t en directo en un mundo perfecto. En la práctica, al escribir pruebas unitarias, concentrarse en la escritura de pruebas para la lógica de negocios (por ejemplo, la lógica de validación). En concreto, se *no* escribir pruebas unitarias para los datos de acceso lógica o la lógica de vista.

Para que sea útil, pruebas unitarias deben ejecutar muy rápidamente. Fácilmente puede acumular cientos (o incluso miles) de pruebas unitarias para una aplicación. Si las pruebas unitarias tardan mucho tiempo en ejecutarse, a continuación, podrá evitar ejecutarlas. En otras palabras, pruebas unitarias de larga ejecución son útiles para fines de codificación de un día a otro.

Por este motivo, normalmente no escribe pruebas unitarias para código que interactúa con una base de datos. Ejecutan centenares de pruebas unitarias en una base de datos sería demasiado lento. En su lugar, simular la base de datos y escribe código que interactúa con la base de datos ficticio (planteamos simulación de una base de datos a continuación).

Por un motivo similar, normalmente no escribe pruebas unitarias para las vistas. Para probar una vista, debe poner en marcha un servidor web. Dado que poner en marcha un servidor web es un proceso relativamente lento, no se recomienda crear pruebas unitarias para las vistas.

Si la vista contiene lógica compleja, a continuación, puede mover la lógica en los métodos auxiliares. Puede escribir pruebas unitarias para métodos de aplicación auxiliar que se ejecutan sin poner en marcha un servidor web.

> [!NOTE] 
> 
> Al escribir las pruebas para la lógica de acceso a datos o lógica de vista no es una buena idea al escribir pruebas unitarias, estas pruebas pueden ser muy valiosas cuando edificio funcional o integración de pruebas.


> [!NOTE] 
> 
> ASP.NET MVC es el motor de vista Web Forms. Mientras el motor de vista Web Forms depende de un servidor web, no pueden ser otros motores de vista.


## <a name="using-a-mock-object-framework"></a>Con el marco de objeto ficticio

Al generar pruebas unitarias, casi siempre necesario aprovechar las ventajas de un marco de objeto simular. Un marco de objeto simular permite crear simulaciones y códigos auxiliares para las clases en la aplicación.

Por ejemplo, puede usar un marco de objeto simular para generar una versión ficticia de la clase de repositorio. De este modo, puede utilizar la clase de repositorio ficticio en lugar de la clase de repositorio real en las pruebas unitarias. El repositorio ficticio permite evitar la ejecución de código de base de datos al ejecutar una prueba unitaria.

Visual Studio no incluye un marco de objeto simular. Sin embargo, existen varios marcos de trabajo de simular objetos comerciales y de código abierto de .NET framework:

1. Moq - este marco de trabajo está disponible bajo la licencia BSD de código abierto. Puede descargar Moq de [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks; este marco de trabajo está disponible bajo la licencia BSD de código abierto. Puede descargar Rhino Mocks de [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock aislador - se trata de un entorno comercial. Puede descargar una versión de prueba de [http://www.typemock.com/](http://www.typemock.com/).

En este tutorial, se decidió usar Moq. Sin embargo, puede usar fácilmente Rhino Mocks o Typemock aislador para crear el simulacro objetos para la aplicación póngase en contacto con el administrador.

Para poder usar Moq, debe completar los pasos siguientes:

1. .
2. Antes de descomprimir la descarga, asegúrese de que haga clic en el archivo y haga clic en el botón con la etiqueta **Unblock** (consulte la figura 1).
3. Descomprima la descarga.
4. Agregue una referencia al ensamblado Moq al proyecto de prueba, seleccione la opción de menú **proyecto, agregar referencia** para abrir el **Agregar referencia** cuadro de diálogo. En la ficha Examinar, vaya a la carpeta donde se descomprimió Moq y seleccione el ensamblado Moq.dll. Haga clic en el **Aceptar** botón (consulte la figura 2).


[![Moq desbloqueo](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: Moq desbloqueo ([haga clic aquí para ver la imagen a tamaño completo](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Referencias después de agregar Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: referencias después de agregar Moq ([haga clic aquí para ver la imagen a tamaño completo](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Crear pruebas unitarias para el nivel de servicio

Permiten s empiece por crear un conjunto de pruebas unitarias para nuestro nivel de servicio de s de aplicación póngase en contacto con el administrador. Vamos a usar estas pruebas para comprobar la lógica de validación.

Cree una carpeta nueva denominada modelos en el proyecto ContactManager.Tests. A continuación, haga clic en la carpeta Models y seleccione **agregar, nueva prueba**. El **agregar nueva prueba** aparece el cuadro de diálogo se muestra en la figura 3. Seleccione el **de prueba unitaria** plantilla y el nombre de la nueva prueba ContactManagerServiceTest.vb. Haga clic en el **Aceptar** botón para agregar la nueva prueba a su proyecto de prueba.

> [!NOTE] 
> 
> En general, desea que la estructura de carpetas del proyecto de prueba para que coincida con la estructura de carpetas del proyecto ASP.NET MVC. Por ejemplo, coloque las pruebas de controlador en una carpeta de controladores, pruebas de modelo en una carpeta de modelos y así sucesivamente.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([haga clic aquí para ver la imagen a tamaño completo](iteration-5-create-unit-tests-vb/_static/image6.png))


Inicialmente, debe probar el método CreateContact() expone la clase ContactManagerService. Vamos a crear las cinco pruebas siguientes:

- CreateContact() - pruebas que CreateContact() devuelve el valor true cuando un contacto válido se pasa al método.
- CreateContactRequiredFirstName() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con un nombre que faltan se pasa al método CreateContact().
- CreateContactRequredLastName() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto cuyo apellido que faltan se pasa al método CreateContact().
- CreateContactInvalidPhone() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con un número de teléfono no válido se pasa al método CreateContact().
- CreateContactInvalidEmail() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con una dirección de correo electrónico no válido se pasa al método CreateContact()...

La primera prueba comprueba que un contacto válido no genera un error de validación. Las pruebas restantes comprueban cada una de las reglas de validación.

El código de estas pruebas se encuentra en la lista 1.

**Lista 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Se usa la clase de contacto en la lista 1, es preciso agregar una referencia a Microsoft Entity Framework a nuestro proyecto de prueba. Agregue una referencia al ensamblado System.Data.Entity.


Lista 1 contiene un método denominado Initialize() se decora con el atributo [TestInitialize]. Se llama automáticamente a este método antes de ejecuta cada una de las pruebas unitarias (se llama justo antes de cada una de las pruebas unitarias 5 veces). El método Initialize() crea un repositorio ficticio con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Esta línea de código usa el marco de trabajo de Moq para generar un repositorio ficticio de la interfaz IContactManagerRepository. El repositorio ficticio se utiliza en lugar de la EntityContactManagerRepository real para evitar el acceso a la base de datos cuando se ejecuta cada prueba unitaria. El repositorio ficticio implementa los métodos de la interfaz IContactManagerRepository, pero la t don métodos realmente hacer nada.

> [!NOTE] 
> 
> Cuando se utiliza el marco de trabajo Moq, hay una diferencia entre \_mockRepository y \_mockRepository.Object. El primero se refiere a la clase de simulacro (de IContactManagerRepository) que contiene métodos para especificar cómo se comportará el repositorio ficticio. El segundo se refiere al repositorio ficticio real que implementa la interfaz IContactManagerRepository.


Se utiliza el repositorio ficticio en el método Initialize() al crear una instancia de la clase ContactManagerService. Todas las pruebas unitarias individuales utilizan esta instancia de la clase ContactManagerService.

Lista 1 contiene cinco métodos que corresponden a cada una de las pruebas unitarias. Cada uno de estos métodos se decora con el atributo [TestMethod]. Al ejecutar las pruebas unitarias, se llama a cualquier método que tenga este atributo. En otras palabras, cualquier método que se decora con el atributo [TestMethod] es una prueba unitaria.

La primera prueba unitaria, denominada CreateContact(), comprueba que una llamada a CreateContact() devuelve el valor true cuando se pasa una instancia de la clase Contact válida al método. La prueba crea una instancia de la clase Contact, llama al método CreateContact() y se comprueba que CreateContact() devuelve el valor true.

Las pruebas restantes comprueban que cuando se llama al método CreateContact() con un contacto no válido, a continuación, el método devuelve false y se agrega el mensaje de error de validación esperado al estado del modelo. Por ejemplo, la prueba de CreateContactRequiredFirstName() crea una instancia de la clase Contact con una cadena vacía para su propiedad FirstName. A continuación, se llama al método de CreateContact() con el contacto no válido. Por último, la prueba comprueba que CreateContact() devuelve false y el estado del modelo contiene el mensaje de error de validación esperado "nombre es necesario."

Puede ejecutar las pruebas unitarias en el listado 1 seleccionando la opción de menú **prueba, ejecute todas las pruebas de soluciones (CTRL + R, A)**. Los resultados de las pruebas se muestran en la ventana Resultados de pruebas (consulte la figura 4).


[![Resultados de pruebas](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: resultados de pruebas ([haga clic aquí para ver la imagen a tamaño completo](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Crear pruebas unitarias para controladores

Aplicación de ASP.NET MVC controlan el flujo de interacción del usuario. Al probar un controlador, que desea comprobar si el controlador devuelve los datos de resultado y vista de acción correcta. También puede comprobar si un controlador interactúa con las clases de modelo de la manera esperada.

Por ejemplo, el listado 2 contiene dos pruebas unitarias para el controlador de contacto método Create(). La primera prueba unitaria comprueba si un contacto válido se pasa al método Create(), a continuación, el método Create() redirige a la acción del índice. En otras palabras, cuando se pasa un contacto válido, el método Create() debe devolver un RedirectToRouteResult que representa la acción del índice.

No queremos t desea probar el nivel de servicio ContactManager cuando se está probando el nivel de controlador. Por lo tanto, es simular el nivel de servicio con el código siguiente en el método Initialize:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

En la prueba unitaria CreateValidContact(), es simular el comportamiento de la llamada a la capa de servicio CreateContact() método con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Esta línea de código hace que el servicio de ContactManager ficticio devolver el valor true cuando se llama a su método CreateContact(). Por el nivel de servicio de simulación, podemos probar el comportamiento de nuestro controlador sin necesidad de ejecutar cualquier código en el nivel de servicio.

La segunda prueba unitaria comprueba que la acción Create() devuelve la vista de creación cuando un contacto no válido se pasa al método. Se producen el método CreateContact() para devolver el valor false con la siguiente línea de código de nivel de servicio:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Si el método Create() se comporta tal y como se esperaba, a continuación, debe devolver la vista de creación cuando el nivel de servicio devuelve el valor false. De este modo, el controlador puede mostrar los mensajes de error de validación en la vista de creación y el usuario tiene una oportunidad para corregir ese propiedades del contacto no válido.


Si tiene previsto crear pruebas unitarias para los controladores debe devolver nombres de vista explícita de las acciones de controlador. Por ejemplo, no devuelven una vista similar al siguiente:

Devolver View()

En su lugar, devuelve la vista como el siguiente:

Devolver View("Create")

Si no estás explícita cuando se devuelve una vista de la propiedad ViewResult.ViewName devuelve una cadena vacía.


**La lista 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Resumen

En esta iteración, se crean pruebas unitarias para nuestra aplicación póngase en contacto con el administrador. Podemos ejecutar estas pruebas unitarias en cualquier momento para comprobar que nuestra aplicación todavía se comporta de la manera que se espera. Las pruebas unitarias actúan como una red de seguridad para nuestra aplicación que nos permite modificar de forma segura nuestra aplicación en el futuro.

Hemos creado dos conjuntos de pruebas unitarias. En primer lugar, hemos probado nuestra lógica de validación mediante la creación de pruebas unitarias para nuestro nivel de servicio. A continuación, hemos probado la lógica de flujo de control mediante la creación de pruebas unitarias para nuestro nivel de controlador. Al probar el nivel de servicio, se aíslan nuestras pruebas para nuestro nivel de servicio de la capa de repositorio nuestro nivel de repositorio de simulación. Al probar el nivel de controlador, se aíslan nuestras pruebas para la capa de controlador por el nivel de servicio de simulación.

En la siguiente iteración, se modifique la aplicación póngase en contacto con el administrador para que admita grupos de contactos. Vamos a agregar esta nueva funcionalidad a la aplicación mediante un proceso de diseño de software llamado desarrollo controlado por pruebas.

>[!div class="step-by-step"]
[Anterior](iteration-4-make-the-application-loosely-coupled-vb.md)
[Siguiente](iteration-6-use-test-driven-development-vb.md)
