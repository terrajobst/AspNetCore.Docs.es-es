---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar automatizada pruebas unitarias | Documentos de Microsoft
author: microsoft
description: "Paso 12, muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que comprueben nuestra funcionalidad NerdDinner, así que nos proporcionará la confianza para realizar cambios..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 1a4258054d90b2d5bcc06a63fb6f3b4673a4837d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="enable-automated-unit-testing"></a>Habilitar pruebas unitarias automatizadas
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 12 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 12, muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que comprueben nuestra funcionalidad NerdDinner, así que nos proporcionará la confianza para realizar cambios y mejoras a la aplicación en el futuro.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner paso 12: Pruebas unitarias

Vamos a desarrollar un conjunto de pruebas unitarias automatizadas que comprueben nuestra funcionalidad NerdDinner, así que nos proporcionará la confianza para realizar cambios y mejoras a la aplicación en el futuro.

### <a name="why-unit-test"></a>¿Por qué pruebas unitarias?

En la unidad en un día de trabajo tiene flash repentino de ideas acerca de una aplicación que esté trabajando. Se detecta que hay un cambio que se puede implementar que harán que la aplicación significativamente mejor. Podría ser una refactorización que limpia el código, agrega una nueva característica o corrige un error.

La pregunta al cuando llegan en el equipo que es: "cómo segura es para realizar esta mejora?" ¿Qué ocurre si la realización del cambio tiene efectos secundarios o interrumpe algo? El cambio puede ser simple y solo tardará unos minutos para implementar, pero ¿qué ocurre si tarda horas para probar manualmente todos los escenarios de aplicación? ¿Qué ocurre si se olvida de cubrir un escenario y una aplicación rota entre en producción? ¿Está realizando esta mejora realmente vale la pena todo el esfuerzo?

Pruebas unitarias automatizadas pueden proporcionar una red de seguridad que permite mejorar continuamente las aplicaciones y evitar la atreve del código que está trabajando. Tener pruebas que comprobar rápidamente si la funcionalidad le permite al código con confianza – y le permiten realizar mejoras que podría en caso contrario, no se sientan cómodos automatizadas llevando a cabo. También ayudan a crear soluciones que son más fácil de mantener y tienen una duración más larga - lo que lleva a una mayor rentabilidad de la inversión.

El marco de MVC de ASP.NET permite fácilmente y natural a la funcionalidad de aplicación de prueba de unidad. También permite que un flujo de trabajo de desarrollo de controlado por pruebas (TDD) que permite el desarrollo basado en pruebas en primer lugar.

### <a name="nerddinnertests-project"></a>Proyecto NerdDinner.Tests

Cuando creamos nuestra aplicación NerdDinner al principio de este tutorial, nos estábamos le presenta un cuadro de diálogo que pregunta si desea crear un proyecto de prueba unitaria para ir junto con el proyecto de aplicación:

![](enable-automated-unit-testing/_static/image1.png)

Se mantiene el "Sí, crear un proyecto de prueba unitaria" botón de radio seleccionado: lo que provocó que se va a agregar a nuestra solución un proyecto de "NerdDinner.Tests":

![](enable-automated-unit-testing/_static/image2.png)

El proyecto NerdDinner.Tests hace referencia al ensamblado de proyecto de aplicación de NerdDinner y nos permite agregar fácilmente las pruebas automatizadas que compruebe la funcionalidad de la aplicación.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Crear pruebas unitarias para nuestra clase de modelo de la cena

Vamos a agregar algunas pruebas a nuestro proyecto NerdDinner.Tests que comprueban la clase cena que creamos cuando creamos nuestro nivel de modelo.

Comenzaremos por crear una carpeta nueva dentro de nuestro proyecto de prueba denominado "Modelos", donde se colocará nuestras pruebas relacionadas con el modelo. A continuación, se le haga doble clic en la carpeta y elija el **Add -&gt;nueva prueba** comando de menú. Se abrirá el cuadro de diálogo "Agregar nueva prueba".

También podrá optar por crear una "prueba unitaria" y asígnele el nombre "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Cuando se hace clic en el botón "Aceptar" Visual Studio agregará (y abra) un archivo DinnerTest.cs al proyecto:

![](enable-automated-unit-testing/_static/image4.png)

La plantilla de prueba de unidad de Visual Studio predeterminada tiene una serie de modelo de código dentro de él que resultar un poco complicado. Vamos a limpiar para que solo contenga el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

El atributo [TestClass] en la clase DinnerTest anterior lo identifica como una clase que contendrá pruebas, así como la inicialización de pruebas opcional y el código de destrucción. Podemos definir pruebas dentro de él mediante la adición de métodos públicos que tienen un atributo [TestMethod].

A continuación se muestran el primero de dos pruebas agregaremos que actúan sobre nuestra clase cena. La primera prueba comprueba que nuestra cena no es válido si se crea una nueva cena sin todas las propiedades no están establecidas correctamente. La segunda prueba comprueba que nuestra cena es válida cuando una cena tiene todas las propiedades establecidas con valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Observará anteriormente que nuestros nombres de prueba son muy explícitas (y un poco detallado). Estamos haciendo esto porque nos tengamos que terminan por crear cientos o miles de pruebas de pequeñas y queremos que resulte sencillo determinar rápidamente el intento y el comportamiento de cada uno de ellos (especialmente si estamos buscando a través de una lista de errores en un ejecutor de pruebas). Los nombres de prueba deben denominarse después de la funcionalidad que se están probando. Anteriormente, usamos un "sustantivo\_debe\_verbo" patrón de nomenclatura.

Le estamos estructurar las pruebas con pruebas patrón y que es el acrónimo "Arrange, Act, Assert" "AAA":

- Organizar: La unidad que se está probando el programa de instalación
- Acción: Ejecute la unidad sometida a prueba y capturar los resultados
- Aserción: Comprobar el comportamiento

Cuando se escribe pruebas que deseamos evitar tener las pruebas individuales hacer demasiado. En su lugar, cada prueba debe comprobar solo un concepto único (que le resultará mucho más fácil de identificar la causa de errores). Es aconsejable probar y basta una sola instrucción para cada prueba de aserción. Si tiene más de una instrucción en un método de prueba de aserción, asegúrese de que se usa para probar el mismo concepto. En caso de duda, asegúrese de otra prueba.

### <a name="running-tests"></a>Ejecutando pruebas

Visual Studio 2008 Professional (y las ediciones superiores) incluyen un ejecutor de pruebas integradas que puede usarse para ejecutar proyectos de prueba unitaria de Visual Studio desde el IDE. Podemos seleccionar el **prueba -&gt;ejecución -&gt;todas las pruebas de la solución** comando de menú (o tipo Ctrl R, A) para ejecutar todas las pruebas unitarias. O también podemos colocar el cursor dentro de un método de prueba o clase de pruebas específico y usar el **prueba -&gt;ejecución -&gt;pruebas del contexto actual** comando de menú (o tipo Ctrl R, T) para ejecutar un subconjunto de las pruebas unitarias.

Vamos a colocar el cursor dentro de la clase DinnerTest y escriba "Ctrl R, T" para ejecutar las dos pruebas que acabamos de definir. Al hacer esto una ventana de "Resultados de pruebas" aparecerá dentro de Visual Studio y veremos los resultados de nuestra prueba ejecutar enumerados dentro de él:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: La ventana de resultados de pruebas de VS no muestra la columna de nombre de clase de forma predeterminada. Puede agregar esto, haga clic en la ventana Resultados de pruebas y con el comando de menú Agregar o quitar columnas.*

Nuestras dos pruebas tardaron sólo una fracción de segundo para ejecutar – y como se puede ver pasaran. Ahora podemos se enciende y aumentarlas mediante la creación de otras pruebas que compruebe las validaciones de regla concreta, así como para cubren los dos métodos auxiliares - IsUserHost() y IsUserRegisterd(): que se agrega a la clase de la cena. Disponer de todas estas pruebas en el lugar de la clase cena le resultará más fácil y seguro agregar nuevas reglas de negocios y validaciones a él en el futuro. Podemos agregar la lógica de la regla nueva a cena y, a continuación, en segundos, comprobar que no se ha interrumpido cualquiera de nuestra funcionalidad lógica anterior.

Tenga en cuenta cómo utilizando un nombre descriptivo prueba resulta muy sencillo comprender rápidamente lo que consiste en comprobar cada prueba. Recomienda utilizar la **Tools -&gt;opciones** comando de menú, abrir las herramientas de pruebas -&gt;pantalla de configuración de ejecución de pruebas, y comprobando el "haciendo doble clic en un resultado de pruebas unitarias que dé error o no concluyente muestra casilla de verificación el momento del error en la prueba". Esto le permitirá hacer doble clic en un error en la ventana de resultados de pruebas y pasar inmediatamente al error de aserción.

### <a name="creating-dinnerscontroller-unit-tests"></a>Crear pruebas unitarias de DinnersController

Vamos a crear ahora algunas pruebas unitarias que comprueben nuestra funcionalidad DinnersController. Comenzaremos con el botón secundario en la carpeta "Controladores" dentro de nuestro proyecto de prueba y, a continuación, elija la **Add -&gt;nueva prueba** comando de menú. Se creará una "prueba unitaria" y asígnele el nombre "DinnersControllerTest.cs".

Vamos a crear dos métodos de prueba que compruebe el método de acción Details() en el DinnersController. La primera para comprobar que una vista se devuelve cuando se solicita una cena existente. El segundo comprobará que se devuelve una vista de "NotFound" cuando se solicita una cena inexistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

El código anterior se compila de limpieza. Al ejecutar las pruebas, sin embargo, ambas finalizan con errores:

![](enable-automated-unit-testing/_static/image6.png)

Si miramos los mensajes de error, veremos que el motivo por el error de las pruebas era porque nuestra clase DinnersRepository no pudo conectarse a una base de datos. Nuestra aplicación NerdDinner está usando una cadena de conexión en un archivo local de SQL Server Express que reside en el \App\_directorio de datos del proyecto de aplicación NerdDinner. Puesto que nuestro proyecto NerdDinner.Tests compila y ejecuta en un directorio diferente, a continuación, el proyecto de aplicación, la ubicación de ruta de acceso relativa de la cadena de conexión es incorrecta.

Se *podría* solucionar este problema copiando el archivo de base de datos de SQL Express a nuestro proyecto de prueba y, a continuación, agregarle una cadena de conexión de prueba adecuado en el archivo App.config de nuestro proyecto de prueba. Esto podría obtener las pruebas anteriores desbloqueado y ejecuta.

Pruebas unitarias de código con una base de datos real, sin embargo, aporta una serie de desafíos. De manera específica:

- Significativamente ralentiza el tiempo de ejecución de pruebas unitarias. Cuanto más tiempo que se tarda en ejecutar pruebas, menos probable es que se ejecuten con frecuencia. Lo ideal es que desea que las pruebas unitarias para poder ejecutarse en segundos y tiene ser algo de lo que naturalmente como compilar el proyecto.
- Complica la lógica de instalación y limpieza de pruebas. Desea que cada prueba unitaria se aislado y es independiente de otras personas (con sin efectos secundarios o dependencias). Cuando se trabaja con una base de datos real que se debe prestar atención a estado y restablecer entre las pruebas.

Echemos un vistazo a un modelo de diseño que se denomina "inyección de dependencia" que nos ayudan a solucionar estos problemas y evitar la necesidad de usar una base de datos real con nuestras pruebas.

### <a name="dependency-injection"></a>Inyección de dependencia

Ahora DinnersController es "estrechamente" a la clase DinnerRepository. "Acoplamiento" hace referencia a una situación donde una clase explícitamente se basa en otra clase para poder funcionar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Dado que la clase DinnerRepository requiere acceso a una base de datos, la dependencia estrechamente acoplada la clase DinnersController tiene en los extremos de DinnerRepository la necesidad de que tiene una base de datos en orden para los métodos de acción de DinnersController va a probar.

Podemos obtener resolver este problema, utilice un modelo de diseño que se denomina "inyección de dependencia:" que es un enfoque donde dependencias (por ejemplo, las clases de repositorio que proporcionan acceso a datos) ya no es implícitamente se crean dentro de las clases que las utilizan. En su lugar, las dependencias se pueden pasar explícitamente a la clase que usa con argumentos de constructor. Si las dependencias se definen mediante interfaces, a continuación, tenemos la flexibilidad para pasar de las implementaciones de dependencia "falso" para escenarios de prueba unitaria. Esto nos permite crear implementaciones de dependencia específica de la prueba que no se requieren acceso a una base de datos.

Para ver esto en acción, vamos a implementar la inyección de dependencia con nuestro DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraer una interfaz IDinnerRepository

Será el primer paso crear una nueva interfaz IDinnerRepository que encapsula el contrato de repositorio nuestro los controladores que se requieren para recuperar y actualizar cenas.

Podemos definir manualmente este contrato de interfaz con el botón secundario en la carpeta \Models y, a continuación, elija la **Add -&gt;nuevo elemento** comando de menú y la creación de una nueva interfaz denominada IDinnerRepository.cs.

También podemos usar la extracción de refactorización integrada en Visual Studio Professional de herramientas (y las ediciones superiores) para automáticamente y crear una interfaz para que podamos desde nuestra clase DinnerRepository existente. Para extraer esta interfaz con VS, basta con colocar el cursor en el editor de texto en la clase DinnerRepository y, a continuación, haga clic en y elija la **refactorizar -&gt;Extraer interfaz** comando de menú:

![](enable-automated-unit-testing/_static/image7.png)

Esto iniciará el cuadro de diálogo "Extraer interfaz" y nos pedirá el nombre de la interfaz para crear. Se IDinnerRepository de forma predeterminada y seleccionar automáticamente todos los métodos públicos de la clase DinnerRepository existente para agregar a la interfaz:

![](enable-automated-unit-testing/_static/image8.png)

Cuando se haga clic en el botón "Aceptar", Visual Studio agregará una nueva interfaz IDinnerRepository a nuestra aplicación:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Y nuestra clase DinnerRepository existente se actualizará para que implemente la interfaz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Actualizar DinnersController para admitir la inyección de constructor

Ahora, actualizaremos la clase DinnersController para usar la nueva interfaz.

Actualmente DinnersController está codificado de forma rígida forma que el campo "dinnerRepository" siempre es una clase DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Podrá cambiarlo para que el campo "dinnerRepository" es de tipo IDinnerRepository en lugar de DinnerRepository. A continuación, vamos a agregar dos constructores DinnersController públicos. Uno de los constructores permite un IDinnerRepository pasarse como argumento. El otro es un constructor predeterminado que usa la implementación de DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Dado que ASP.NET MVC predeterminada se crea utilizando los constructores predeterminados de las clases de controlador, nuestro DinnersController en tiempo de ejecución continuarán utilizando la clase DinnerRepository tener acceso a datos.

Ahora podemos actualizar nuestras pruebas unitarias, sin embargo, para pasar de una implementación de repositorio cena "falso" mediante el constructor parameter. Este repositorio cena "falso" no necesiten acceder a una base de datos real y en su lugar, utilizará los datos de ejemplo en memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Crear la clase FakeDinnerRepository

Vamos a crear una clase FakeDinnerRepository.

Se le empiece por crear un directorio "Fakes" dentro de nuestro proyecto NerdDinner.Tests y, a continuación, agregue una nueva clase de FakeDinnerRepository a él (haga doble clic en la carpeta y elija **Add -&gt;nueva clase**):

![](enable-automated-unit-testing/_static/image9.png)

Actualizaremos el código para que la clase FakeDinnerRepository implementa la interfaz IDinnerRepository. Podemos, a continuación, haga doble clic en él y elija el comando de menú contextual "Implementar interfaz IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Esto hará que Visual Studio agregar automáticamente todos los miembros de interfaz IDinnerRepository a nuestra clase FakeDinnerRepository con implementaciones de "código auxiliar" predeterminadas:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

A continuación, podemos actualizar la implementación de FakeDinnerRepository para trabajar fuera de una lista en memoria&lt;Dinner&gt; pasado colección como un argumento de constructor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Ahora tenemos una implementación de IDinnerRepository falsa que no requiere una base de datos y, en su lugar, puede trabajar fuera de una lista en memoria de objetos de la cena.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Utilizar la FakeDinnerRepository con pruebas unitarias

Vamos a volver a las pruebas unitarias de DinnersController que dieron error anteriormente porque la base de datos no estaba disponible. Podemos actualizar los métodos de prueba para utilizar un FakeDinnerRepository rellenado con datos de ejemplo en memoria cena a la DinnersController con el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Y ahora al ejecutar estas pruebas pasen:

![](enable-automated-unit-testing/_static/image11.png)

Lo mejor de todo, tomar sólo una fracción de segundo para ejecutar y no requieren ninguna lógica complicada el programa de instalación y limpieza. Podemos ahora la prueba unitaria de todo el código del método de acción DinnersController (lista incluida, paginación, obtener información detallada, crear, actualizar y eliminar) sin necesidad de conectarse a una base de datos real.

| **Lado tema: Marcos de inyección de dependencia** |
| --- |
| Realizar la inserción de dependencias manual (como se están por encima) funciona correctamente, pero se convierten en mantenimiento como el número de dependencias y componentes de una aplicación aumenta. Existen varios marcos de trabajo de inyección de dependencia para .NET que puede ayudar a proporcionar más flexibilidad de administración de dependencia. Estos marcos de trabajo, que también se denomina contenedores "Inversión de Control" (IoC), proporcionan mecanismos que habilitan un nivel adicional de compatibilidad con la configuración para especificar y pasar las dependencias en los objetos en tiempo de ejecución (con mayor frecuencia mediante inyección de constructor ). Algunos de los más populares inyección de dependencia de OSS / incluyen marcos IOC en. NET: AutoFac, Ninject, Spring.NET, StructureMap y Windsor. ASP.NET MVC expone API que permiten a los desarrolladores a participar en la resolución y la creación de instancias de controladores y, lo que permite la inserción de dependencias de extensibilidad / marcos IoC para integrarse correctamente en este proceso. Con el marco de DI/IOC también permitiría que quite el constructor predeterminado de nuestro DinnersController: lo que se quitará por completo el acoplamiento entre él y el DinnerRepositorys. No va a usar una inyección de dependencia / framework IOC con nuestra aplicación NerdDinner. Pero es algo que podríamos consideramos para el futuro si la base de código de NerdDinner y capacidades que ha crecido. |

### <a name="creating-edit-action-unit-tests"></a>Crear pruebas unitarias de acciones de edición

Vamos a crear ahora algunas pruebas unitarias que comprueben la funcionalidad de edición de la DinnersController. Comenzaremos por comprobar la versión de HTTP-GET de la acción de edición:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vamos a crear una prueba que compruebe que se representa una vista respaldada por un objeto DinnerFormViewModel cuando se solicita una cena válida:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Cuando se ejecuta la prueba, sin embargo, buscaremos se produce un error porque se produce una excepción de referencia nula cuando el método Edit tiene acceso a la propiedad User.Identity.Name para realizar la comprobación de Dinner.IsHostedBy().

El objeto de usuario en la clase base del controlador encapsula los detalles sobre el usuario ha iniciado la sesión y se rellena con ASP.NET MVC cuando crea el controlador en tiempo de ejecución. Dado que estamos probando el DinnersController fuera de un entorno de servidor web, no se ha configurado el objeto de usuario (por lo tanto, la referencia nula excepción).

### <a name="mocking-the-useridentityname-property"></a>La propiedad User.Identity.Name de simulación

Marcos de simulación simplificar las pruebas por lo que nos permite crear dinámicamente falsas versiones de los objetos dependientes que admiten nuestras pruebas. Por ejemplo, podemos usar un marco de simulación en nuestra prueba de acción de edición para crear dinámicamente un objeto de usuario que nuestro DinnersController puede usar para buscar un nombre de usuario simulada. Esto evita que una referencia nula desde que se producen al ejecutar la prueba.

Hay muchas .NET marcos que pueden usarse con ASP.NET MVC de simulación (puede ver una lista de las mismas: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Para probar la aplicación de NerdDinner vamos a usar un marco denominado "Moq" de simulación de código abierto, que puede descargarse de forma gratuita desde [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Una vez descargado, vamos a agregar una referencia en el proyecto NerdDinner.Tests al ensamblado Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

A continuación, vamos a agregar un método de aplicación auxiliar de "CreateDinnersControllerAs(username)" a la clase de prueba que toma un nombre de usuario como un parámetro y que, a continuación, "mocks" la propiedad User.Identity.Name en la instancia de DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Anteriormente se está usando Moq para crear un objeto ficticios que fakes un objeto de elemento ControllerContext (que es lo que ASP.NET MVC pasa a las clases de controlador para exponer los objetos en tiempo de ejecución como usuario, la solicitud, respuesta y sesión). Lo llamamos el método "SetupGet" en el simulacro para indicar que la propiedad HttpContext.User.Identity.Name en elemento ControllerContext debe devolver la cadena de nombre de usuario que se pasa al método auxiliar.

Se puede simular cualquier número de elemento ControllerContext propiedades y métodos. Para ilustrar esto también he agregado una llamada SetupGet() para la propiedad Request.IsAuthenticated (que realmente no es necesario para las pruebas siguientes: pero que le ayuda a mostrar cómo puede simular propiedades de la solicitud). Cuando que finalicemos asignamos una instancia del elemento ControllerContext simulacro a la DinnersController devuelve el método auxiliar.

Ahora podemos escribir pruebas unitarias que usan este método auxiliar para probar los escenarios de edición que implican a varios usuarios:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Y ahora al ejecutar las pruebas pasan:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() escenarios de pruebas

Hemos creado pruebas que cubren la versión de HTTP-GET de la acción de edición. Vamos a crear ahora algunas pruebas que comprueban la versión de HTTP POST de la acción de edición:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

El escenario de prueba nueva interesante para poder admitir con este método de acción es su uso del método de aplicación auxiliar UpdateModel() en la clase base del controlador. Estamos usando este método auxiliar para enlazar los valores de formulario post a nuestra instancia de objeto de la cena.

A continuación se muestran dos pruebas que muestra cómo podemos proporcionarle registran valores para el método de aplicación auxiliar UpdateModel() utilizar el formulario. Se podrá hacerlo creando y rellenando un objeto ColecciónFormulario y, a continuación, asignarlo a la propiedad "ValueProvider" en el controlador.

La primera prueba comprueba que en un proceso de guardar correctamente el explorador se redirige a la acción de detalles. La segunda prueba comprueba que cuando se registra una entrada no válida la acción vuelve a mostrar la vista de edición nuevo con un mensaje de error.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Resumen de prueba

Hemos analizado los conceptos principales implicados en las clases de controlador pruebas unitarias. Podemos utilizar estas técnicas para crear fácilmente de cientos de pruebas simples que comprueben el comportamiento de la aplicación.

Dado que nuestro controlador y las pruebas de modelo no requieren una base de datos real, es muy rápida y sencilla para que se ejecute. Se podrá ejecutar cientos de pruebas automatizadas en segundos e inmediatamente después aparece comentarios en cuanto a si un cambio realizados interrumpiera algo. Esto le ayudará a nosotros ofrecen la confianza para mejorar, refactorizar continuamente y refinar nuestra aplicación.

Hemos aprendido pruebas como el último tema en este capítulo, pero no porque pruebas son algo que debe hacer al final de un proceso de desarrollo. Por el contrario, debe escribir pruebas automatizadas tan pronto como sea posible en el proceso de desarrollo. Con ello, podrá obtener información inmediata cuando desarrolle, le ayuda a pensar cuidadosamente en escenarios de casos de uso de la aplicación y le guiará para diseñar la aplicación con limpia disponer en capas y acoplar en cuenta.

Un capítulo posterior en el libro tratará desarrollo controlado por pruebas (TDD) y cómo se usa con ASP.NET MVC. TDD es una práctica de codificación iterativa donde primero tienes que escribir las pruebas que cumplirá el código resultante. Con TDD comenzar cada característica mediante la creación de una prueba que compruebe la funcionalidad que se va a implementar. Escritura de la unidad de pruebas en primer lugar le ayuda a asegurarse de que comprende claramente la característica y cómo se suponía que funcione. Después de la prueba se escribe (y ha comprobado que se produce un error) es, a continuación, implementar la funcionalidad real que comprueba la prueba. Dado que ya ha dedicado tiempo a pensar acerca de cómo la característica es compatible para trabajar en el caso de uso, tendrá una mejor comprensión de los requisitos y la mejor manera de implementarlos. Cuando haya terminado con la implementación, puede volver a ejecutar la prueba: y obtener una respuesta inmediata a si la característica funciona correctamente. Hablaremos sobre TDD más en el capítulo 10.

### <a name="next-step"></a>Paso siguiente

Algunos encapsulado final los comentarios.

>[!div class="step-by-step"]
[Anterior](use-ajax-to-implement-mapping-scenarios.md)
[Siguiente](nerddinner-wrap-up.md)
