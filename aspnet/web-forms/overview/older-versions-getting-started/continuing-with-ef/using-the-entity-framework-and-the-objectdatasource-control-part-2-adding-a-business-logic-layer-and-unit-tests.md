---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: "Con Entity Framework 4.0 y el Control ObjectDataSource, parte 2: agregar una capa de lógica de negocios y pruebas unitarias | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales se basa en la aplicación web de la Universidad de Contoso que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. ¿..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 0440f807c7baa7b92e5f05590eca9cc237b5aef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Con Entity Framework 4.0 y el Control ObjectDataSource, parte 2: agregar una capa de lógica de negocios y pruebas unitarias
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales que se basa en la aplicación web de la Universidad de Contoso que se crea mediante la [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales. Si no has completado los tutoriales anteriores, como punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa. Si tiene alguna pregunta acerca de los tutoriales, puede publicar para la [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


En el tutorial anterior creó una aplicación web de n niveles con Entity Framework y el `ObjectDataSource` control. Este tutorial muestra cómo agregar lógica de negocios durante la separación de la capa de lógica empresarial (BLL) y la capa de acceso a datos (DAL) y muestra cómo crear pruebas unitarias automatizadas para la capa BLL.

En este tutorial, se realizarán las siguientes tareas:

- Crear una interfaz de repositorio que declara los métodos de acceso a datos que necesita.
- Implemente la interfaz de repositorio en la clase de repositorio.
- Cree una clase de lógica de negocios que llama a la clase de repositorio para llevar a cabo funciones de acceso a datos.
- Conectar el `ObjectDataSource` control a la clase de la lógica de negocios en lugar de a la clase de repositorio.
- Crear un proyecto de prueba unitaria y una clase de repositorio que usa colecciones en memoria para su almacén de datos.
- Crear una prueba unitaria para la lógica de negocios que desea agregar a la clase de la lógica de negocios, a continuación, ejecutar la prueba y ve un error.
- Implementar la lógica de negocios de la clase de la lógica de negocios, a continuación, vuelva a ejecutar la unidad de prueba y véalo a pasar.

Trabajará con la *Departments.aspx* y *DepartmentsAdd.aspx* páginas que creó en el tutorial anterior.

## <a name="creating-a-repository-interface"></a>Crear una interfaz de repositorio

Comenzará mediante la creación de la interfaz del repositorio.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

En el *DAL* carpeta, cree un nuevo archivo de clase, asígnele el nombre *ISchoolRepository.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

La interfaz define un método para cada uno de lo CRUD (crear, leer, actualizar y eliminar) métodos que creó en la clase de repositorio.

En el `SchoolRepository` clase *SchoolRepository.cs*, indicar que esta clase implementa la `ISchoolRepository` interfaz:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Crear una clase de lógica de negocios

A continuación, creará la clase de la lógica de negocios. Hacer esto para que pueda agregar lógica de negocios que se ejecutará por el `ObjectDataSource` controlar, aunque no lo hará que todavía. Por ahora, la nueva clase de lógica de negocios sólo realizará las mismas operaciones CRUD que realiza el repositorio.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Cree una nueva carpeta y asígnele el nombre *BLL*. (En una aplicación del mundo real, la capa de lógica de negocios normalmente se implementa como una biblioteca de clases: un proyecto independiente, pero para que este tutorial resulte sencillo, clases BLL se mantendrán en una carpeta de proyecto.)

En el *BLL* carpeta, cree un nuevo archivo de clase, asígnele el nombre *SchoolBL.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Este código crea los mismos métodos CRUD que vio anteriormente en la clase de repositorio, pero en lugar de tener acceso directamente a los métodos de Entity Framework, llama el repositorio de métodos de clase.

La variable de clase que contiene una referencia a la clase de repositorio se define como un tipo de interfaz y el código que crea una instancia de la clase del repositorio se encuentra en dos constructores. El constructor sin parámetros que se utilizarán en el `ObjectDataSource` control. Crea una instancia de la `SchoolRepository` clase que creó anteriormente. El otro constructor permite que cualquier código que crea una instancia de la clase de lógica de negocios para pasar cualquier objeto que implementa la interfaz del repositorio.

Los métodos CRUD que llaman a la clase de repositorio y los dos constructores permiten usar la clase de la lógica de negocios con cualquier almacén de datos back-end que elija. La clase de la lógica de negocios no debe ser consciente de cómo la clase que realiza la llamada conserva los datos. (Esto se suele denominar *omisión de persistencia*.) Esto facilita la pruebas unitarias, porque la clase de la lógica de negocios se pueden conectar a una implementación de repositorio que usa algo como simple como en memoria `List` colecciones para almacenar datos.

> [!NOTE]
> Técnicamente, los objetos de entidad son todavía no que ignoran la persistencia, porque se crea una instancia de las clases que heredan de Entity Framework `EntityObject` clase. Omisión de persistencia completa, puede usar *objetos CLR antiguos sin formato*, o *POCOs*, en lugar de objetos que se heredan de la `EntityObject` clase. Uso de POCOs queda fuera del ámbito de este tutorial. Para obtener más información, consulte [capacidad de prueba y Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx) en el sitio Web MSDN.)


Ahora puede conectar el `ObjectDataSource` controles a la lógica de negocios en lugar de la clase en el repositorio y comprobar que todo funciona igual que antes.

En *Departments.aspx* y *DepartmentsAdd.aspx*, cambiar cada aparición de `TypeName="ContosoUniversity.DAL.SchoolRepository"` a `TypeName="ContosoUniversity.BLL.SchoolBL`". (Hay cuatro instancias en total).

Ejecute el *Departments.aspx* y *DepartmentsAdd.aspx* páginas para comprobar que seguirán funcionando como lo hacían antes.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Crear un proyecto de prueba unitaria y la implementación de repositorio

Agregar un nuevo proyecto a la solución mediante la **proyecto de prueba** plantilla y asígnele el nombre `ContosoUniversity.Tests`.

En el proyecto de prueba, agregue una referencia a `System.Data.Entity` y agregue una referencia al proyecto el `ContosoUniversity` proyecto.

Ahora puede crear la clase de repositorio que va a utilizar con pruebas unitarias. El almacén de datos para este repositorio será dentro de la clase.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

En el proyecto de prueba, cree un nuevo archivo de clase, asígnele el nombre *MockSchoolRepository.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Esta clase de repositorio tiene los mismos métodos CRUD como uno que tiene acceso directo a Entity Framework, pero funcionan con `List` las colecciones en memoria en lugar de con una base de datos. Esto facilita a una clase de prueba configurar y validar pruebas unitarias para la clase de la lógica de negocios.

## <a name="creating-unit-tests"></a>Crear pruebas unitarias

El **probar** plantilla de proyecto crea una clase de prueba de unidad de código auxiliar, y la siguiente tarea consiste en modificar esta clase agregando métodos de prueba unitaria a él de lógica de negocios que desea agregar a la clase de la lógica de negocios.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

En la Universidad de Contoso, cualquier instructor individual solo puede ser el Administrador de un departamento único y debe agregar lógica de negocios para aplicar esta regla. Para empezar, agregar pruebas y ejecutar las pruebas para verlos producirá un error. A continuación, podrá agregar el código y vuelva a ejecutar las pruebas, se espera verlos a pasar.

Abra la *UnitTest1.cs* de archivos y agregar `using` instrucciones para las capas de lógica y acceso a datos empresariales que creó en el proyecto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Reemplace el `TestMethod1` método con los métodos siguientes:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

El `CreateSchoolBL` método crea una instancia de la clase de repositorio que ha creado para la prueba unitaria de proyecto, que, a continuación, pasa a una nueva instancia de la clase de la lógica de negocios. El método, a continuación, utiliza la clase de la lógica de negocios para insertar tres departamentos que puede usar en métodos de prueba.

Los métodos de prueba para comprobar que la clase de la lógica de negocios produce una excepción si alguien intenta insertar un nuevo departamento con el mismo administrador de un departamento existente, o si alguien intenta actualizar el administrador del departamento si se establece en el identificador de una persona ya que es el Administrador de otro departamento.

No ha creado la clase de excepción, por lo que este código no se compilará. Para obtener compilarla, haga clic en `DuplicateAdministratorException` y seleccione **generar**y, a continuación, **clase**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Esto crea una clase en el proyecto de prueba que se puede eliminar después de crear la clase de excepción en el proyecto principal. e implementa la lógica de negocios.

Ejecute el proyecto de prueba. Como se esperaba, producirá un error en las pruebas.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Agregar lógica de negocios para realizar un paso de prueba

A continuación, podrá implementar la lógica de negocios que hace imposible que se establecerá como el Administrador de un departamento de alguien que ya es administrador de otro departamento. Que produce una excepción de la capa de lógica de negocios y, a continuación, detectar en la capa de presentación si un usuario edita un departamento y haga clic en **actualización** después de seleccionar un usuario que ya es un administrador. (También puede quitar instructores en la lista desplegable que ya son administradores antes de presentar la página, pero el propósito aquí es para que funcione con la capa de lógica de negocios).

Empiece por crear la clase de excepción que podrá producir cuando un usuario intenta establecer a un instructor el Administrador de más de un departamento. En el proyecto principal, cree un nuevo archivo de clase en el *BLL* carpeta, asígnele el nombre *DuplicateAdministratorException.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Ahora, elimine la contraseña *DuplicateAdministratorException.cs* archivo que crearon en el proyecto de prueba para poder compilar.

En el proyecto principal, abra el *SchoolBL.cs* de archivos y agregue el siguiente método que contiene la lógica de validación. (El código hace referencia a un método que creará más adelante).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Llamaremos a este método cuando se insertan o actualizan `Department` entidades con el fin de comprobar si otro departamento ya tiene el mismo administrador.

El código llama a un método para buscar la base de datos para un `Department` entidad que tiene el mismo `Administrator` valor de la propiedad como la entidad que se va a insertar o actualizar. Si encuentra alguno, el código produce una excepción. Ninguna comprobación de validación es necesaria si la entidad que se va a insertar o actualizar no tiene ningún `Administrator` valor y ninguna excepción se produce si se llama al método durante una actualización y el `Department` entidad encuentra coincidencias el `Department` entidad que se está actualizando.

Llame al método nuevo desde la `Insert` y `Update` métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

En *ISchoolRepository.cs*, agregue la siguiente declaración para el nuevo método de acceso a datos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

En *SchoolRepository.cs*, agregue las siguientes `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

En *SchoolRepository.cs*, agregar el nuevo método de acceso a datos siguientes:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Este código recupera `Department` entidades que tienen un administrador especificado. Solo un departamento debe encontrarse (si existe). Sin embargo, dado que ninguna restricción se basa en la base de datos, el tipo de valor devuelto es una colección en caso de que se encuentran varios departamentos.

De forma predeterminada, cuando el contexto del objeto recupera las entidades de la base de datos, realiza un seguimiento de ellos en su administrador de estado de objeto. El `MergeOption.NoTracking` parámetro especifica que este seguimiento no se hará para esta consulta. Esto es necesario porque la consulta podría devolver la entidad exacta que está intentando actualizar y, a continuación, no podrá asociar esa entidad. Por ejemplo, si edita el departamento de historial de la *Departments.aspx* página y el administrador no modifiquen, esta consulta devolverá el departamento de historial. Si `NoTracking` no está establecido, el contexto del objeto ya tendrá la entidad department de historial en el Administrador de estado de su objeto. A continuación, cuando se adjunta la entidad del departamento de historial que se vuelve a crear del estado de vista, el contexto del objeto producirá una excepción que indicara `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Como alternativa a especificar `MergeOption.NoTracking`, puede crear un nuevo contexto de objeto solo para esta consulta. Dado que el contexto del objeto nuevo tendría su propio administrador de estado de objeto, no habría ningún conflicto cuando se llama a la `Attach` método. El contexto del objeto nuevo compartirían la conexión de base de datos y metadatos con el contexto del objeto original, por lo que la reducción del rendimiento de este enfoque alternativo sería mínima. Sin embargo, presenta el enfoque mostrado en este caso, el `NoTracking` opción, que puede encontrar útiles en otros contextos. El `NoTracking` opción se describe más adelante en un tutorial posterior de esta serie.)

En el proyecto de prueba, agregue el nuevo método de acceso a datos a *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Este código usa LINQ para efectuar la misma selección de datos que el `ContosoUniversity` repositorio del proyecto usa LINQ to Entities para.

Vuelva a ejecutar el proyecto de prueba. Esta vez las pruebas son correctas.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Control de excepciones de ObjectDataSource

En el `ContosoUniversity` project, lleve a cabo la *Departments.aspx* página e intenta cambiar el Administrador de un departamento a alguien que ya es administrador de otro departamento. (Recuerde que solo se pueden editar los departamentos que agregó durante este tutorial, dado que la base de datos viene cargado previamente con datos no válidos). Obtener la siguiente página de error de servidor:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

No desea que los usuarios vean este tipo de página de error, por lo que necesita agregar código de control de errores. Abra *Departments.aspx* y especificar un controlador para el `OnUpdated` eventos de la `DepartmentsObjectDataSource`. La `ObjectDataSource` etiqueta de apertura ahora similar al siguiente ejemplo.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

En *Departments.aspx.cs*, agregue las siguientes `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Agregue el siguiente controlador para el `Updated` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Si el `ObjectDataSource` control detecta una excepción al intentar realizar la actualización, la excepción se pasa en el argumento de evento (`e`) para este controlador. El código en el controlador se comprueba para ver si la excepción es la excepción de administrador duplicado. Si es así, el código crea un control de validación que contiene un mensaje de error para el `ValidationSummary` control para mostrar.

Ejecute la página e intentará convertir a alguien en el Administrador de los dos departamentos de nuevo. Esta vez el `ValidationSummary` control muestra un mensaje de error.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Realizar modificaciones similares a la *DepartmentsAdd.aspx* página. En *DepartmentsAdd.aspx*, especificar un controlador para el `OnInserted` eventos de la `DepartmentsObjectDataSource`. El marcado resultante tendrá un aspecto similar en el siguiente ejemplo.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

En *DepartmentsAdd.aspx.cs*, agregar la misma `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Agregue el siguiente controlador de eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Ahora puede probar el *DepartmentsAdd.aspx.cs* página para comprobar que también correctamente controla intentos que realizará el Administrador de más de un departamento de una persona.

Con esto finaliza la introducción a implementar el patrón de repositorio para utilizar el `ObjectDataSource` control con Entity Framework. Para obtener más información sobre el modelo de repositorio y la capacidad de prueba, vea las notas del producto MSDN [capacidad de prueba y Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx).

En el tutorial siguiente verá cómo agregar ordenación y filtrado funcionalidad a la aplicación.

>[!div class="step-by-step"]
[Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[Siguiente](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
