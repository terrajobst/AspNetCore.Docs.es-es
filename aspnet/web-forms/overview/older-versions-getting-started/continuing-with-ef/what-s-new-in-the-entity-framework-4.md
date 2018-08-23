---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Novedades de Entity Framework 4.0 | Microsoft Docs
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de Contoso University establecen que se crea mediante la introducción a la serie de tutoriales de Entity Framework 4.0. PUEDO...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 402e7ace1abad899d32ed179d6b68de4e5a129f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838217"
---
<a name="whats-new-in-the-entity-framework-40"></a>Novedades de Entity Framework 4.0
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web de Contoso University creado por el [Introducción a Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales. Si no has completado los tutoriales anteriores, como un punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa. Si tiene preguntas acerca de los tutoriales, puede publicarlos en el [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


En el tutorial anterior, ha visto algunos métodos para maximizar el rendimiento de una aplicación web que utiliza Entity Framework. En este tutorial se tratan algunas de las características nuevas más importantes en la versión 4 de Entity Framework, e incluye vínculos a recursos que proporcionan una introducción más completa para todas las características nuevas. Las características que se resaltan en este tutorial incluyen lo siguiente:

- Asociaciones de clave externa.
- Ejecutar comandos SQL definido por el usuario.
- Desarrollo de Model first.
- Compatibilidad con POCO.

Además, el tutorial se presentan brevemente *desarrollo code first*, una característica que entra en la próxima versión de Entity Framework.

Para iniciar el tutorial, inicie Visual Studio y abra la aplicación web de Contoso University establecen que estaba trabajando con en el tutorial anterior.

## <a name="foreign-key-associations"></a>Asociaciones de clave externa

La versión 3.5 de Entity Framework incluye las propiedades de navegación, pero no incluir propiedades de clave externa en el modelo de datos. Por ejemplo, el `CourseID` y `StudentID` columnas de la `StudentGrade` se omitiría tabla desde el `StudentGrade` entidad.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

El motivo de este enfoque fue que, en realidad, las claves externas son un detalle de implementación física y no pertenezcan a un modelo de datos conceptual. Sin embargo, la práctica, a menudo es más fácil trabajar con entidades del código cuando tenga acceso directo a las claves externas.

Para un ejemplo de cómo externas claves en el modelo de datos puede simplificar el código, tenga en cuenta cómo habría tenido que código el *DepartmentsAdd.aspx* página sin ellos. En el `Department` entidad, el `Administrator` propiedad es una clave externa que corresponde a `PersonID` en el `Person` entidad. Para establecer la asociación entre un nuevo departamento y su administrador, sólo tenía que hacer se estableció el valor de la `Administrator` propiedad en el `ItemInserting` controlador de eventos del control enlazado a datos:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Sin claves externas en el modelo de datos, controlaría la `Inserting` eventos del control de origen de datos en lugar de la `ItemInserting` evento del control enlazado a datos, con el fin de obtener una referencia a la propia entidad antes de la entidad se agrega al conjunto de entidades. Una vez que hacen referencia, establecer la asociación con código similar en los ejemplos siguientes:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Como puede ver en el equipo de Entity Framework [entrada de blog sobre las asociaciones de clave externa](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), hay otros casos donde la diferencia en la complejidad del código es mucho mayor. Para satisfacer las necesidades de aquellos que prefieren que vivir con los detalles de implementación en el modelo de datos conceptual en aras de código más simple, Entity Framework ahora ofrece la opción de incluir claves externas en el modelo de datos.

En la terminología de Entity Framework, si incluye las claves externas en el modelo de datos usa *asociaciones de clave externa*, y si excluye las claves externas que usa *asociaciones independientes*.

## <a name="executing-user-defined-sql-commands"></a>Ejecutar comandos SQL definidas por el usuario

En versiones anteriores de Entity Framework, no había ninguna manera fácil de crear sus propios comandos SQL sobre la marcha y ejecutarlas. Entity Framework genera dinámicamente los comandos SQL para usted, o había que crear un procedimiento almacenado e impórtelo como una función. La versión 4 agrega `ExecuteStoreQuery` y `ExecuteStoreCommand` métodos el `ObjectContext` clase que resulte más fácil para pasar de cualquier consulta directamente a la base de datos.

Supongamos que los administradores de Contoso University quieren realizar cambios masivos en la base de datos sin tener que pasar por el proceso de creación de un procedimiento almacenado e importarlo en el modelo de datos. Su primera solicitud es para una página que les permite cambiar el número de créditos para todos los cursos de la base de datos. En la página web, desean poder introducir un número que se utiliza para multiplicar el valor de cada `Course` la fila `Credits` columna.

Crear una nueva página que usa el *Site.Master* página principal y denomínelo *UpdateCredits.aspx*. A continuación, agregue el siguiente marcado para la `Content` control denominado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Este marcado crea un `TextBox` control en el que el usuario puede escribir el valor de multiplicador, un `Button` control hacer clic con el fin de ejecutar el comando y un `Label` control para que indica el número de filas afectadas.

Abra *UpdateCredits.aspx.cs*y agregue la siguiente `using` instrucción y un controlador para el botón `Click` eventos:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Este código ejecuta la instrucción SQL `Update` utilizando el valor en el cuadro de texto de comando y usa la etiqueta para mostrar el número de filas afectadas. Antes de ejecutar la página, ejecutar el *Courses.aspx* página para obtener una imagen "before" de algunos datos.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Ejecute *UpdateCredits.aspx*, escriba "10" como el multiplicador y, a continuación, haga clic en **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Ejecute el *Courses.aspx* página nuevamente para ver los datos modificados.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Si desea establecer el número de créditos a sus valores originales, en *UpdateCredits.aspx.cs* cambiar `Credits * {0}` a `Credits / {0}` y vuelva a ejecutar la página, escriba 10 como el divisor.)

Para obtener más información acerca de cómo ejecutar consultas que definen en el código, consulte [Cómo: directamente ejecutar comandos contra el origen de datos](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Desarrollo de Model First

En estos tutoriales se crea la base de datos en primer lugar y, a continuación, genera el modelo de datos basado en la estructura de base de datos. En el Entity Framework 4 puede comenzar con el modelo de datos en su lugar y generar la base de datos en función de la estructura del modelo de datos. Si va a crear una aplicación para que la base de datos ya no existe, el enfoque model first le permite crear entidades y relaciones que tengan sentido conceptualmente de la aplicación, y no preocuparse acerca de los detalles de implementación física . (Esto es así solo a través de las fases iniciales de desarrollo, sin embargo. Finalmente, la base de datos se crearán y tendrá los datos de producción en él, y volver a crearla desde el modelo ya no será práctico; en ese momento podrá volver al enfoque centrado en la base de datos.)

En esta sección del tutorial, creará un modelo de datos simple y generar la base de datos a partir de él.

En **el Explorador de soluciones**, haga clic en el *DAL* carpeta y seleccione **Agregar nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo **plantillas instaladas** seleccione **datos** y, a continuación, seleccione el **ADO.NET Entity Data Model** plantilla . Asigne al nuevo archivo *AlumniAssociationModel.edmx* y haga clic en **agregar**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Esto inicia al Asistente para Entity Data Model. En el **Elegir contenido de Model** paso, seleccione **modelo vacío** y, a continuación, haga clic en **finalizar**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

El **Entity Data Model Designer** se abre con una superficie de diseño en blanco. Arrastre un **entidad** elemento desde el **cuadro de herramientas** a la superficie de diseño.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Cambie el nombre de entidad de `Entity1` a `Alumnus`, cambie el `Id` nombre de propiedad para `AlumnusId`y agregue una nueva propiedad escalar denominada `Name`. Para agregar nuevas propiedades puede presionar ENTRAR después de cambiar el nombre de la `Id` columna, o haga clic en la entidad y seleccione **Agregar propiedad escalar**. Es el tipo predeterminado para nuevas propiedades `String`, lo cual está bien para esta demostración sencilla, pero por supuesto puede cambiar cosas como tipo de datos en el **propiedades** ventana.

Crear otra entidad del mismo modo y asígnele el nombre `Donation`. Cambiar el `Id` propiedad `DonationId` y agregue una propiedad escalar denominada `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Para agregar una asociación entre estas dos entidades, haga clic en el `Alumnus` entidad, seleccione **agregar**y, a continuación, seleccione **asociación**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Los valores predeterminados de la **Agregar asociación** cuadro de diálogo son los que desea (uno a varios, incluyen propiedades de navegación, incluir claves externas), así que simplemente haga clic en **Aceptar**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

El diseñador agrega una línea de asociación y una propiedad de clave externa.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Ahora está listo para crear la base de datos. Haga clic en la superficie de diseño y seleccione **generar base de datos de modelo**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Esto inicia al Asistente para generar base de datos. (Si ve las advertencias que indican que no se asignan las entidades, puede ignorarlos por el momento.)

En el **elegir la conexión de datos** paso, haga clic en **nueva conexión**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

En el **las propiedades de conexión** diálogo cuadro, seleccione la instancia local de SQL Server Express y el nombre de la base de datos `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Haga clic en **Sí** cuando se le pregunte si desea crear la base de datos. Cuando el **elegir la conexión de datos** paso vuelve a aparecer, haga clic en **siguiente**.

En el **resumen y configuración** paso, haga clic en **finalizar**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Un *.sql* se crea el archivo con los comandos de data definition language (DDL), pero los comandos no se han ejecutado todavía.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Usar una herramienta como **SQL Server Management Studio** para ejecutar el script y crear las tablas, que se haya realizado cuando crea el `School` para la base de datos [el primer tutorial de la serie de tutoriales de introducción ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A menos que se descargó con la base de datos).

Ahora puede usar el `AlumniAssociation` modelo de datos del sitio Web de páginas del mismo modo que ha estado usando el `School` modelo. Para probar esto, agregue algunos datos a las tablas y cree una página web que muestra los datos.

Mediante **Explorador de servidores**, agregue las siguientes filas a la `Alumnus` y `Donation` tablas.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Crear una nueva página web denominada *Alumni.aspx* que usa el *Site.Master* página maestra. Agregue el siguiente marcado para la `Content` control denominado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Este marcado crea anidada `GridView` controla el exterior para mostrar los nombres antiguos alumnos y uno interno para mostrar las fechas de donación y cantidades.

Abra *Alumni.aspx.cs*. Agregar un `using` acceder a una instrucción para los datos de capa y un controlador para el exterior `GridView` del control `RowDataBound` eventos:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Este código enlaza los datos interno `GridView` controlar mediante la `Donations` propiedad de navegación de la fila actual `Alumnus` entidad.

Ejecute la página.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Nota: esta página se incluye en el proyecto descargable, pero para que funcione, debe crear la base de datos en la instancia local de SQL Server Express; la base de datos no se incluye como un *.mdf* de archivos en el *aplicación\_ Datos* carpeta.)

Para obtener más información sobre cómo usar la característica de model first de Entity Framework, vea [Model First en Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Compatibilidad con POCO

Cuando se usa la metodología de diseño guiado por el dominio, diseñar clases de datos que representan los datos y el comportamiento que es relevante para el dominio de negocio. Estas clases deben ser independientes de cualquier tecnología específica que se usa para almacenar (Guardar) los datos; en otras palabras, deben ser *ignoran la persistencia*. Omisión de persistencia puede también facilitar una clase de prueba unitaria porque el proyecto de prueba unitaria puede usar cualquier tecnología de persistencia es más conveniente para las pruebas. Las versiones anteriores de Entity Framework ofrecían compatibilidad limitada para la omisión de persistencia porque las clases de entidad que se tenían que heredan de la `EntityObject` clase y, por tanto, incluye una gran cantidad de funcionalidad específica de Entity Framework.

Entity Framework 4 introduce la posibilidad de usar las clases de entidad que no heredan de la `EntityObject` clase y, por tanto, ignoran la persistencia. En el contexto de Entity Framework, normalmente se denominan clases como esta *los objetos CLR antiguos sin formato* (POCO o poco). Puede escribir clases POCO manualmente, o puede generar automáticamente a partir de un modelo de datos existente mediante las plantillas del Kit de herramientas de transformación de plantillas de texto (T4) proporcionadas por Entity Framework.

Para obtener más información acerca de cómo utilizar poco en Entity Framework, vea los siguientes recursos:

- [Trabajar con entidades POCO](https://msdn.microsoft.com/library/dd456853.aspx). Se trata de un documento MSDN que es una introducción a poco, con vínculos a otros documentos con información más detallada.
- [Tutorial: POCO plantilla para Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) se trata de una entrada de blog del equipo de desarrollo de Entity Framework, con vínculos a otras entradas de blog sobre poco.

## <a name="code-first-development"></a>Desarrollo de Code First

Compatibilidad con POCO en Entity Framework 4 todavía requiere que cree un modelo de datos y vincular las clases de entidad al modelo de datos. La próxima versión de Entity Framework incluirá una característica denominada *desarrollo code first*. Esta característica permite usar Entity Framework con sus propias clases POCO sin necesidad de usar el Diseñador de modelos de datos o un archivo XML de modelo de datos. (Por lo tanto, esta opción también se ha llamado *sólo código*; *code first* y *sólo código* ambos hacen referencia a la misma característica de Entity Framework.)

Para obtener más información sobre cómo usar el enfoque de code first al desarrollo, consulte los siguientes recursos:

- [Desarrollo con 4 de Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Se trata de un artículo de blog de desarrollo code first presentación de Scott Guthrie.
- [Publicaciones de Blog del equipo de desarrollo de Entity Framework - CodeOnly etiquetada](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entradas de Blog del equipo de desarrollo de Entity Framework - Code First etiquetadas](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Tutorial de Store música de MVC - parte 4: acceso a datos y modelos](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Introducción a MVC 3 - parte 4: desarrollo de Code First de Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Además, se proyecta un nuevo tutorial de Code First de MVC que compila una aplicación similar a la aplicación Contoso University para publicarse en la primavera de 2011 a las [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Más información

Con esto finaliza la introducción hasta novedades de Entity Framework y este si continúa con la serie de tutoriales de Entity Framework. Para obtener más información acerca de las nuevas características de Entity Framework 4 no se tratan aquí, consulte los siguientes recursos:

- [What ' s New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) tema de MSDN sobre las nuevas características de versión 4 de Entity Framework.
- [Anuncio de la versión de Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) entrada de blog del equipo de desarrollo de Entity Framework sobre las nuevas características en la versión 4.

> [!div class="step-by-step"]
> [Anterior](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
