---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Crear un modelo con las validaciones de regla de negocios | Documentos de Microsoft
author: microsoft
description: Paso 3 muestra cómo crear un modelo que podemos usar para ambos consultar y actualizar la base de datos de nuestra aplicación NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874946"
---
<a name="build-a-model-with-business-rule-validations"></a>Crear un modelo con las validaciones de regla de negocios
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 3 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 3 muestra cómo crear un modelo que podemos usar para ambos consultar y actualizar la base de datos de nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner paso 3: Generar el modelo

En un marco de trabajo model-view-controller el término "modelo" hace referencia a los objetos que representan los datos de la aplicación, así como la lógica del dominio correspondiente que integra la validación y las reglas de negocios con él. El modelo es de muchas formas "Centro" de una aplicación basada en MVC y como veremos más adelante fundamentalmente controla el comportamiento del mismo.

El marco de ASP.NET MVC admite el uso de cualquier tecnología de acceso a datos y los desarrolladores pueden elegir entre una variedad de opciones enriquecidas de datos de .NET para implementar sus modelos incluidos: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM o ADO sin procesar. NET DataReaders o conjuntos de datos.

Para nuestra aplicación NerdDinner vamos a usar LINQ to SQL para crear un modelo sencillo que corresponde bastante estrechamente a nuestro diseño de base de datos y agrega algunas reglas de negocio y lógica de validación personalizada. A continuación, implementaremos a una clase de repositorio que ayuda a abstracta inmediatamente la implementación de persistencia de datos del resto de la aplicación y permite que nos fácilmente unidad probarlo.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL es un ORM (asignador relacional de objetos) que se incluye como parte de .NET 3.5.

LINQ to SQL proporciona una manera sencilla de tablas de base de datos se asignan a las clases de .NET en que se puede escribir código basado. Para nuestra aplicación NerdDinner vamos a usar para asignar las tablas cenas y RSVP dentro de nuestra base de datos a clases cena y RSVP. Las columnas de las tablas cenas y RSVP se corresponderán con las propiedades de la clase cena y RSVP. Cada objeto cena y RSVP representará una fila independiente dentro de las tablas de cenas o RSVP en la base de datos.

LINQ to SQL permite evitar tener que crear manualmente las instrucciones SQL para recuperar y actualizar cena y RSVP objetos con datos de la base de datos. En su lugar, se definirá las clases cena y RSVP, cómo se asignan a/desde la base de datos y las relaciones entre ellos. LINQ to SQL, a continuación, se toma la atención de la generación de la lógica de ejecución de SQL adecuada que se usará en tiempo de ejecución cuando se interactuar y usarlos.

Podemos usar la compatibilidad de idioma LINQ en C# y VB para escribir más expresivas consultas que recuperan cena y RSVP objetos desde la base de datos. Esto reduce la cantidad de código de datos es necesario para escribir, y nos permite crear aplicaciones realmente limpias.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Agregar clases de LINQ to SQL a nuestro proyecto

Se podrá comenzar con el botón secundario en la carpeta "Models" dentro de nuestro proyecto y seleccione el **Add -&gt;nuevo elemento** comando de menú:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar nuevo elemento". Se podrá filtrar por la categoría de "Datos" y seleccione la plantilla de "Clases de LINQ a SQL" dentro de él:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Comenzaremos nombre del elemento "NerdDinner" y haga clic en el botón "Agregar". Visual Studio agregará un archivo NerdDinner.dbml bajo el directorio \Models y, a continuación, abra el diseñador LINQ to SQL objeto relacional:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Crear clases de modelo de datos con LINQ to SQL

LINQ to SQL permite crear rápidamente clases del modelo de datos de esquema de base de datos existente. Tareas pendientes esta se abra la base de datos de NerdDinner en el Explorador de servidores y seleccione las tablas que desea modelar en ella:

![](build-a-model-with-business-rule-validations/_static/image4.png)

A continuación, podemos arrastrar las tablas en LINQ a la superficie del Diseñador de SQL. Cuando se realiza esta LINQ to SQL crea automáticamente cena y clases RSVP usando el esquema de las tablas (con propiedades de la clase que se asignan a las columnas de tabla de base de datos):

![](build-a-model-with-business-rule-validations/_static/image5.png)

De forma predeterminada, LINQ to SQL diseñador Pluraliza automáticamente "los" nombres de tabla y columna cuando crea clases basadas en un esquema de base de datos. Por ejemplo: la tabla "Cenas" en nuestro ejemplo anterior generó una clase "Cena". Esta clase de nomenclatura facilita nuestros modelos coherente con las convenciones de nomenclatura de .NET y suelen encontrar ese con la corrección de diseñador esto una cómoda (especialmente al agregar una gran cantidad de tablas). Si no le gusta el nombre de una clase o propiedad que genera el diseñador, sin embargo, siempre puede invalidarlo y cambiar a cualquier nombre que desee. Para ello, ya sea mediante la edición de la entidad o la propiedad nombre en línea dentro del diseñador o mediante la modificación a través de la cuadrícula de propiedades.

De forma predeterminada el diseñador LINQ to SQL también inspecciona las relaciones de clave externa y clave principales de las tablas y basadas en ellas automáticamente crea de forma predeterminada "asociaciones de relación" entre las clases de modelo diferente que crea. Por ejemplo, cuando se arrastran el cenas y RSVP tablas en el diseñador LINQ to SQL, una asociación de relación de uno a varios entre los dos se deducen basada en el hecho de que la tabla RSVP tenía una clave externa a la tabla de cenas (Esto se indica mediante la flecha en el Diseñador de):

![](build-a-model-with-business-rule-validations/_static/image6.png)

La asociación anterior hará que LINQ to SQL para agregar una propiedad "Cena" fuertemente tipada a la clase RSVP que los desarrolladores pueden usar para tener acceso a la cena asociada a un determinado RSVP. También hará que la clase cena tener una propiedad de colección "RSVPs" que permite a los desarrolladores recuperar y actualizar los objetos RSVP asociados a una cena determinada.

A continuación puede ver un ejemplo de intellisense en Visual Studio cuando se crea un nuevo objeto RSVP y agregarlo a la colección de confirmaciones de asistencia de la cena. Tenga en cuenta cómo LINQ to SQL agrega automáticamente una colección de "Confirmaciones de asistencia" en el objeto de la cena:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Al agregar el objeto RSVP a la colección de confirmaciones de asistencia de la cena se estamos indica LINQ to SQL para asociar una relación de clave externa entre la cena y la fila RSVP en nuestra base de datos:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si no le gusta cómo el diseñador se modelan o con el nombre de una asociación de la tabla, se puede invalidar. Simplemente haga clic en la flecha de asociación en el diseñador y acceder a sus propiedades a través de la cuadrícula de propiedades para cambiar el nombre, eliminar o modificar. Para nuestra aplicación NerdDinner, sin embargo, las reglas de asociación predeterminado funcionan bien para las clases del modelo de datos que estamos creando y podemos utilizar el comportamiento predeterminado.

### <a name="nerddinnerdatacontext-class"></a>Clase NerdDinnerDataContext

Visual Studio creará automáticamente las clases de .NET que representan los modelos y las relaciones de base de datos definidas mediante el diseñador LINQ to SQL. También se genera una clase de LINQ to SQL DataContext para cada archivo de LINQ to SQL diseñador agregado a la solución. Dado que se denomina nuestro LINQ al elemento de clase SQL "NerdDinner", la clase DataContext creada se llamará "NerdDinnerDataContext". Esta clase NerdDinnerDataContext es la manera principal que interactuamos con la base de datos.

Nuestra clase NerdDinnerDataContext expone dos propiedades: "Cenas" y "RSVPs" - que representan las dos tablas que se modelan dentro de la base de datos. Podemos usar C# para escribir consultas LINQ en las propiedades en la consulta y recuperar objetos cena y RSVP desde la base de datos.

El código siguiente muestra cómo crear una instancia de un objeto NerdDinnerDataContext y realizar una consulta LINQ en ella para obtener una secuencia de cenas que se producen en el futuro. Visual Studio proporciona todas las características intellisense al escribir la consulta LINQ, y los objetos devueltos por el están fuertemente tipadas y también son compatibles con intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Además de lo que nos permite consultar los objetos de la cena y RSVP, un NerdDinnerDataContext también automáticamente realiza un seguimiento de los cambios que realice posteriormente a los objetos de cena y RSVP que recuperamos a través de él. Podemos usar esta funcionalidad para guardar fácilmente los cambios a la base de datos: sin tener que escribir ningún código de actualización SQL explícita.

Por ejemplo, el código siguiente muestra cómo usar una consulta LINQ para recuperar un único objeto de la cena de la base de datos, actualizar dos de las propiedades de la cena y, a continuación, guarde los cambios a la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

El objeto NerdDinnerDataContext en el código anterior realiza automáticamente un seguimiento a los cambios de propiedad realizados en el objeto de la cena que se recuperan de ella. Cuando se llama al método "SubmitChanges()", ejecutará una instrucción "Actualizar" adecuada de SQL a la base de datos para conservar los valores actualizados de nuevo.

### <a name="creating-a-dinnerrepository-class"></a>Crear una clase DinnerRepository

Para las aplicaciones pequeñas a veces es correcto tener controladores de trabajar directamente con una clase de LINQ to SQL DataContext y lo inserte las consultas LINQ dentro de los controladores. Como las aplicaciones obtener mayores, sin embargo, este enfoque se vuelve muy complicada de mantener y probar. También se pueden producir con nosotros duplicar las mismas consultas LINQ en varios lugares.

Un enfoque que puede hacer que las aplicaciones fáciles de mantener y probar es usar un patrón de "repository". Una clase de repositorio le ayuda a encapsular la consulta de datos y lógica de persistencia y resúmenes inmediatamente los detalles de implementación de la persistencia de los datos de la aplicación. Además de hacer que el código de aplicación más limpio, con un modelo de repositorio puede resulte más fácil cambiar las implementaciones de almacenamiento de datos en el futuro y puede que resulte más fácil probar una aplicación sin necesidad de una base de datos real de unidad.

Para nuestra aplicación NerdDinner se definirá una clase DinnerRepository con la siguiente firma:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: Más adelante en este capítulo comenzaremos extraer una interfaz IDinnerRepository de esta clase y habilitar la inserción de dependencias con él en nuestro controladores. Para comenzar con, sin embargo, vamos a iniciar simple y simplemente trabajar directamente con la clase DinnerRepository.*

Para implementar esta clase comenzaremos haga doble clic en la carpeta "Models" y elija la **Add -&gt;nuevo elemento** comando de menú. En el cuadro de diálogo "Agregar nuevo elemento" se podrá seleccionar la plantilla de "Clase" y "DinnerRepository.cs" un nombre al archivo:

![](build-a-model-with-business-rule-validations/_static/image10.png)

A continuación, podemos implementar nuestra clase DinnerRespository con el código siguiente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperar, actualizar, insertar y eliminar el uso de la clase DinnerRepository

Ahora que hemos creado nuestra clase DinnerRepository, veamos algunos ejemplos de código que muestran tareas habituales, que podemos hacer con él:

#### <a name="querying-examples"></a>Ejemplos de consultas

El código siguiente recupera una cena única con el valor de DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

El código siguiente recupera todos los próximos cenas y bucles sobre ellos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Insertar y ejemplos de actualización

El código siguiente muestra cómo agregar dos cenas nueva. Adiciones y modificaciones en el repositorio no están asignadas a la base de datos hasta que se llama al método "Save()" en él. LINQ to SQL ajusta de forma automática todos los cambios en una transacción de base de datos: por lo que todos los cambios se realizan o ninguna de ellas cuando guarda nuestro repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

El código siguiente recupera un objeto existente de la cena y modifica dos propiedades en él. Los cambios se confirman en la base de datos cuando se llama al método "Save()" en nuestro repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

El código siguiente recupera una cena y, a continuación, agrega un RSVP a él. Esto consigue mediante la colección de confirmaciones de asistencia en el objeto de cena que LINQ to SQL crea para que podamos (porque no hay una relación primary key/foreign key entre los dos en la base de datos). Este cambio se conserva en la base de datos como una nueva fila de tabla RSVP cuando se llama al método "Save()" en el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Ejemplo de cómo eliminar

El código siguiente recupera un objeto de la cena existente y, a continuación, lo marca va a eliminar. Cuando se llama al método "Save()" en el repositorio confirmará la eliminación a la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integración de validación y lógica de la regla de negocios con clases de modelo

Integración de validación y regla de negocios lógica es una parte fundamental de cualquier aplicación que funcione con datos.

#### <a name="schema-validation"></a>Validación de esquemas

Cuando se definen las clases de modelo mediante el diseñador LINQ to SQL, los tipos de datos de las propiedades de las clases del modelo de datos se corresponden con los tipos de datos de la tabla de base de datos. Por ejemplo: si la columna de "fecha delevento" en la tabla de cenas es un "datetime", la clase de modelo de datos creada por LINQ to SQL será de tipo "DateTime" (que es un tipo de datos de .NET integrada). Esto significa que se producirán errores de compilación si se intenta asignar un valor entero o un valor booleano a él desde el código y se producirá un error automáticamente si se intenta convertir implícitamente un tipo de cadena no válida a ella en tiempo de ejecución.

LINQ to SQL también automáticamente controla los valores de escape SQL de cuando se usa cadenas - que ayuda a protegerle contra los ataques de inyección de SQL al utilizarlo.

#### <a name="validation-and-business-rule-logic"></a>Validación y lógica de la regla de negocios

La validación del esquema es útil como primer paso, pero basta con poca frecuencia. Mayoría de los escenarios reales requiere la capacidad para especificar una lógica de validación que puede abarcar varias propiedades, ejecutar código y a menudo tiene conocimiento del estado de un modelo (por ejemplo: se crea/actualizar/eliminado, o dentro de un estado específico de dominio al igual que "archivados"). Hay una variedad de patrones diferentes y marcos de trabajo que pueden utilizarse para definir y aplicar reglas de validación a clases del modelo, y hay varias versiones de .NET en función Framework disponible que puede usarse para ayudar a estas. Puede usar más o menos cualquiera de ellos dentro de las aplicaciones de ASP.NET MVC.

Para los fines de nuestra aplicación NerdDinner, vamos a usar un patrón relativamente sencillo y directa donde se exponen una propiedad IsValid y un método GetRuleViolations() en nuestro objeto de modelo de la cena. La propiedad IsValid devolverá true o false, según si la validación y reglas de negocios son válidas. El método GetRuleViolations() devolverá una lista de los errores de la regla.

Implementaremos IsValid y GetRuleViolations() para nuestro modelo cena mediante la adición de una "clase parcial" para el proyecto. Clases parciales pueden usarse para agregar métodos, propiedades y eventos para las clases que se mantiene un diseñador de VS (por ejemplo, la clase de la cena generada por el diseñador LINQ to SQL) y ayuda a evitar la herramienta de informes de los resultados con el código. Podemos agregar una nueva clase parcial para el proyecto con el botón secundario en la carpeta \Models y, a continuación, seleccione el comando de menú "Agregar nuevo elemento". A continuación, podemos elegir la plantilla "Clase" en el cuadro de diálogo "Agregar nuevo elemento" y asígnele el nombre Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Al hacer clic en el botón "Agregar" se agregue un archivo Dinner.cs a nuestro proyecto y ábralo desde el IDE. A continuación, podemos implementar un marco de aplicación básica y validación de la regla mediante el siguiente código:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algunas notas sobre el código anterior:

- La clase cena va precedida por una palabra clave "parcial", lo que significa el código incluido dentro de él se combinarán con la clase genera/mantenerse por el diseñador LINQ to SQL y compila en una sola clase.
- La clase RuleViolation es una clase auxiliar que se agregará al proyecto que nos permite proporcionar más detalles acerca de una infracción de regla.
- Hace que el método Dinner.GetRuleViolations() nuestro validación y reglas de negocios que se debe evaluar (implementaremos ellos en breve). A continuación, devuelve volver una secuencia de objetos RuleViolation que proporcionan más detalles sobre los errores de la regla.
- La propiedad Dinner.IsValid proporciona una propiedad auxiliares útiles que indica si el objeto de la cena cualquier RuleViolations active. Que se puede comprobar de forma proactiva por un desarrollador que utiliza el objeto de la cena en cualquier momento (y no provoca una excepción).
- El método parcial Dinner.OnValidate() es un enlace que LINQ to SQL proporciona que nos permite recibir una notificación cada vez que el objeto de la cena está a punto de conservarse dentro de la base de datos. Nuestra implementación OnValidate() anterior garantiza que la cena no tiene ningún RuleViolations antes de guardarlos. Si se encuentra en un estado no válido produce una excepción, lo que hará que LINQ to SQL se anule la transacción.

Este enfoque proporciona un marco de trabajo simple que se pueden integrar la validación y las reglas de negocios en. Por ahora vamos a agregar las siguientes reglas para el método GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Se está usando la característica "yield return" de C# para devolver una secuencia de cualquier RuleViolations. Las primeras comprobaciones de seis regla anterior simplemente exigen que las propiedades de cadena en nuestra cena no pueden ser nulo ni estar vacío. La última regla es un poco más interesante y llamadas a un método de aplicación auxiliar de PhoneValidator.IsValidNumber() que podemos agregar a nuestro proyecto para comprobar que la Teléfonodecontacto número país de formato coincide con la cena.

Podemos usar. Compatibilidad de expresiones regulares de NET para implementar esta compatibilidad de validación de teléfono. A continuación se muestra una implementación sencilla de PhoneValidator que podemos agregar a nuestro proyecto que nos permite agregar comprobaciones de patrón de expresión regular específico del país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Control de validación y las infracciones de lógica de negocios

Ahora que hemos agregado el código de regla de validación y business anterior, siempre que se intenta crear o actualizar una cena, nuestro reglas de la lógica de validación se evalúan y se aplica.

Los programadores pueden escribir código como a continuación para determinar de forma proactiva si un objeto de la cena es válido y recuperar una lista de todas las infracciones en él sin generar ninguna excepción:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si se intenta guardar una cena en un estado no válido, se producirá una excepción cuando se llama al método Save() en el DinnerRepository. Esto ocurre porque LINQ to SQL llama automáticamente a nuestro método parcial Dinner.OnValidate() antes de que guarda los cambios de la cena, y se agrega código a Dinner.OnValidate() para generar una excepción si existe alguna infracción de regla de la cena. Podemos detecte esta excepción y reactivamente recuperar una lista de las infracciones de corregir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Dado que la validación y reglas de negocios se implementa en nuestro nivel de modelo y no en la capa de interfaz de usuario, se aplica y se utiliza en todos los escenarios de nuestra aplicación. Más adelante podemos cambiar o agregar reglas de negocios y tiene todo el código que funciona con nuestros objetos cena respeta ellos.

Tiene la flexibilidad de cambiar las reglas de negocios en un solo lugar, sin tener que estos cambios ripple a lo largo de la aplicación y la lógica de la interfaz de usuario, es un inicio de sesión de una aplicación bien escrita y una de las ventajas que un marco MVC le ayuda a promover.

### <a name="next-step"></a>Paso siguiente

Ahora tenemos un modelo que podemos usar para consultar y actualizar la base de datos.

Ahora agreguemos algunos controladores y vistas al proyecto que podemos usar para crear una experiencia de interfaz de usuario HTML alrededor de ella.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Siguiente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
