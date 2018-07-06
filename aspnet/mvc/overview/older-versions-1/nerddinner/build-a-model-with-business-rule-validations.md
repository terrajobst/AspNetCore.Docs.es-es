---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Crear un modelo con validaciones de regla de negocio | Microsoft Docs
author: microsoft
description: Paso 3 muestra cómo crear un modelo que podemos usar para ambas consultas y actualizar la base de datos para nuestra aplicación NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 4bed4dd794c7c34551cd3c7543e08ed12d83505a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836780"
---
<a name="build-a-model-with-business-rule-validations"></a>Crear un modelo con validaciones de regla de negocio
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 3 de una segunda oportunidad ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 3 muestra cómo crear un modelo que podemos usar para ambas consultas y actualizar la base de datos para nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner paso 3: Creación del modelo

En un marco model-view-controller el término "modelo" hace referencia a los objetos que representan los datos de la aplicación, así como la lógica del dominio correspondiente que integra la validación y las reglas de negocios con él. El modelo es de muchas maneras el "corazón" de una aplicación basada en MVC y, como veremos más adelante fundamentalmente controla el comportamiento del mismo.

El marco de ASP.NET MVC admite el uso de cualquier tecnología de acceso a datos y los desarrolladores pueden elegir entre una variedad de opciones enriquecidas de datos .NET para implementar sus modelos incluidos: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM o ADO sin procesar. NET DataReaders o conjuntos de datos.

Para nuestra aplicación NerdDinner, vamos a usar LINQ to SQL para crear un modelo sencillo que bastante estrechamente corresponde a nuestro diseño de base de datos y agrega algunas reglas de negocio y la lógica de validación personalizada. A continuación, implementaremos a una clase de repositorio que ayuda a distancia abstracta la implementación de persistencia de datos del resto de la aplicación y permite que nos fácilmente la unidad de prueba.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL es un ORM (asignador relacional de objetos) que se incluye como parte de .NET 3.5.

LINQ to SQL proporciona una manera sencilla de las tablas de base de datos se asignan a las clases de .NET que se puede codificar. Para nuestra aplicación NerdDinner vamos a usar para asignar las instancias Dinners y RSVP tablas dentro de nuestra base de datos a las clases de Dinner y RSVP. Las columnas de las tablas Dinners y RSVP se corresponden con las propiedades de la clase Dinner y RSVP. Cada objeto Dinner y RSVP representará una fila independiente dentro de las instancias Dinners o RSVP tablas en la base de datos.

LINQ to SQL permite evitar tener que construir manualmente las instrucciones SQL para recuperar y actualizar Dinner y RSVP objetos con la base de datos. En su lugar, definiremos las clases de Dinner y RSVP, cómo se asignan a/desde la base de datos y las relaciones entre ellos. LINQ to SQL, a continuación, se toma la atención de generar la lógica de ejecución de SQL adecuada para usar en tiempo de ejecución cuando se interactúe y usarlos.

Podemos usar la compatibilidad de idioma LINQ en VB y C# para escribir consultas expresivas que recuperen Dinner y RSVP objetos desde la base de datos. Esto minimiza la cantidad de código de datos que necesitamos escribir, y nos permite crear aplicaciones muy limpias.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Agregar clases de LINQ to SQL a nuestro proyecto

Nos comenzar con el botón secundario en la carpeta "Models" dentro de nuestro proyecto y seleccione el **Add -&gt;nuevo elemento** comando de menú:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar nuevo elemento". Se podrá filtrar por la categoría de "Datos" y seleccione la plantilla de "Clases de LINQ a SQL" dentro de él:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Se le asigne un nombre del elemento "NerdDinner" y haga clic en el botón "Agregar". Visual Studio agregará un archivo NerdDinner.dbml bajo nuestro directorio \Models y, a continuación, abra el diseñador LINQ to SQL objeto relacional:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Crear clases de modelo de datos con LINQ to SQL

LINQ to SQL permite crear rápidamente clases de modelo de datos de esquema de base de datos existente. Tareas pendientes esto, abra la base de datos de NerdDinner en el Explorador de servidores y seleccione las tablas que desea modelar en ella:

![](build-a-model-with-business-rule-validations/_static/image4.png)

A continuación, podemos arrastrar las tablas en LINQ a la superficie del Diseñador de SQL. Cuando hacemos esta LINQ to SQL crea automáticamente Dinner y clases RSVP mediante el esquema de las tablas (con propiedades de la clase que se asignan a las columnas de tabla de base de datos):

![](build-a-model-with-business-rule-validations/_static/image5.png)

De forma predeterminada, LINQ to SQL diseñador automáticamente "en singular" nombres de tabla y columna cuando crea clases basadas en un esquema de base de datos. Por ejemplo: dio como resultado de la tabla "Dinners" en nuestro ejemplo anterior en una clase "Comida". Esta clase de nomenclatura le ayuda a realizar nuestros modelos coherente con las convenciones de nomenclatura de .NET y suelen encontrar ese tenerlo la corrección del Diseñador de conveniente (especialmente al agregar una gran cantidad de tablas). Si no le gusta el nombre de una clase o propiedad que genera el diseñador, sin embargo, siempre puede invalidarlo y cambiarla a cualquier nombre que desee. Para ello, ya sea mediante la edición de la entidad o la propiedad nombre en línea dentro del diseñador o modificando a través de la cuadrícula de propiedades.

De forma predeterminada, el diseñador LINQ to SQL también inspecciona las relaciones de clave externa y clave principales de las tablas y basadas en ellas automáticamente crea de forma predeterminada "las asociaciones de relación" entre las clases de modelo diferente, que crea. Por ejemplo, cuando se arrastran el Dinners y RSVP tablas en el diseñador LINQ to SQL, una asociación de la relación de uno a varios entre los dos dedujo basada en el hecho de que la tabla RSVP tenía una clave externa a la tabla de instancias Dinners (Esto se indica mediante la flecha en el diseñador):

![](build-a-model-with-business-rule-validations/_static/image6.png)

La asociación anterior provocará LINQ to SQL para agregar una propiedad de "Cena" fuertemente tipada a la clase RSVP que los desarrolladores pueden usar para tener acceso a la cena asociada con un determinado RSVP. También hará que la clase Dinner tengan una propiedad de colección "RSVPs" que permite a los desarrolladores recuperar y actualizar los objetos RSVP asociados a una cena en particular.

A continuación puede ver un ejemplo de intellisense en Visual Studio cuando se cree un nuevo objeto de confirmación de asistencia y agregarlo a la colección de confirmaciones de asistencia de la cena. Tenga en cuenta cómo LINQ to SQL agrega automáticamente una colección de "Confirmaciones de asistencia" en el objeto de la cena:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Agregando el objeto de confirmación de asistencia a la colección de confirmaciones de asistencia de la cena indicamos a LINQ to SQL para asociar una relación de clave externa entre la cena y la fila RSVP en nuestra base de datos:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si no le gusta cómo el diseñador tiene modelar o con el nombre de una asociación de la tabla, se puede reemplazar. Simplemente haga clic en la flecha de la asociación en el diseñador y acceder a sus propiedades a través de la cuadrícula de propiedades para cambiar el nombre, eliminar o modificar. Para nuestra aplicación NerdDinner, sin embargo, las reglas de asociación predeterminadas funcionan bien para las clases del modelo de datos que estamos creando y podemos utilizar el comportamiento predeterminado.

### <a name="nerddinnerdatacontext-class"></a>Clase NerdDinnerDataContext

Visual Studio creará automáticamente las clases de .NET que representan los modelos y las relaciones de base de datos definidas mediante el diseñador LINQ to SQL. También se genera una clase LINQ to SQL DataContext para cada archivo LINQ to SQL diseñador agregado a la solución. Dado que hemos llamado a nuestro LINQ al elemento de clase SQL "NerdDinner", se llamará a la clase DataContext creada "NerdDinnerDataContext". Esta clase NerdDinnerDataContext es la principal manera que interactúan con la base de datos.

Nuestra clase NerdDinnerDataContext expone dos propiedades: "Dinners" y "RSVPs -" que representan las dos tablas que se modelan dentro de la base de datos. Podemos usar C# para escribir consultas LINQ en esas propiedades a los objetos de la cena y RSVP consulta y recuperar desde la base de datos.

El código siguiente muestra cómo crear una instancia de un objeto NerdDinnerDataContext y realizar una consulta LINQ en él para obtener una secuencia de instancias Dinners que se producen en el futuro. Visual Studio proporciona intellisense completo al escribir la consulta LINQ, y los objetos devueltos desde la están fuertemente tipados y también admiten intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Además de lo que nos permite consultar los objetos de la cena y RSVP, un NerdDinnerDataContext también automáticamente realiza un seguimiento de los cambios que realice posteriormente a los objetos de la cena y RSVP que recuperamos a través de él. Podemos usar esta funcionalidad para guardar fácilmente los cambios en la base de datos: sin tener que escribir ningún código explícito de actualización SQL.

Por ejemplo, el código siguiente muestra cómo usar una consulta LINQ para recuperar un único objeto de la cena de la base de datos, dos de las propiedades de la cena actualizar y, a continuación, guarde los cambios en la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

El objeto NerdDinnerDataContext en el código anterior automáticamente realiza un seguimiento de los cambios de propiedad realizados en el objeto de la cena que se puede recuperar de ella. Cuando se llama al método "SubmitChanges()", ejecutará una instrucción SQL apropiada "Actualizar" para conservar los valores actualizados de nuevo la base de datos.

### <a name="creating-a-dinnerrepository-class"></a>Creación de una clase de instancia DinnerRepository

Para las aplicaciones pequeñas a veces es correcto tener controladores de trabajar directamente con una clase LINQ to SQL DataContext e inserción de las consultas LINQ dentro de los controladores. Como las aplicaciones obtener mayores, sin embargo, este enfoque pasa a ser difícil de mantener y probar. También puede provocar que nos duplicar las mismas consultas LINQ en varios lugares.

Un enfoque que puede hacer que las aplicaciones fáciles de mantener y probar es usar un patrón "repositorio". Una clase de repositorio permite encapsular la consulta de datos y lógica de persistencia y abstraen los detalles de implementación de la persistencia de datos de la aplicación. Además de hacer que el código de aplicación más limpio, con un patrón de repositorio puede facilitar cambiar en el futuro las implementaciones de almacenamiento de datos y puede ayudar a facilitar la unidad de probar una aplicación sin necesidad de una base de datos real.

Para nuestra aplicación NerdDinner definiremos una clase de instancia DinnerRepository con la siguiente firma:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: Más adelante en este capítulo analizaremos extraer una interfaz IDinnerRepository de esta clase y habilitar la inserción de dependencias con él en nuestros controladores. Para empezar, sin embargo, vamos a iniciar simple y simplemente funcionan directamente con la clase de instancia DinnerRepository.*

Para implementar esta clase se haga doble clic en la carpeta "Models" y elija el **Add -&gt;nuevo elemento** comando de menú. En el cuadro de diálogo "Agregar nuevo elemento" seleccionaremos la plantilla "Clase" y "DinnerRepository.cs" el nombre del archivo:

![](build-a-model-with-business-rule-validations/_static/image10.png)

A continuación, podemos implementar nuestra clase DinnerRespository con el código siguiente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperar, actualizar, insertar y eliminar mediante la clase de instancia DinnerRepository

Ahora que hemos creado nuestra clase DinnerRepository, echemos un vistazo a algunos ejemplos de código que muestran las tareas habituales que podemos hacer con él:

#### <a name="querying-examples"></a>Ejemplos de consultas

El código siguiente recupera una cena única utilizando el valor de DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

El código siguiente recupera todas las instancias dinners próximas y bucles a través de ellos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Insertar y ejemplos de actualización

El código siguiente muestra cómo agregar dos instancias dinners nuevo. Adiciones y modificaciones en el repositorio no están asignadas a la base de datos hasta que se llama al método de "Save()" en él. LINQ to SQL ajusta automáticamente todos los cambios en una transacción de base de datos: por lo que todos los cambios se realizan o ninguno de ellos hacer cuando se guarda el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

El código siguiente recupera un objeto existente de la cena y modifica las dos propiedades en él. Los cambios se confirman en la base de datos cuando se llama al método de "Save()" en nuestro repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

El código siguiente recupera una cena y, a continuación, agrega una confirmación de asistencia a él. Se consigue usando la colección de confirmaciones de asistencia en el objeto Dinner que LINQ to SQL creado para nosotros (porque no hay una relación primary-key/clave externa entre las dos en la base de datos). Este cambio se conserva en la base de datos como una nueva fila de tabla RSVP cuando se llama al método de "Save()" en el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Ejemplo de cómo eliminar

El código siguiente recupera un objeto de la cena existente y, a continuación, lo marca para eliminarse. Cuando se llama al método de "Save()" en el repositorio confirmará la eliminación en la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integración de validación y lógica de regla de negocios con clases de modelo

Integración de validación y lógica es una parte fundamental de cualquier aplicación que funcione con datos de regla de negocios.

#### <a name="schema-validation"></a>Validación de esquemas

Cuando se definen las clases de modelo mediante el diseñador LINQ to SQL, los tipos de datos de las propiedades de las clases del modelo de datos se corresponden con los tipos de datos de la tabla de base de datos. Por ejemplo: si la columna "EventDate" en la tabla de instancias Dinners es un valor "datetime", la clase de modelo de datos creada por LINQ to SQL será de tipo "DateTime" (que es un tipo de datos de .NET integrado). Esto significa que obtendrá errores de compilación si intenta asignar un número entero o un valor booleano a él desde el código y se producirá un error automáticamente si se intenta convertir implícitamente un tipo de cadena no válida a ella en tiempo de ejecución.

LINQ to SQL también automáticamente controla valores de escape de SQL por usted al usar cadenas: lo que ayuda a protegerle contra ataques de inyección SQL cuando se usa.

#### <a name="validation-and-business-rule-logic"></a>Validación y lógica de regla de negocios

Validación de esquemas es útil como primer paso, pero es suficiente con poca frecuencia. Mayoría de los escenarios reales requiere la capacidad para especificar la lógica de validación más completa que puede abarcar varias propiedades, ejecutar el código y suelen tener conocimiento del estado del modelo (por ejemplo: es lo que se va a crear/actualizar/eliminar, o dentro de un estado específico de dominio al igual que "archivados"). Hay una variedad de patrones diferentes y marcos de trabajo que pueden usarse para definir y aplicar las reglas de validación a clases de modelo, y hay varios basados en .NET marcos informales que pueden usarse para ayudar con esto. Puede usar prácticamente cualquiera de ellos dentro de las aplicaciones de ASP.NET MVC.

Para los fines de nuestra aplicación NerdDinner, vamos a usar un patrón relativamente sencillo y directa donde se exponen una propiedad IsValid y un método GetRuleViolations() en el objeto del modelo cena. La propiedad IsValid devolverá Verdadero o falso dependiendo de si la validación y reglas de negocios son válidas. El método GetRuleViolations() devolverá una lista de los errores de la regla.

Implementaremos IsValid y GetRuleViolations() para nuestro modelo cena agregando una "clase parcial" en nuestro proyecto. Las clases parciales pueden usarse para agregar métodos, propiedades y eventos para clases mantenidas por un diseñador de VS (por ejemplo, la clase Dinner generado por el diseñador LINQ to SQL) y ayudar a evitar la herramienta de temas relacionados con nuestro código. Podemos agregar una nueva clase parcial a nuestro proyecto con el botón secundario en la carpeta \Models y, a continuación, seleccione el comando de menú "Agregar nuevo elemento". A continuación, podemos elegir la plantilla "Clase" en el cuadro de diálogo "Agregar nuevo elemento" y asígnele el nombre Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Al hacer clic en el botón "Agregar" agregará un archivo Dinner.cs a nuestro proyecto y ábralo en el IDE. A continuación, podemos implementar un marco de cumplimiento o validación de regla básica mediante el siguiente código:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algunas notas sobre el código anterior:

- La clase Dinner se prologa con una palabra clave "parcial", lo que significa que el código dentro del mismo se combina con la clase genera y mantiene mediante el diseñador LINQ to SQL y compilado en una sola clase.
- La clase RuleViolation es una clase auxiliar que vamos a agregar al proyecto que nos permite proporcionar más detalles sobre una infracción de regla.
- El método Dinner.GetRuleViolations() hace que nuestro validación y reglas de negocios que se debe evaluar (implementaremos ellos en breve). A continuación, devuelve una secuencia de objetos RuleViolation que proporcionan más detalles sobre los errores de la regla.
- La propiedad Dinner.IsValid proporciona una propiedad auxiliares útiles que indica si el objeto de la cena tiene cualquier RuleViolations activo. Que se puede comprobar de forma proactiva por un desarrollador que utiliza el objeto de la cena en cualquier momento (y no provoca una excepción).
- El método parcial Dinner.OnValidate() es un enlace que LINQ to SQL proporciona que nos permite recibir una notificación cada vez que el objeto de la cena está a punto de conservarla en la base de datos. Nuestra implementación OnValidate() anterior garantiza que la cena no tiene ningún RuleViolations antes de guardarlo. Si se encuentra en un estado no válido provoca una excepción, lo que hará que LINQ to SQL a anular la transacción.

Este enfoque proporciona un marco sencillo que podemos integrar en las reglas de negocios y de validación. Por ahora vamos a agregar las siguientes reglas para nuestro método GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Estamos usando la característica "yield return" de C# para devolver una secuencia de cualquier RuleViolations. Las primeras comprobaciones de seis regla anterior simplemente exigir que las propiedades de cadena en nuestra Dinner no pueden ser nulo ni estar vacío. La última regla es un poco más interesante y llama a un método auxiliar de PhoneValidator.IsValidNumber() que podemos agregar nuestro proyecto para comprobar que el Teléfonodecontacto número país de formato coincide con la cena.

Podemos usar. Compatibilidad de expresiones regulares de la red para implementar esta compatibilidad de validación del teléfono. A continuación es una implementación simple de PhoneValidator que podemos agregar nuestro proyecto que nos permite agregar comprobaciones de patrón de Regex específico del país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Control de validación y las infracciones de lógica de negocios

Ahora que hemos agregado el código de regla de validación y business anterior, siempre que se intenta crear o actualizar una cena, se evalúan las reglas de lógica de validación y se apliquen.

Los desarrolladores pueden escribir código como a continuación para determinar si un objeto de la cena es válido de manera proactiva y recuperar una lista de todas las infracciones en él sin generar ninguna excepción:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si se intenta guardar una cena en un estado no válido, se producirá una excepción cuando se llama al método Save() en la instancia DinnerRepository. Esto ocurre porque LINQ to SQL llama automáticamente a nuestro método parcial Dinner.OnValidate() antes de guardar los cambios de la cena y hemos agregado código para Dinner.OnValidate() para generar una excepción si existe cualquier infracción de regla en la cena. Podemos detectar esta excepción y reactiva recuperar una lista de las infracciones de corregir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Dado que la validación y reglas de negocios se implementan dentro de nuestro nivel de modelo y no dentro de la capa de interfaz de usuario, se aplica y utiliza en todos los escenarios dentro de nuestra aplicación. Más adelante podemos cambiar o agregar reglas de negocios y tiene todo el código que funciona con nuestros objetos cena respeta.

Tener la flexibilidad de cambiar las reglas de negocios en un solo lugar, sin necesidad de ripple a lo largo de la aplicación y la lógica de la interfaz de usuario, estos cambios es un inicio de sesión de una aplicación bien escrita y una ventaja de que un marco de MVC ayuda a promover.

### <a name="next-step"></a>Paso siguiente

Ahora tenemos un modelo que podemos usar para consultar y actualizar la base de datos.

Ahora agreguemos algunos controladores y vistas al proyecto que podemos usar para crear una experiencia de interfaz de usuario HTML alrededor de ella.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Siguiente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
