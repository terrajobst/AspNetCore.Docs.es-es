---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar las pruebas unitarias automatizadas | Microsoft Docs
author: microsoft
description: Paso 12 muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que comprueben la funcionalidad de NerdDinner, y que nos proporcionará la confianza para realizar cambios...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 0d7986bcff7a0ddc89d40c3c03eb4ebb26a8b230
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402488"
---
<a name="enable-automated-unit-testing"></a>Habilitar pruebas unitarias automatizadas
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 12 de una segunda oportunidad ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 12 muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que comprueben la funcionalidad de NerdDinner, y que nos proporcionará la confianza para realizar cambios y mejoras a la aplicación en el futuro.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner paso 12: Las pruebas unitarias

Vamos a desarrollar un conjunto de pruebas unitarias automatizadas que comprueben la funcionalidad de NerdDinner, y que nos proporcionará la confianza para realizar cambios y mejoras a la aplicación en el futuro.

### <a name="why-unit-test"></a>¿Por qué prueba unitaria?

En la unidad en el trabajo una mañana tener flash repentino de inspiración acerca de una aplicación que está trabajando. Se dará cuenta que hay que puede implementar un cambio que harán que la aplicación muchísimo mejor. Es posible una refactorización que limpia el código, agrega una nueva característica o corrige un error.

La pregunta al cuando llega al equipo que es: "safe es para realizar esta mejora?" ¿Qué ocurre si realizar el cambio tiene efectos secundarios o interrumpe algo? El cambio podría ser sencillo y tardar solo unos minutos en implementar, pero ¿qué ocurre si tarda horas para probar manualmente todos los escenarios de aplicación? ¿Qué sucede si olvida cubrir un escenario y una aplicación rota entre en producción? ¿Está realizando esta mejora realmente vale la pena todo el esfuerzo?

Pruebas unitarias automatizadas pueden proporcionar una red de seguridad que le permite mejorar continuamente sus aplicaciones y evita deje intimidar por el código que está trabajando. Tener las pruebas automatizadas que comprobación rápidamente funcionalidad le permite codificar con total tranquilidad; y le permiten realizar mejoras que es posible que en caso contrario, no se sientan cómodos haciendo. También ayudan a crear soluciones que son más fáciles de mantener y tienen una duración más larga - lo que lleva a una mayor rentabilidad de la inversión.

El marco de MVC de ASP.NET hace fácil y natural a la funcionalidad de aplicación de pruebas unitarias. También permite un flujo de trabajo de desarrollo de controlado por pruebas (TDD) que permite el desarrollo basado en pruebas previas.

### <a name="nerddinnertests-project"></a>Proyecto NerdDinner.Tests

Cuando se creó nuestra aplicación NerdDinner al principio de este tutorial, nos estábamos se le solicite un cuadro de diálogo que pregunta si deseamos crear un proyecto de prueba unitaria para continuar con el proyecto de aplicación:

![](enable-automated-unit-testing/_static/image1.png)

Se mantiene el "Sí, crear un proyecto de prueba unitaria" botón de radio seleccionado: que dieron lugar a un proyecto de "NerdDinner.Tests" que se va a agregar a nuestra solución:

![](enable-automated-unit-testing/_static/image2.png)

El proyecto NerdDinner.Tests hace referencia al ensamblado de proyecto de aplicación NerdDinner y nos permite agregar fácilmente las pruebas automatizadas que comprobar la funcionalidad de la aplicación.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Crear pruebas unitarias para la clase de modelo cena

Vamos a agregar algunas pruebas para nuestro proyecto NerdDinner.Tests que comprueban la clase Dinner que hemos creado cuando creamos nuestro nivel de modelo.

Comenzaremos por crear una carpeta dentro de nuestro proyecto de prueba llamada "Models" donde se colocarán las pruebas relacionadas con el modelo. A continuación, se le haga doble clic en la carpeta y elija el **Add -&gt;nueva prueba** comando de menú. Se abrirá el cuadro de diálogo "Agregar nueva prueba".

Decidimos crear una "prueba unitaria" y asígnele el nombre "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Cuando se haga clic en el botón "Aceptar" Visual Studio agregará (y abrir) un archivo DinnerTest.cs al proyecto:

![](enable-automated-unit-testing/_static/image4.png)

La plantilla de prueba unitaria de forma predeterminada Visual Studio tiene una serie de la cantidad de código reutilizable en lo que considero un tanto lioso. Vamos a limpiarlo simplemente contenga el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

El atributo [TestClass] en la clase DinnerTest anterior lo identifica como una clase que contendrá las pruebas, así como inicialización de la prueba opcional y el código de desmontaje. Podemos definir pruebas dentro de él mediante la adición de métodos públicos que tienen un atributo [TestMethod] sobre ellos.

A continuación se muestran en el primero de dos pruebas que actúan sobre nuestra clase Dinner vamos a agregar. La primera prueba comprueba que nuestro Dinner no es válida si se crea una nueva cena sin todas las propiedades que se establece correctamente. La segunda prueba comprueba que nuestro Dinner es válida cuando una cena tiene todas las propiedades establecidas por los valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Anteriormente se dará cuenta que los nombres de prueba son muy explícitas (y detallada). Hacemos esto porque se podría acabar crear cientos o miles de pruebas de pequeñas y queremos que sea fácil de determinar rápidamente la intención y el comportamiento de cada uno de ellos (especialmente cuando buscamos a través de una lista de errores en un ejecutor de pruebas). Los nombres de prueba deben llamarse después de la funcionalidad que se están probando. Anteriormente, usamos un "sustantivo\_debe\_verbo" patrón de nomenclatura.

Nos estamos estructurar las pruebas con pruebas de patrón, que es el acrónimo "Arrange, Act, Assert" "AAA":

- Organizar: Configurar la unidad que se está probando
- ACT: Ejecute la unidad sometida a prueba y capturar los resultados
- Aserción: Comprobar el comportamiento

Cuando escribimos las pruebas que desea evitar que las pruebas individuales hacer demasiado. En su lugar, cada prueba debe comprobar solo un único concepto (que le resultará mucho más fácil de identificar la causa de errores). Una buena directriz es intentar tener sólo una única instrucción para cada prueba de aserción. Si tiene más de una instrucción en un método de prueba de aserción, asegúrese de que todos los que se sirven para probar el mismo concepto. En caso de duda, realice otra prueba.

### <a name="running-tests"></a>Ejecutando pruebas

Visual Studio 2008 Professional (y las ediciones superiores) incluyen un ejecutor de pruebas integradas que puede usarse para ejecutar proyectos de prueba unitaria de Visual Studio en el IDE. Podemos seleccionar el **Test -&gt;ejecución -&gt;todas las pruebas de la solución** comando de menú (o tipo Ctrl R, A) para ejecutar todas nuestras pruebas unitarias. O también podemos colocar el cursor dentro de un método de prueba o clase de prueba concreta y usar el **Test -&gt;ejecución -&gt;pruebas del contexto actual** comando de menú (o escriba Ctrl R, T) para ejecutar un subconjunto de las pruebas unitarias.

Vamos a colocar el cursor dentro de la clase DinnerTest y escriba "Ctrl R, T" para ejecutar las dos pruebas que acabamos de definir. Cuando lo hacemos una ventana "Resultados de pruebas" aparecerá dentro de Visual Studio y veremos los resultados de nuestra prueba ejecute enumerados dentro de él:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: La ventana de resultados de pruebas de VS no muestra la columna de nombre de clase de forma predeterminada. Se puede agregar con el botón secundario en la ventana Resultados de pruebas y usando el comando de menú Agregar o quitar columnas.*

Las dos pruebas duraban sólo una fracción de segundo para ejecutar y como se puede ver pasados. Ahora podemos se encienden y aumentarlas mediante la creación de pruebas adicionales que comprobación las validaciones de regla concreta, así como para cubren los dos métodos auxiliares - IsUserHost() y IsUserRegisterd(): que se han agregado a la clase Dinner. Con todas estas pruebas en su lugar para la clase Dinner resultará más fácil y seguro agregar nuevas reglas de negocios y validaciones a él en el futuro. Podemos agregar nuestra lógica de la regla nueva a cenar y, a continuación, en cuestión de segundos, comprobar que no se ha interrumpido cualquiera de nuestra funcionalidad lógica anterior.

Tenga en cuenta cómo utilizando un nombre descriptivo prueba facilita comprender rápidamente lo que está comprobando cada prueba. Recomienda usar la **Tools -&gt;opciones** comando de menú, abrir las herramientas de pruebas -&gt;pantalla de configuración de ejecución de prueba y comprobar el "doble clic en un resultado de prueba unitaria con error o no concluyente muestra casilla de verificación de punto de error en la prueba". Esto le permitirá hacer doble clic en un error en la ventana de resultados de pruebas y pasar inmediatamente al error de aserción.

### <a name="creating-dinnerscontroller-unit-tests"></a>Crear pruebas unitarias DinnersController

Ahora creemos algunas pruebas unitarias que comprueben la funcionalidad de DinnersController. Comenzaremos con el botón secundario en la carpeta "Controladores" dentro de nuestro proyecto de prueba y, a continuación, elija el **Add -&gt;nueva prueba** comando de menú. Crearemos una "prueba unitaria" y asígnele el nombre "DinnersControllerTest.cs".

Vamos a crear dos métodos de prueba que comprueben el método de acción Details() en el DinnersController. La primera comprobará que se devuelve una vista cuando se solicita una cena existente. El segundo comprobará que se devuelve una vista de "NotFound" cuando se solicita una cena inexistente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

El código anterior se compila limpia. Al ejecutar las pruebas, sin embargo, ambas finalizan con errores:

![](enable-automated-unit-testing/_static/image6.png)

Si miramos los mensajes de error, veremos que el motivo de error de las pruebas era porque nuestra clase DinnersRepository no pudo conectarse a una base de datos. Nuestra aplicación NerdDinner usa una cadena de conexión en un archivo local de SQL Server Express que se encuentra bajo el \App\_directorio de datos del proyecto de aplicación NerdDinner. Dado que nuestro proyecto NerdDinner.Tests compila y ejecuta en un directorio diferente, a continuación, el proyecto de aplicación, la ubicación de ruta de acceso relativa de nuestra cadena de conexión es correcta.

Nos *podría* solucionar este problema copiando el archivo de base de datos de SQL Express en nuestro proyecto de prueba y, a continuación, agregarle una cadena de conexión de prueba adecuada en el archivo App.config de nuestro proyecto de prueba. Esto obtendría las pruebas anteriores desbloqueado y ejecuta.

Pruebas unitarias de código mediante una base de datos real, sin embargo, trae consigo una serie de desafíos. De manera específica:

- Ralentiza considerablemente el tiempo de ejecución de pruebas unitarias. Cuanto más tiempo tarda en ejecutarse las pruebas, menos probable es que se ejecuten con frecuencia. Lo ideal es que desea que las pruebas unitarias para poder ejecutarse en segundos y que sea algo que se hace como naturalmente como compilar el proyecto.
- Complica la lógica del programa de instalación y limpieza dentro de las pruebas. Desea que cada prueba unitaria se aislada e independiente de otros usuarios (sin efectos secundarios o con dependencias). Cuando se trabaja con una base de datos real, debe ser consciente del estado y restablecerlo entre las pruebas.

Echemos un vistazo a un patrón de diseño denominado "inserción de dependencias" que puede ayudarnos a solucionar estos problemas y evitar la necesidad de usar una base de datos real con nuestras pruebas.

### <a name="dependency-injection"></a>Inserción de dependencias

Ahora mismo DinnersController está "estrechamente" a la clase de instancia DinnerRepository. "Acoplamiento" hace referencia a una situación donde una clase explícitamente se basa en otra clase para poder funcionar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Dado que la clase de instancia DinnerRepository requiere acceso a una base de datos, la dependencia estrechamente acoplada tiene la clase DinnersController en la instancia DinnerRepository termina la necesidad de que tener una base de datos para los métodos de acción DinnersController va a probar.

Podemos solucionar este problema mediante el uso de un patrón de diseño denominado "inserción de dependencias", que es un enfoque donde las dependencias (como las clases de repositorio que proporcionan acceso a datos) ya no es implícitamente se crean dentro de las clases que los usan. En su lugar, las dependencias se pueden pasar explícitamente a la clase que se usa con argumentos de constructor. Si las dependencias se definen mediante las interfaces, a continuación, tenemos la flexibilidad necesaria para pasar las implementaciones de dependencia "falsos" para escenarios de pruebas unitarias. Esto nos permite crear implementaciones de dependencia específica de la prueba que no se requieren acceso a una base de datos.

Para ver esto en acción, vamos a implementar la inserción de dependencias con nuestra DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraer una interfaz IDinnerRepository

El primer paso será crear una nueva interfaz de IDinnerRepository que encapsula el contrato del repositorio que requieren nuestros controladores para recuperar y actualizar las cenas.

Podemos definir este contrato de interfaz manualmente, con el botón secundario en la carpeta \Models y, a continuación, elija el **Add -&gt;nuevo elemento** comando de menú y la creación de una nueva interfaz denominada IDinnerRepository.cs.

También podemos usar la refactorización automáticamente herramientas integrados en Visual Studio Professional (y las ediciones superiores) para extraer y crear una interfaz para que podamos de nuestra clase DinnerRepository existente. Para extraer esta interfaz con VS, simplemente coloque el cursor en el editor de texto en la clase de instancia DinnerRepository y, a continuación, haga clic en y elija el **refactorizar -&gt;Extraer interfaz** comando de menú:

![](enable-automated-unit-testing/_static/image7.png)

Esto iniciará el cuadro de diálogo "Extraer interfaz" y nos solicitará el nombre de la interfaz para crear. Se IDinnerRepository de forma predeterminada y seleccionar automáticamente todos los métodos públicos en la clase de instancia DinnerRepository existente para agregar a la interfaz:

![](enable-automated-unit-testing/_static/image8.png)

Cuando hacemos clic en el botón "Aceptar", Visual Studio agregará una nueva interfaz de IDinnerRepository a nuestra aplicación:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Y nuestra clase DinnerRepository existente se actualizará para que implemente la interfaz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Actualizando DinnersController para admitir la inserción de constructor

Ahora, actualizaremos la clase DinnersController para usar la nueva interfaz.

Actualmente DinnersController está codificado de forma rígida que su campo "dinnerRepository" siempre es una clase de instancia DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Podrá cambiarlo para que el campo "dinnerRepository" es de tipo IDinnerRepository en lugar de la instancia DinnerRepository. A continuación, vamos a agregar dos constructores públicos de DinnersController. Uno de los constructores permite una instancia IDinnerRepository pasarse como argumento. El otro es un constructor predeterminado que usa nuestra implementación DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Dado que ASP.NET MVC predeterminada se crea utilizando los constructores predeterminados de las clases de controlador, nuestro DinnersController en tiempo de ejecución continuará usar la clase de instancia DinnerRepository para tener acceso a datos.

Ahora podemos actualizar nuestro pruebas unitarias, sin embargo, para pasar una implementación de repositorio cena "falsos" con el parámetro constructor. Este repositorio cena "falsos" no requerirá acceso a una base de datos real y en su lugar, usará los datos de ejemplo en memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Creación de la clase FakeDinnerRepository

Vamos a crear una clase FakeDinnerRepository.

Crearemos empiece por crear un directorio "Fakes" dentro de nuestro proyecto NerdDinner.Tests y, a continuación, agregaremos una nueva clase FakeDinnerRepository a él (haga doble clic en la carpeta y elija **Add -&gt;nueva clase**):

![](enable-automated-unit-testing/_static/image9.png)

Actualizaremos el código para que la clase FakeDinnerRepository implementa la interfaz de IDinnerRepository. Podemos, a continuación, haga doble clic en él y elija el comando del menú contextual "Implementar interfaz IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Esto hará que Visual Studio agregar automáticamente todos los miembros de interfaz de IDinnerRepository a nuestra clase FakeDinnerRepository con implementaciones de "código auxiliar de" valor predeterminado:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

A continuación, podemos actualizar la implementación FakeDinnerRepository trabajar fuera de una lista en memoria&lt;Dinner&gt; recopilación le pasa como argumento de constructor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Ahora tenemos una implementación falsa de IDinnerRepository que no requiere una base de datos y, en su lugar, puede trabajar fuera de una lista en memoria de objetos de la cena.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Utilizar el FakeDinnerRepository con pruebas unitarias

Vamos a volver a las pruebas unitarias de DinnersController con error anteriormente debido a la base de datos no estaba disponible. Podemos actualizar los métodos de prueba para utilizar un FakeDinnerRepository rellenado con datos de ejemplo en memoria cena el DinnersController con el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Y ahora al ejecutar estas pruebas pasen:

![](enable-automated-unit-testing/_static/image11.png)

Lo mejor de todo, tomar sólo una fracción de segundo para ejecutar y no requieren ninguna lógica complicada configuración o limpieza. Podemos ahora la prueba unitaria todo nuestro código de método de acción DinnersController (lista incluida, paginación, detalles, crear, actualizar y eliminar) sin necesidad de conectarse a una base de datos real.

| **Del lado de tema: Marcos de inserción de dependencias** |
| --- |
| Realizar la inserción de dependencias de manual (por ejemplo, se están por encima) funciona bien, pero se vuelven más difícil de mantener como el número de dependencias y los componentes de una aplicación aumenta. Existen varios marcos de inserción de dependencia para .NET que puede ayudar a proporcionar mayor flexibilidad de administración de dependencias. Estos marcos, a veces se denominan contenedores "Inversión de Control" (IoC), proporcionan mecanismos que permiten un nivel adicional de compatibilidad con la configuración para especificar y pasar las dependencias a objetos en tiempo de ejecución (con mayor frecuencia mediante inserción de constructor ). Algunas de la inserción de dependencias más populares de OSS / incluyen marcos IOC en. NET: Windsor, Ninject, Spring.NET, StructureMap y AutoFac. ASP.NET MVC expone la API que permiten a los desarrolladores participar en la resolución y la creación de instancias de los controladores y, lo que permite la inserción de dependencias de extensibilidad / marcos IoC para integrarse sin problemas dentro de este proceso. Con el marco DI/IOC sería también nos permiten quitar el constructor predeterminado de nuestro DinnersController – que desea quitar completamente el acoplamiento entre ella y la DinnerRepositorys. No se usará con una inserción de dependencias / marco IOC con nuestra aplicación NerdDinner. Pero es algo que podemos considerar para el futuro si las capacidades y la base de código de NerdDinner ha crecido. |

### <a name="creating-edit-action-unit-tests"></a>Crear pruebas unitarias de acción de edición

Ahora creemos algunas pruebas unitarias que comprueben la funcionalidad de edición de la DinnersController. Comenzaremos por comprobar la versión de HTTP-GET de la acción de edición:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vamos a crear una prueba que comprueba que se representa una vista respaldada por un objeto DinnerFormViewModel cuando se solicita una cena válida:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Cuando se ejecuta la prueba, sin embargo, se encontrará que se produce un error porque se produce una excepción de referencia nula cuando el método Edit tiene acceso a la propiedad User.Identity.Name para realizar la comprobación de Dinner.IsHostedBy().

El objeto de usuario en la clase base del controlador encapsula los detalles sobre el usuario ha iniciado sesión y se rellena con ASP.NET MVC cuando crea el controlador en tiempo de ejecución. Dado que estamos probando la DinnersController fuera de un entorno de servidor web, no se establece el objeto de usuario (por lo tanto, la excepción de referencia null).

### <a name="mocking-the-useridentityname-property"></a>La propiedad User.Identity.Name de simulación

Marcos de simulación simplificar las pruebas por lo que nos permite crear dinámicamente falsas versiones de los objetos dependientes que admiten nuestras pruebas. Por ejemplo, podemos usar un marco de simulación en nuestra prueba de acción de edición para crear dinámicamente un objeto de usuario que puede usar nuestro DinnersController para buscar un nombre de usuario simulado. Esto evita una referencia nula que se produzca cuando ejecutemos nuestra prueba.

Hay muchos .NET marcos que pueden utilizarse con ASP.NET MVC de simulación (puede ver una lista de ellos: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Para probar nuestra aplicación NerdDinner, vamos a usar un marco denominado "Moq" de simulación de código abierto, que puede descargarse de forma gratuita desde [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Una vez descargado, vamos a agregar una referencia en nuestro proyecto NerdDinner.Tests al ensamblado Moq.dll:

![](enable-automated-unit-testing/_static/image12.png)

Método auxiliar "CreateDinnersControllerAs(username)", a continuación, vamos a agregar a nuestra clase de prueba que toma un nombre de usuario como un parámetro y que, a continuación, "mocks" la propiedad User.Identity.Name en la instancia de DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Estamos usando Moq anteriormente para crear un objeto de simulacro que un objeto de elemento ControllerContext (que es lo que ASP.NET MVC que se pasa a las clases de controlador para exponer objetos en tiempo de ejecución como usuario, solicitud, respuesta y sesión) de fakes. Lo llamamos el método "SetupGet" en el simulacro para indicar que la propiedad HttpContext.User.Identity.Name en el elemento ControllerContext debe devolver la cadena de nombre de usuario que se pasa al método auxiliar.

Nos podemos simular cualquier número de métodos y propiedades de elemento ControllerContext. Para ilustrar esto también agregué una llamada SetupGet() para la propiedad Request.IsAuthenticated (que realmente no es necesario para las pruebas siguientes: pero que puede ayudarle a ilustrar cómo se pueden simular propiedades de la solicitud). Cuando terminemos asignamos una instancia de la simulación de elemento ControllerContext a la DinnersController devuelve el método auxiliar.

Ahora podemos escribir pruebas unitarias que usan este método auxiliar para probar los escenarios de edición que implican a distintos usuarios:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Y ahora al ejecutar las pruebas pasan:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() escenarios de pruebas

Hemos creado las pruebas que abarcan la versión de HTTP-GET de la acción de edición. Ahora, vamos a crear algunas pruebas que comprueben la versión de HTTP POST de la acción de edición:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

El escenario de prueba nuevo interesante soporte con este método de acción es su uso del método auxiliar UpdateModel() en la clase base del controlador. Estamos usando este método auxiliar para enlazar los valores de formulario post a nuestra instancia de objeto de la cena.

A continuación se muestran dos pruebas que se muestra cómo podemos proporcionarle registrado los valores para el método auxiliar UpdateModel() utilizar el formulario. Se deberá hacer esto creando y rellenando un objeto ColecciónFormulario y, a continuación, asignarlo a la propiedad "ValueProvider" en el controlador.

La primera prueba comprueba que en una operación de guardar correctamente el explorador se redirige a la acción de detalles. La segunda prueba comprueba que cuando se publica la entrada no es válida la acción vuelve a mostrar la vista de edición nuevo con un mensaje de error.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Resumen de pruebas

Nos hemos tratado los conceptos principales implicados en las clases del controlador de pruebas unitarias. Podemos usar estas técnicas para crear fácilmente cientos de pruebas simples que comprueben el comportamiento de nuestra aplicación.

Dado que nuestro controlador y las pruebas de modelo no requieren una base de datos real, son extremadamente rápida y fácil de ejecutar. Podremos ejecutar cientos de pruebas automatizadas en segundos y obtener comentarios sobre si se realizó un cambio interrumpió algo de inmediatamente. Esto le ayudará a Proporciónenos la confianza necesaria para mejorar, refactorizar continuamente y refinar la aplicación.

Se tratan las pruebas como el último tema en este capítulo, pero no porque las pruebas son algo que debe hacer al final de un proceso de desarrollo! Por el contrario, debe escribir pruebas automatizadas tan pronto como sea posible en el proceso de desarrollo. Si lo hace por lo que le permite obtener una respuesta inmediata a medida que desarrolle, le ayuda a pensar cuidadosamente en escenarios de casos de uso de la aplicación y le guía para diseñar la aplicación con limpia la creación de capas y de acoplamiento en mente.

Desarrollo controlado por pruebas (TDD) y cómo usarlo con ASP.NET MVC, veremos un capítulo posterior en el libro. TDD es una práctica de codificación iterativa donde se escriben las pruebas que cumplirá el código resultante. Con TDD comenzar cada característica mediante la creación de una prueba que comprueba la funcionalidad que se va a implementar. Escritura de la unidad de prueba primero le ayuda a asegurarse de que comprende claramente la característica y cómo se supone que funcione. Después de la prueba se escribe (y ha comprobado que se produce un error) es, a continuación, implementar la funcionalidad real de que la prueba comprueba. Dado que ya ha invertido tiempo a pensar acerca de cómo se supone que la característica funciona en el caso de uso, tendrá una mejor comprensión de los requisitos y la mejor manera de implementarlos. Cuando haya terminado con la implementación, puede volver a ejecutar la prueba – y reciben una respuesta inmediata a si la característica funciona correctamente. Hablaremos sobre TDD más en el capítulo 10.

### <a name="next-step"></a>Paso siguiente

Algunos encapsulado final de los comentarios.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-implement-mapping-scenarios.md)
> [Siguiente](nerddinner-wrap-up.md)
