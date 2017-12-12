---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: "Iteración #4: hacer que la aplicación de acoplamiento flexible (C#) | Documentos de Microsoft"
author: microsoft
description: "En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que resulten más fáciles de mantener y modificar la aplicación póngase en contacto con el administrador. Para..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 1b8df72f51b4730a1fa9178b51a3770ce9edf181
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Iteración #4: hacer que la aplicación de acoplamiento flexible (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> En esta tercera iteración, aprovechamos de varios patrones de diseño de software para que resulten más fáciles de mantener y modificar la aplicación póngase en contacto con el administrador. Por ejemplo, se refactoriza nuestra aplicación para que use el modelo de repositorio y el modelo de inserción de dependencias.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación de MVC de ASP.NET de administración de contactos (C#)

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

En este cuarta iteración de la aplicación Administrador de contacto, se tiene que refactorizar la aplicación para que la aplicación de acoplamiento más flexible. Cuando una aplicación es de acoplamiento flexible, puede modificar el código en una parte de la aplicación sin necesidad de modificar el código en otras partes de la aplicación. Aplicaciones de acoplamiento flexible son más resistentes a cambiar.

Actualmente, toda la lógica de acceso y la validación de datos utilizada por la aplicación de administrador de contacto se encuentra en las clases de controlador. Se trata de una mala idea. Siempre que tenga que modificar una parte de la aplicación, corre el riesgo de introducir errores en otra parte de la aplicación. Por ejemplo, si modifica su lógica de validación, se arriesga a la introducción de errores en la lógica de acceso o el controlador de datos.

> [!NOTE] 
> 
> (SRP), una clase nunca debe tener más de un motivo para cambiarlo. Combinación de controlador, validación y lógica de la base de datos constituye una infracción masiva del principio de responsabilidad única.


Hay varias razones por las que tendrá que modificar la aplicación. Tendrá que agregar una nueva característica a la aplicación, tendrá que corregir un error en la aplicación o tendrá que modificar cómo se implementa una característica de la aplicación. Las aplicaciones son rara vez estáticas. Tienden a crecer y se modifique con el tiempo.

Por ejemplo, imagine que decide cambiar la forma de implementar la capa de acceso a datos. Derecha ahora, la aplicación póngase en contacto con el administrador utiliza Microsoft Entity Framework para tener acceso a la base de datos. Sin embargo, puede migrar a una tecnología de acceso a datos nuevos o alternativo como ADO.NET Data Services o NHibernate. Sin embargo, ya que el código de acceso de datos no está aislado desde el código de validación y el controlador, no hay ninguna manera de modificar el código de acceso a datos en la aplicación sin modificar otro código que no está directamente relacionado con el acceso a datos.

Por otro lado, cuando una aplicación es de acoplamiento flexible, puede realizar cambios en una parte de una aplicación sin tocar otras partes de una aplicación. Por ejemplo, puede cambiar las tecnologías de acceso de datos sin modificar la lógica de validación o controlador.

En esta iteración, aprovechamos de varios patrones de diseño de software que nos permitan refactorizar nuestra aplicación póngase en contacto con el administrador en una aplicación de acoplamiento más flexible. Que finalicemos, el Administrador de contacto won t no nada que lo Professionals t hacer antes. Sin embargo, se podrá cambiar la aplicación más fácilmente en el futuro.

> [!NOTE] 
> 
> La refactorización es el proceso de volver a escribir una aplicación de forma que no pierde ninguna funcionalidad existente.


## <a name="using-the-repository-software-design-pattern"></a>Mediante el patrón de diseño de Software de repositorio

Nuestro primer cambio consiste en aprovechar las ventajas de un modelo de diseño de software denominado el modelo de repositorio. Vamos a usar el modelo de repositorio para aislar el código de acceso de datos del resto de nuestra aplicación.

Implementar el modelo de repositorio, es necesario poder completar los dos pasos siguientes:

1. Crear una interfaz
2. Crear una clase concreta que implementa la interfaz

En primer lugar, necesitamos crear una interfaz que describe todos los métodos de acceso de datos que se deben realizar. La interfaz de IContactManagerRepository está contenida en la lista 1. Esta interfaz describe cinco métodos: CreateContact(), DeleteContact(), EditContact(), GetContact y ListContacts().

**Lista 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

A continuación, necesitamos crear una clase concreta que implementa la interfaz IContactManagerRepository. Como estamos usando Microsoft Entity Framework para tener acceso a la base de datos, vamos a crear una nueva clase denominada EntityContactManagerRepository. Esta clase está contenida en el listado 2.

**La lista 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Tenga en cuenta que la clase EntityContactManagerRepository implementa la interfaz IContactManagerRepository. La clase implementa los cinco de los métodos descritos por esa interfaz.

Tal vez se pregunte por qué se tendrá que preocuparse por una interfaz. ¿Por qué se necesita crear una interfaz y una clase que lo implementa?

Con una excepción, el resto de nuestra aplicación interactuará con la interfaz y no la clase concreta. En lugar de llamar a los métodos expuestos por la clase EntityContactManagerRepository, llamaremos a los métodos expuestos por la interfaz IContactManagerRepository.

De este modo, podemos implementar la interfaz con una nueva clase sin necesidad de modificar el resto de la aplicación. Por ejemplo, en una fecha futura, tengamos que queremos implementar una clase DataServicesContactManagerRepository que implementa la interfaz IContactManagerRepository. La clase DataServicesContactManagerRepository podría utilizar ADO.NET Data Services para tener acceso a una base de datos en lugar de Microsoft Entity Framework.

Si el código de aplicación programado en la interfaz de IContactManagerRepository en lugar de la clase EntityContactManagerRepository concretos, a continuación, podemos cambiar las clases concretas sin modificar el resto de nuestro código. Por ejemplo, podemos cambiar de la clase EntityContactManagerRepository a la clase DataServicesContactManagerRepository sin modificar la lógica de acceso o la validación de datos.

Programación con interfaces (abstracciones) en lugar de las clases concretas hace que nuestra aplicación sea más resistente a cambiar.

> [!NOTE] 
> 
> Puede crear rápidamente una interfaz de una clase concreta en Visual Studio seleccionando la opción de menú refactorizar Extraer interfaz. Por ejemplo, puede crear la clase EntityContactManagerRepository en primer lugar y, a continuación, usar Extraer interfaz para generar la interfaz de IContactManagerRepository automáticamente.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Utilizando el modelo de diseño de Software de inyección de dependencia

Ahora que hemos migramos nuestro código de acceso a datos a una clase distinta del repositorio, es necesario modificar nuestro controlador de contacto para utilizar esta clase. Se aprovechará denominado inyección de dependencia para utilizar la clase de repositorio en el controlador de un modelo de diseño de software.

El controlador de contacto modificado se encuentra en el listado 3.

**El listado 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Tenga en cuenta que el controlador de contacto en la lista 3 tiene dos constructores. El primer constructor pasa una instancia concreta de la interfaz IContactManagerRepository para el segundo constructor. La clase de controlador de contacto utiliza *inyección de dependencia de Constructor*.

Uno y solo el contexto que se usa la clase EntityContactManagerRepository está en el primer constructor. El resto de la clase utiliza la interfaz de IContactManagerRepository en lugar de la clase EntityContactManagerRepository concretos.

Esto facilita el modificador de implementaciones de la clase IContactManagerRepository en el futuro. Si desea usar la clase DataServicesContactRepository en lugar de la clase EntityContactManagerRepository, simplemente modifique el primer constructor.

Inserción de dependencias de constructor también hace que la clase de controlador de contacto muy comprobables. En las pruebas unitarias, puede crear instancias del controlador de contacto pasando una implementación de la clase IContactManagerRepository ficticia. Esta característica de inserción de dependencias será muy importante para nosotros en la siguiente iteración cuando se generan pruebas unitarias para la aplicación póngase en contacto con el administrador.

> [!NOTE] 
> 
> Si desea completamente desacoplar la clase de controlador de contacto de una implementación concreta de la interfaz IContactManagerRepository, a continuación, se pueden aprovechar las ventajas de una plataforma que admite la inserción de dependencias, como StructureMap o Microsoft Entity Framework (MEF). Aprovechando las ventajas de un marco de trabajo de inserción de dependencias, nunca debe hacer referencia a una clase concreta en el código.


## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Es posible que haya observado que nuestra lógica de validación se mezcla todavía con nuestra lógica de controlador en la clase del controlador modificado en el listado 3. Por la misma razón que resulta una buena idea aislar la lógica de acceso a datos, es una buena idea aislar la lógica de validación.

Para solucionar este problema, podemos crear otro [ *capa de servicio*](http://martinfowler.com/eaaCatalog/serviceLayer.html). El nivel de servicio es una capa independiente que podemos insertamos entre nuestro controlador y las clases de repositorio. El nivel de servicio contiene la lógica de negocios, además de toda nuestra lógica de validación.

El ContactManagerService se incluye en el listado 4. Contiene la lógica de validación de la clase de controlador de contacto.

**Listado 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Tenga en cuenta que el constructor para el ContactManagerService requiere un ValidationDictionary. El nivel de servicio se comunica con el nivel de controlador a través de este ValidationDictionary. Se explica lo ValidationDictionary en detalle en la sección siguiente cuando se describe el patrón de decorador.

Además, tenga en cuenta que el ContactManagerService implementa la interfaz IContactManagerService. Siempre debe procurar Programar interfaces en lugar de las clases concretas. Otras clases de la aplicación Administrador de contacto no interactúan directamente con la clase ContactManagerService. En su lugar, con una excepción, el resto de la aplicación póngase en contacto con el administrador se programa con la interfaz IContactManagerService.

La interfaz de IContactManagerService está contenida en el listado 5.

**Listado 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

La clase de controlador modificada de contacto se encuentra en el listado 6. Tenga en cuenta que el controlador de contacto ya no interactúa con el repositorio ContactManager. En su lugar, el controlador de contacto interactúa con el servicio ContactManager. Cada capa está aislada tanto como sea posible de otras capas.

**Listado 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Nuestra aplicación ya no se ejecuta mantiene del principio de responsabilidad única (SRP). El controlador de contacto en el listado 6 se ha eliminado de cada responsabilidad distintos de controlar el flujo de ejecución de la aplicación. Toda la lógica de validación tiene ha quitado del controlador de contacto e inserta en el nivel de servicio. Toda la lógica de la base de datos se ha insertado en la capa de repositorio.

## <a name="using-the-decorator-pattern"></a>Uso del patrón Decorator

Queremos poder completamente desacoplar nuestro nivel de servicio de la capa de controlador. En principio, deberíamos estar puede compilar nuestro nivel de servicio en un ensamblado independiente de la capa de controlador sin necesidad de agregar una referencia a nuestra aplicación de MVC.

Sin embargo, el nivel de servicio debe poder pasar mensajes de error de validación a la capa de controlador. ¿Cómo podemos habilitar el nivel de servicio transmitir mensajes de error de validación sin acoplar el controlador y el nivel de servicio? Podemos aprovechar las ventajas de un modelo de diseño de software denominado el [patrón Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controlador utiliza una ModelStateDictionary denominado ModelState para representar errores de validación. Por lo tanto, podría verse tentado a pasar ModelState de la capa de controlador a la capa de servicio. Sin embargo, utilizar ModelState en el nivel de servicio haría que el nivel de servicio dependiente de una característica del marco de ASP.NET MVC. Esto sería incorrecta porque algún día, puede utilizar el nivel de servicio con una aplicación de WPF en lugar de una aplicación ASP.NET MVC. En ese caso, no sería t desea hacer referencia el marco de MVC de ASP.NET para usar la clase ModelStateDictionary.

El patrón de decorador permite ajustar una clase existente en una nueva clase con el fin de implementar una interfaz. Nuestro proyecto póngase en contacto con el administrador incluye la clase ModelStateWrapper contenida en la lista 7. La clase ModelStateWrapper implementa la interfaz del listado 8.

**Listado 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Enumerar 8 - Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Si echa un vistazo más detenidamente listado 5, a continuación, verá que el nivel de servicio ContactManager usa la interfaz IValidationDictionary exclusivamente. El servicio ContactManager no es dependiente de la clase ModelStateDictionary. Cuando el controlador de contacto crea el servicio ContactManager, el controlador ajusta su ModelState similar al siguiente:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Resumen

En esta iteración, no se ha agregado ninguna funcionalidad nueva a la aplicación póngase en contacto con el administrador. El objetivo de esta iteración fue refactorizar la aplicación de Contact Manager por lo que es más fácil de mantener y modificar.

En primer lugar, se implementa el modelo de diseño de software de repositorio. Todo el código de acceso de datos se migran a una clase de repositorio ContactManager distinta.

También se aisladas nuestra lógica de validación de la lógica del controlador. Se crea una capa de servicio independiente que contenga todo el código de validación. El nivel de controlador interactúa con el nivel de servicio y el nivel de servicio interactúa con la capa de repositorio.

Cuando se crea el nivel de servicio, aprovechamos el patrón de decorador para aislar ModelState de nuestro nivel de servicio. En nuestro nivel de servicio, se programan con la interfaz de IValidationDictionary en lugar de ModelState.

Por último, aprovechamos de un modelo de diseño de software denominado el modelo de inserción de dependencias. Este patrón permite programar en interfaces (abstracciones) en lugar de las clases concretas. Implementar el patrón de diseño de inyección de dependencia también hace que el código más comprobables. En la siguiente iteración, se agregan pruebas unitarias a nuestro proyecto.

>[!div class="step-by-step"]
[Anterior](iteration-3-add-form-validation-cs.md)
[Siguiente](iteration-5-create-unit-tests-cs.md)
