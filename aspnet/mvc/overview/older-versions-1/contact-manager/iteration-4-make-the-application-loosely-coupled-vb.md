---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: 'Iteración #4: hacer que la aplicación tenga un acoplamiento (VB) | Microsoft Docs'
author: microsoft
description: En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Para...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 246069902e67d3d901bbd40e950198825a5ab01d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804677"
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iteración #4: hacer que la aplicación tenga un acoplamiento (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descargue el código](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación de ASP.NET MVC (VB) de administración de contactos

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa desde el principio para finalizar. La aplicación de Contact Manager permite almacenar información de contacto - nombres, números de teléfono y direcciones de correo electrónico: para obtener una lista de personas.

Creamos la aplicación a través de varias iteraciones. Con cada iteración, mejorar gradualmente la aplicación. Es el objetivo de este enfoque de varias iteraciones para que pueda entender el motivo de cada cambio.

- Iteración #1: crear la aplicación. En la primera iteración, se creará el Administrador de contactos de la manera más sencilla posible. Se agrega compatibilidad con las operaciones de base de datos básicos: creación, lectura, actualización y eliminación (CRUD).

- Iteración #2: hacer que la aplicación parezca interesante. En esta iteración, mejorar la apariencia de la aplicación mediante la modificación de la página principal de la vista de MVC de ASP.NET predeterminada y en cascada de hoja de estilos.

- Iteración #3: agregar validación de formulario. En la tercera iteración, se agrega una validación de formulario básico. Evitamos personas desde el envío de un formulario sin completar los campos obligatorios. También hemos validar direcciones de correo electrónico y números de teléfono.

- Iteración #4: hacer que la aplicación tenga un acoplamiento. En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que sea más fácil de mantener y modificar la aplicación de Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y la inserción de dependencias.

- Iteración #5: crear pruebas unitarias. En la iteración quinta, realizamos nuestra aplicación más fácil de mantener y modificar mediante la adición de las pruebas unitarias. Hemos simular nuestras clases de modelo de datos y generar pruebas unitarias para nuestros controladores y la lógica de validación.

- Iteración #6: usar el desarrollo controlado por pruebas. En esta iteración sexta, se agregan nuevas funciones a nuestra aplicación escribir pruebas unitarias en primer lugar y escribir código en las pruebas unitarias. En esta iteración, se agrega grupos de contactos.

- Iteración #7: agregar funcionalidad Ajax. En la iteración séptima, mejorar la capacidad de respuesta y el rendimiento de nuestra aplicación agregando compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En este cuarta iteración de la aplicación de Contact Manager, se refactoriza la aplicación para que la aplicación tenga un acoplamiento flexible. Cuando una aplicación tiene un acoplamiento flexible, puede modificar el código en una parte de la aplicación sin necesidad de modificar el código en otras partes de la aplicación. Aplicaciones de acoplamiento flexible son más resistentes a cambiar.

Actualmente, toda la lógica de acceso y la validación de datos utilizada por la aplicación de Contact Manager se encuentra en las clases del controlador. Se trata de una mala idea. Siempre que necesite modificar una parte de la aplicación, corre el riesgo de introducir errores en otra parte de la aplicación. Por ejemplo, si modifica su lógica de validación, se arriesga a introducir nuevos errores en la lógica de acceso o un controlador de datos.

> [!NOTE] 
> 
> (SRP), una clase nunca debe tener más de una razón para cambiar. Mezcla de controlador, validación y lógica de la base de datos es una infracción del principio de responsabilidad única masiva.


Hay varias razones que es posible que deba modificar la aplicación. Es posible que deba agregar una nueva característica a la aplicación, es posible que deba corregir un error en la aplicación, o es posible que deba modificar cómo se implementa una característica de la aplicación. Las aplicaciones son estáticas con poca frecuencia. Tienden a crecer y se modifique con el tiempo.

Por ejemplo, imagine que decide cambiar cómo implementar la capa de acceso a datos. Derecha ahora, la aplicación de Contact Manager utiliza Microsoft Entity Framework para tener acceso a la base de datos. Sin embargo, podría decidir migrar a una tecnología de acceso de datos nuevas o alternativas como ADO.NET Data Services o NHibernate. Sin embargo, dado que el código de acceso a datos no está aislado de la validación y el controlador de código, no hay ninguna manera para modificar el código de acceso a datos en la aplicación sin modificar otro código que no está directamente relacionado con el acceso a datos.

Por otro lado, cuando una aplicación tiene un acoplamiento flexible, puede realizar cambios en una parte de una aplicación sin tocar otras partes de una aplicación. Por ejemplo, puede cambiar las tecnologías de acceso de datos sin modificar la lógica de validación o controlador.

En esta iteración, aprovechamos de varios patrones de diseño de software que nos permiten refactorizar nuestra aplicación de Contact Manager en una aplicación de acoplamiento más flexible. Si hemos terminado, el Administrador de contactos ha ganado t hacer todo lo que lo t hacer antes. Sin embargo, podremos cambiar la aplicación más fácilmente en el futuro.

> [!NOTE] 
> 
> La refactorización es el proceso de volver a escribir una aplicación de manera que no pierda ninguna funcionalidad existente.


## <a name="using-the-repository-software-design-pattern"></a>Con el patrón de diseño de Software de repositorio

Nuestro primer cambio consiste en aprovechar las ventajas de un patrón de diseño de software denominado el modelo de repositorio. Vamos a usar el modelo de repositorio para aislar el código de acceso a datos del resto de nuestra aplicación.

Implementar el patrón de repositorio requiere que complete los dos pasos siguientes:

1. Crear una interfaz
2. Crear una clase concreta que implementa la interfaz

En primer lugar, necesitamos crear una interfaz que describe todos los métodos de acceso de datos que se deben realizar. La interfaz IContactManagerRepository está contenida en el listado 1. Esta interfaz describe cinco métodos: CreateContact(), DeleteContact(), EditContact(), GetContact y ListContacts().

**Listado 1 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

A continuación, se debe crear una clase concreta que implementa la interfaz IContactManagerRepository. Como estamos usando Microsoft Entity Framework para tener acceso a la base de datos, vamos a crear una nueva clase denominada EntityContactManagerRepository. Esta clase se encuentra en el listado 2.

**Listado 2 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Tenga en cuenta que la clase EntityContactManagerRepository implementa la interfaz IContactManagerRepository. La clase implementa cinco de los métodos descritos por esa interfaz.

Tal vez se pregunte por qué necesitamos lidiar con una interfaz. ¿Por qué se necesita crear una interfaz y una clase que lo implementa?

Con una excepción, el resto de nuestra aplicación interactuará con la interfaz y no la clase concreta. En lugar de llamar a los métodos expuestos por la clase EntityContactManagerRepository, llamaremos a los métodos expuestos por la interfaz IContactManagerRepository.

De este modo, podemos implementar la interfaz con una nueva clase sin necesidad de modificar el resto de nuestra aplicación. Por ejemplo, en una fecha futura, podría desear implementar una clase DataServicesContactManagerRepository que implementa la interfaz IContactManagerRepository. La clase DataServicesContactManagerRepository podría utilizar ADO.NET Data Services para tener acceso a una base de datos en lugar de Microsoft Entity Framework.

Si el código de aplicación programado en la interfaz IContactManagerRepository en lugar de la clase EntityContactManagerRepository, a continuación, podemos cambiar las clases concretas sin modificar el resto de nuestro código. Por ejemplo, podemos cambiar de la clase EntityContactManagerRepository a la clase DataServicesContactManagerRepository sin modificar la lógica de acceso o la validación de datos.

La programación de interfaces (abstracciones) en lugar de clases concretas hace que sea más resistente a cambiar nuestra aplicación.

> [!NOTE] 
> 
> Puede crear rápidamente una interfaz de una clase concreta dentro de Visual Studio seleccionando la opción de menú refactorizar Extraer interfaz. Por ejemplo, puede crear la clase EntityContactManagerRepository primero y, a continuación, utilizar Extraer interfaz para generar la interfaz IContactManagerRepository automáticamente.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Usar el patrón de diseño de Software de inyección de dependencia

Ahora que hemos hemos migrado nuestro código de acceso a datos a una clase de repositorio independiente, necesitamos modificar nuestro controlador de contacto para utilizar esta clase. Se aprovechará un patrón de diseño de software denominado inyección de dependencia para usar la clase de repositorio en nuestro controlador.

El controlador modificado de contacto se encuentra en el listado 3.

**Listado 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Tenga en cuenta que el controlador de contacto en el listado 3 tiene dos constructores. El primer constructor pasa una instancia concreta de la interfaz IContactManagerRepository para el segundo constructor. La clase de controlador contacto utiliza *inserción de dependencias de Constructor*.

El uno y solo el contexto que se usa la clase EntityContactManagerRepository está en el primer constructor. El resto de la clase usa la interfaz de IContactManagerRepository en lugar de la clase EntityContactManagerRepository concreta.

Esto facilita el modificador de implementaciones de la clase IContactManagerRepository en el futuro. Si desea usar la clase DataServicesContactRepository en lugar de la clase EntityContactManagerRepository, sólo tiene que modificar el primer constructor.

Inserción de dependencias de constructor también hace que la clase de controlador Contact muy fácil de probar. En las pruebas unitarias, puede crear instancias del controlador de contacto pasando una implementación ficticia de la clase IContactManagerRepository. Esta característica de inserción de dependencias será muy importante para nosotros en la siguiente iteración cuando se compilación pruebas unitarias para la aplicación de Contact Manager.

> [!NOTE] 
> 
> Si desea desacoplar por completo la clase de controlador de contacto de una implementación concreta de la interfaz IContactManagerRepository, a continuación, se pueden aprovechar las ventajas de un marco que admite la inserción de dependencias, como StructureMap o Microsoft Entity Framework (MEF). Aprovechando las ventajas de un marco de inserción de dependencias, nunca debe hacer referencia a una clase concreta en el código.


## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Es posible que haya observado que nuestra lógica de validación todavía se combina con la lógica del controlador en la clase del controlador modificado en el listado 3. Para la misma razón que resulta una buena idea aislar nuestra lógica de acceso a datos, es una buena idea aislar nuestra lógica de validación.

Para corregir este problema, podemos crear otra [capa de servicio](http://martinfowler.com/eaaCatalog/serviceLayer.html). La capa de servicio es una capa independiente que podemos insertar entre nuestro controlador y las clases de repositorio. La capa de servicio contiene la lógica de negocios incluida toda nuestra lógica de validación.

El ContactManagerService está contenida en el listado 4. Contiene la lógica de validación de la clase de controlador de contacto.

**Listado 4 - Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Tenga en cuenta que el constructor para la ContactManagerService requiere un ValidationDictionary. La capa de servicio se comunica con el nivel de controlador a través de este ValidationDictionary. Analizamos la ValidationDictionary en detalle en la sección siguiente al hablar con el patrón Decorator.

Además, tenga en cuenta que el ContactManagerService implementa la interfaz IContactManagerService. Debe esforzarse siempre para programar en interfaces en lugar de clases concretas. Otras clases en la aplicación de Contact Manager no interactúan directamente con la clase ContactManagerService. En su lugar, con una excepción, el resto de la aplicación de Contact Manager programado en la interfaz IContactManagerService.

La interfaz IContactManagerService está contenida en el listado 5.

**Listado 5 - Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La clase de controlador modificada de contacto se encuentra en el listado 6. Tenga en cuenta que el controlador de contacto ya no interactúa con el repositorio ContactManager. En su lugar, el controlador de contactos interactúa con el servicio de ContactManager. Cada capa se aísla tanto como sea posible desde otras capas.

**Listado 6 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Nuestra aplicación ya no se ejecuta mantiene del principio de responsabilidad única (SRP). Se ha quitado el controlador de contacto en el listado 6 de cada responsabilidad distintos de control del flujo de ejecución de la aplicación. Toda la lógica de validación ha tiene quitó el controlador de contacto e insertan en la capa de servicio. Toda la lógica de la base de datos se ha insertado en la capa de repositorio.

## <a name="using-the-decorator-pattern"></a>Uso del patrón Decorator

Queremos poder separar completamente nuestro nivel de servicio desde nuestro nivel de controlador. En principio, podremos nuestro nivel de servicio en un ensamblado independiente de la capa de nuestro controlador de compilación sin necesidad de agregar una referencia a nuestra aplicación de MVC.

Sin embargo, la capa de servicio debe poder pasar los mensajes de error de validación a la capa de controlador. ¿Cómo podemos habilitar el nivel de servicio transmitir los mensajes de error de validación sin acoplamiento el controlador y la capa de servicio? Podemos aprovechar las ventajas de un patrón de diseño de software denominado el [patrón Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controlador utiliza un ModelStateDictionary denominado ModelState para representar errores de validación. Por lo tanto, podría verse tentado a pasar ModelState desde el nivel de controlador a la capa de servicio. Sin embargo, usar ModelState en el nivel de servicio haría que la capa de servicio depende de una característica de ASP.NET MVC framework. Esto sería incorrecta porque, en algún momento, es posible que desea usar la capa de servicio con una aplicación de WPF en lugar de una aplicación ASP.NET MVC. En ese caso, no sería t desea hacer referencia el marco de ASP.NET MVC para usar la clase ModelStateDictionary.

El patrón Decorator permite ajustar una clase existente en una nueva clase con el fin de implementar una interfaz. Nuestro proyecto Contact Manager incluye la clase ModelStateWrapper contenida en la lista 7. La clase ModelStateWrapper implementa la interfaz del listado 8.

**Listado 7 - Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Listado 8 - Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Si echa un vistazo más detenidamente listado 5, a continuación, verá que la capa de servicio ContactManager usa la interfaz IValidationDictionary exclusivamente. El servicio ContactManager no es dependiente de la clase ModelStateDictionary. Cuando el controlador de contacto, crea el servicio ContactManager, el controlador ajusta su ModelState similar al siguiente:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Resumen

En esta iteración, no se agregó ninguna funcionalidad nueva a la aplicación de Contact Manager. El objetivo de esta iteración era refactorizar la aplicación de Contact Manager por lo que es más fácil de mantener y modificar.

En primer lugar, se implementa el patrón de diseño de software de repositorio. Hemos migrado todo el código de acceso de datos a una clase de repositorio ContactManager independiente.

También nos habían aislada nuestra lógica de validación de la lógica del controlador. Creamos una capa de servicio independiente que contenga todo nuestro código de validación. El nivel de controlador interactúa con el nivel de servicio y la capa de servicio interactúa con la capa de repositorio.

Cuando se crea la capa de servicio, aprovechamos el patrón Decorator para aislar ModelState desde nuestro nivel de servicio. En nuestro nivel de servicio, se programan con la interfaz IValidationDictionary en lugar de ModelState.

Por último, aprovechamos un patrón de diseño de software denominado el patrón de inserción de dependencias. Este patrón nos permite programar en interfaces (abstracciones) en lugar de clases concretas. Implementar el patrón de diseño de la inserción de dependencias también hace que nuestro código más estable. En la siguiente iteración, agregamos las pruebas unitarias para nuestro proyecto.

> [!div class="step-by-step"]
> [Anterior](iteration-3-add-form-validation-vb.md)
> [Siguiente](iteration-5-create-unit-tests-vb.md)
