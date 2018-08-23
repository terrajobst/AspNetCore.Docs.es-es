---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iteración #5: crear pruebas unitarias (VB) | Microsoft Docs'
author: microsoft
description: En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Nos simular nuestras clases de modelo de datos y generar pruebas unitarias para o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 6aee4c01c1555dd2ea95d26a005d61ddab09f6fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828853"
---
<a name="iteration-5--create-unit-tests-vb"></a>Iteración #5: crear pruebas unitarias (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de ASP.NET MVC (VB) de administración de contactos

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar. La aplicación de Contact Manager permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Creamos la aplicación a través de varias iteraciones. Con cada iteración, mejorar gradualmente la aplicación. Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación parezca interesante. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.

- Iteración #3: agregar validación de formulario. En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación tenga un acoplamiento. En este cuarta iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración #6: usar el desarrollo controlado por pruebas. En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.

- Iteración #7: agregar funcionalidad Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.


## <a name="this-iteration"></a>Esta iteración

En la iteración anterior de la aplicación de Contact Manager, hemos refactorizado la aplicación para tener más de un acoplamiento flexible. Se separan la aplicación en un controlador distinto, servicio y capas de repositorio. Cada capa interactúa con la capa debajo de ella a través de interfaces.

Hemos refactorizado la aplicación para hacer más fácil de mantener y modificar la aplicación. Por ejemplo, si es necesario usar una nueva tecnología de acceso de datos, podemos simplemente cambiamos la capa de repositorio sin tocar el controlador o el nivel de servicio. Haciendo que el Administrador de contactos de correspondencia imprecisa, se ve facilitado la aplicación sea más resistente a cambiar.

Pero, ¿qué ocurre cuando sea necesario agregar una nueva característica a la aplicación de Contact Manager? O bien, ¿qué ocurre cuando se corrija un error? Una verdad triste pero bien probada de escribir código es cada vez que toque código que cree el riesgo de introducir nuevos errores.

Por ejemplo, un día bien, el administrador podría pedirle que agregar una nueva característica al administrador de contactos. Quiere agregar compatibilidad para grupos de contactos. Quiere permitir que los usuarios organizar sus contactos en grupos como amigos, negocios y así sucesivamente.

Para implementar esta nueva característica, deberá modificar los tres niveles de la aplicación de Contact Manager. Deberá agregar nueva funcionalidad a los controladores, la capa de servicio y el repositorio. Tan pronto como se empiece a modificar código, corre el riesgo de romper la funcionalidad que antes funcionaba.

Refactorización de nuestra aplicación en capas separadas, como hicimos en la iteración anterior, era una buena opción. Fue una buena opción porque nos permite realizar cambios en capas completas sin tocar el resto de la aplicación. Sin embargo, si desea hacer más fácil de mantener y modificar el código dentro de una capa, deberá crear pruebas unitarias para el código.

Usa una unidad prueba a una unidad de código individual. Estas unidades de código son menores que las capas de aplicación completo. Por lo general, utilice una prueba unitaria para comprobar si un método concreto en el código se comporta de la manera en que se espera. Por ejemplo, podría crear una prueba unitaria para el método CreateContact() expuesta la clase ContactManagerService.

Las pruebas unitarias para un trabajo de la aplicación, como una red de seguridad. Cada vez que modifique el código en una aplicación, puede ejecutar un conjunto de pruebas unitarias para comprobar si la modificación interrumpe la funcionalidad existente. Pruebas unitarias que el código sea segura modificar. Pruebas unitarias que todo el código de la aplicación sea más resistente a cambiar.

En esta iteración, agregamos las pruebas unitarias a nuestra aplicación de Contact Manager. De este modo, en la siguiente iteración, podemos agregar grupos de contactos a nuestra aplicación sin preocuparse por romper la funcionalidad existente.

> [!NOTE] 
> 
> Hay una variedad de marcos como NUnit, xUnit.net y MbUnit de pruebas unitarias. En este tutorial, usamos incluido con Visual Studio del marco de pruebas unitarias. Sin embargo, podría podríamos usar uno de estos marcos de trabajo alternativos.


## <a name="what-gets-tested"></a>¿Qué Obtiene probado

En el mundo perfecto, todo el código podría estar cubierto por las pruebas unitarias. En el mundo perfecto, tendría la red de seguridad perfecta. Sería capaz de modificar cualquier línea de código en la aplicación y sepa al instante, mediante la ejecución de las pruebas unitarias, si el cambio interrumpió la funcionalidad existente.

Sin embargo, no queremos t en directo en un mundo perfecto. En la práctica, al escribir pruebas unitarias, concentrarse en escribir pruebas para la lógica de negocios (por ejemplo, la lógica de validación). En concreto, le *no* escribir pruebas unitarias para los datos de acceso lógica o su lógica de la vista.

Para que sea útil, las pruebas unitarias deben ejecutar muy rápidamente. Puede acumular fácilmente cientos o incluso miles de pruebas unitarias para una aplicación. Si las pruebas unitarias tardan mucho tiempo en ejecutarse, a continuación, se evitará ejecutarlas. En otras palabras, cuánto tiempo se ejecutan pruebas unitarias son inútiles para fines de codificación de un día para otro.

Por este motivo, normalmente no escribe pruebas unitarias para código que interactúa con una base de datos. Ejecutar cientos de pruebas unitarias en una base de datos serían demasiado lento. En su lugar, simulación de la base de datos y escribir código que interactúa con la base de datos ficticio (analizamos la simulación de una base de datos a continuación).

Por un motivo similar, normalmente no escribe pruebas unitarias para las vistas. Para probar una vista, debe poner en marcha un servidor web. Dado que poner en marcha un servidor web es un proceso relativamente lento, no se recomienda crear pruebas unitarias para las vistas.

Si la vista contiene una lógica complicada, a continuación, puede mover la lógica a los métodos auxiliares. Puede escribir pruebas unitarias para métodos auxiliares que se ejecutan sin poner en marcha un servidor web.

> [!NOTE] 
> 
> Mientras prueba escribir la lógica de acceso a datos o lógica de la vista no es una buena idea al escribir pruebas unitarias, estas pruebas pueden ser muy valiosas al construcción funcional o la integración de las pruebas.


> [!NOTE] 
> 
> ASP.NET MVC es el motor de vistas de formularios Web. Mientras el motor de vistas de formularios Web depende de un servidor web, no pueden ser otros motores de vista.


## <a name="using-a-mock-object-framework"></a>Uso de un marco de objeto ficticio

Al compilar las pruebas unitarias, casi siempre necesita aprovechar las ventajas de un marco de simulación de objeto. Un marco de simulación de objeto le permite crear simulacros y auxiliares de las clases en la aplicación.

Por ejemplo, puede usar un marco de simulación de objeto para generar una versión ficticia de la clase de repositorio. De este modo, puede usar la clase de repositorio ficticio en lugar de la clase de repositorio reales en las pruebas unitarias. El repositorio ficticio permite evitar la ejecución de código de la base de datos al ejecutar una prueba unitaria.

Visual Studio no incluye un marco de simulación de objeto. Sin embargo, hay varios marcos de simulación de objeto comercial y de código abierto disponibles para .NET framework:

1. Moq - este marco de trabajo está disponible bajo la licencia BSD de código abierto. Puede descargar Moq desde [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks, este marco de trabajo está disponible bajo la licencia BSD de código abierto. Puede descargar Rhino Mocks desde [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - se trata de un entorno comercial. Puede descargar una versión de prueba de [ http://www.typemock.com/ ](http://www.typemock.com/).

En este tutorial, decidí usar Moq. Sin embargo, podría usar fácilmente Rhino Mocks o Typemock Isolator para crear el simulacro de objetos para la aplicación de Contact Manager.

Para poder usar Moq, es preciso completar los pasos siguientes:

1. .
2. Antes de descomprimir la descarga, asegúrese de que haga clic en el archivo y haga clic en el botón rotulado **desbloquear** (consulte la figura 1).
3. Descomprima la descarga.
4. Agregue una referencia al ensamblado Moq al proyecto de prueba seleccionando la opción de menú **proyecto, agregar referencia** para abrir el **Agregar referencia** cuadro de diálogo. En la pestaña Examinar, vaya a la carpeta donde descomprimió Moq y seleccione el ensamblado Moq.dll. Haga clic en el **Aceptar** botón (consulte la figura 2).


[![Moq desbloqueo](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: Moq desbloqueo ([haga clic aquí para ver imagen en tamaño completo](iteration-5-create-unit-tests-vb/_static/image2.png))


[![Referencias después de agregar Moq](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: referencias después de agregar Moq ([haga clic aquí para ver imagen en tamaño completo](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Crear pruebas unitarias para el nivel de servicio

Permiten s empiece por crear un conjunto de pruebas unitarias para nuestro nivel de servicio de Contact Manager aplicación s. Vamos a usar estas pruebas para comprobar nuestra lógica de validación.

Cree una nueva carpeta denominada Models en el proyecto ContactManager.Tests. A continuación, haga clic en la carpeta modelos y seleccione **Add, nueva prueba**. El **agregar nueva prueba** aparece el cuadro de diálogo se muestra en la figura 3. Seleccione el **de prueba unitaria** plantilla y el nombre de la nueva prueba ContactManagerServiceTest.vb. Haga clic en el **Aceptar** para agregar la nueva prueba a su proyecto de prueba.

> [!NOTE] 
> 
> En general, conviene que la estructura de carpetas del proyecto de prueba para que coincida con la estructura de carpetas del proyecto de ASP.NET MVC. Por ejemplo, coloque las pruebas del controlador en una carpeta de controladores, las pruebas de modelo en una carpeta Models y así sucesivamente.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.cs ([haga clic aquí para ver imagen en tamaño completo](iteration-5-create-unit-tests-vb/_static/image6.png))


Inicialmente, va a probar el método CreateContact() expone la clase ContactManagerService. Vamos a crear las cinco pruebas siguientes:

- CreateContact() - pruebas que CreateContact() devuelve el valor true cuando un contacto válido se pasa al método.
- CreateContactRequiredFirstName() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con un nombre que faltan se pasa al método CreateContact().
- CreateContactRequredLastName() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con un apellido que faltan se pasa al método CreateContact().
- CreateContactInvalidPhone() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con un número de teléfono no válido se pasa al método CreateContact().
- CreateContactInvalidEmail() - pruebas que se agrega un mensaje de error al estado del modelo cuando un contacto con una dirección de correo electrónico no válido se pasa al método CreateContact()...

La primera prueba comprueba que un contacto válido no genera un error de validación. Las pruebas restantes comprueban cada una de las reglas de validación.

El código para estas pruebas se encuentra en el listado 1.

**Listado 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


Dado que se usa la clase Contact en el listado 1, tenemos que agregar una referencia a Microsoft Entity Framework para nuestro proyecto de prueba. Agregue una referencia al ensamblado System.Data.Entity.


Listado 1 contiene un método denominado Initialize() decorada con el atributo [TestInitialize]. Este método es llamado automáticamente antes de que se ejecuta cada una de las pruebas unitarias (se llama justo antes de cada una de las pruebas unitarias 5 veces). El método Initialize() crea un repositorio ficticio con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Esta línea de código usa el marco de trabajo Moq para generar un repositorio simulado desde la interfaz IContactManagerRepository. Se usa el repositorio ficticio en lugar de la EntityContactManagerRepository real para evitar el acceso a la base de datos cuando se ejecuta cada prueba unitaria. El repositorio ficticio implementa los métodos de la interfaz IContactManagerRepository, pero los métodos don t hace realmente nada.

> [!NOTE] 
> 
> Cuando se usa el marco de trabajo Moq, hay una diferencia entre \_mockRepository y \_mockRepository.Object. El primero se refiere a la clase de simulacro (de IContactManagerRepository) que contiene métodos para especificar cómo se comportará el repositorio ficticio. El segundo se refiere al repositorio ficticio real que implementa la interfaz IContactManagerRepository.


Al crear una instancia de la clase ContactManagerService, se usa el repositorio ficticio en el método Initialize(). Todas las pruebas unitarias individuales utilizan esta instancia de la clase ContactManagerService.

Listado 1 contiene cinco métodos que corresponden a cada una de las pruebas unitarias. Cada uno de estos métodos se decora con el atributo [TestMethod]. Al ejecutar las pruebas unitarias, se llama a cualquier método que tenga este atributo. En otras palabras, cualquier método esté decorado con el atributo [TestMethod] es una prueba unitaria.

La primera prueba unitaria, denominada CreateContact(), comprueba que una llamada a CreateContact() devuelve el valor true cuando una instancia válida de la clase Contact se pasa al método. La prueba crea una instancia de la clase Contact, llama al método CreateContact() y comprueba que CreateContact() devuelve el valor true.

Las pruebas restantes comprueban que cuando se llama al método CreateContact() con un contacto no válido, a continuación, el método devuelve false y se agrega el mensaje de error de validación esperado al estado del modelo. Por ejemplo, la prueba CreateContactRequiredFirstName() crea una instancia de la clase Contact con una cadena vacía para su propiedad FirstName. A continuación, se llama al método de CreateContact() con el contacto no válido. Por último, la prueba comprueba que CreateContact() devuelve false y el estado del modelo contiene el mensaje de error de validación esperado "First name is required."

Puede ejecutar las pruebas unitarias en el listado 1 seleccionando la opción de menú **pruebas, ejecutar, todas las pruebas de la solución (CTRL + R, A)**. Los resultados de las pruebas se muestran en la ventana Resultados de pruebas (consulte la figura 4).


[![Resultados de pruebas](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: resultados de pruebas ([haga clic aquí para ver imagen en tamaño completo](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Crear pruebas unitarias para los controladores

Aplicación de ASP.NET MVC controlan el flujo de interacción del usuario. Cuando un controlador de pruebas que desea comprobar si el controlador devuelve el resultado y ver datos de la acción adecuada. También es posible que desee comprobar si un controlador interactúa con las clases de modelo de la manera prevista.

Por ejemplo, el listado 2 contiene dos pruebas unitarias para el método Create() del controlador de contacto. La primera prueba unitaria comprueba si un contacto válido se pasa al método Create(), a continuación, el método Create() redirige a la acción del índice. En otras palabras, cuando se pasa un contacto válido, el método Create() debe devolver un RedirectToRouteResult que representa la acción del índice.

No queremos t desea probar la capa de servicio ContactManager cuando estamos probando el nivel de controlador. Por lo tanto, nos simular la capa de servicio con el código siguiente en el método Initialize:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

En la prueba unitaria CreateValidContact(), nos simular el comportamiento de la llamada a la capa de servicio CreateContact() método con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Esta línea de código hace que el servicio ContactManager ficticio devolver el valor true cuando se llama a su método CreateContact(). Por el nivel de servicio de simulación, podemos probar el comportamiento de nuestro controlador sin necesidad de ejecutar cualquier código en el nivel de servicio.

La segunda prueba unitaria comprueba que la acción Create() devuelve la vista de creación cuando un contacto no válido se pasa al método. Hacemos que el método CreateContact() para devolver el valor false con la siguiente línea de código de nivel de servicio:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Si el método Create() se comporta tal como esperábamos, a continuación, debe devolver la vista de creación cuando el nivel de servicio devuelve el valor false. De este modo, el controlador puede mostrar los mensajes de error de validación en la vista de creación y el usuario tiene la oportunidad de corregir ese propiedades del contacto no válido.


Si va a crear pruebas unitarias para los controladores debe devolver los nombres de vista explícita de sus acciones de controlador. Por ejemplo, no devuelven una vista similar al siguiente:

Devolver View()

En su lugar, devuelve la vista similar al siguiente:

Devolver View("Create")

Si no es explícito al devolver una vista de la propiedad ViewResult.ViewName devuelve una cadena vacía.


**Listado 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Resumen

En esta iteración, hemos creado las pruebas unitarias para nuestra aplicación de Contact Manager. Podemos ejecutar estas pruebas unitarias en cualquier momento para comprobar que nuestra aplicación todavía se comporta de la manera en que se espera. Las pruebas unitarias actúan como una red de seguridad para nuestra aplicación lo que nos permite modificar de forma segura nuestra aplicación en el futuro.

Hemos creado dos conjuntos de pruebas unitarias. En primer lugar, hemos probado nuestra lógica de validación mediante la creación de pruebas unitarias para nuestro nivel de servicio. A continuación, hemos probado nuestra lógica de flujo de control mediante la creación de pruebas unitarias para nuestro nivel de controlador. Al probar nuestro nivel de servicio, nuestras pruebas nuestro nivel de servicio se habían aislada de la capa de repositorio mediante la simulación de la capa de repositorio. Al probar el nivel de controlador, hemos aislado nuestras pruebas para nuestro nivel de controlador mediante la capa de servicio de simulación.

En la siguiente iteración, se modificará la aplicación de Contact Manager para que admita los grupos de contactos. Vamos a agregar esta nueva funcionalidad a la aplicación mediante un proceso de diseño de software denominado desarrollo controlado por pruebas.

> [!div class="step-by-step"]
> [Anterior](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Siguiente](iteration-6-use-test-driven-development-vb.md)
