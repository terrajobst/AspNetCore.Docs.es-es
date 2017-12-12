---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: Implementar una base de datos (VB) | Documentos de Microsoft
author: rick-anderson
description: "Implementar una aplicación web ASP.NET implica obtener los archivos necesarios y los recursos del entorno de desarrollo al entorno de producción. Para da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: b0890d50f21eb790d81d54261a67fcf487b1c95e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-database-vb"></a>Implementar una base de datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> Implementar una aplicación web ASP.NET implica obtener los archivos necesarios y los recursos del entorno de desarrollo al entorno de producción. Para las aplicaciones web orientadas a datos, esto incluye el esquema de base de datos y los datos. Este tutorial es el primero de una serie que explora los pasos necesarios para implementar correctamente la base de datos desde el entorno de desarrollo a producción.


### <a name="introduction"></a>Introducción

Implementar una aplicación web ASP.NET implica obtener los archivos necesarios y los recursos del entorno de desarrollo al entorno de producción. A lo largo de los últimos seis tutoriales analizamos implementar una aplicación web simple de reseñas de libros. Este sitio de demostración se compone de un número de recursos de servidor: las páginas ASP.NET, archivos de configuración, un `Web.sitemap` archivo y así sucesivamente - junto con los recursos del lado cliente, como imágenes y archivos CSS. Pero ¿qué ocurre con controladas por datos aplicaciones web? ¿Deben tener cuidados qué pasos adicionales para implementar una aplicación web que usa una base de datos?

En el siguientes varios tutoriales se abordará los pasos necesarios para implementar una aplicación web controlada por datos. Este tutorial se inicia mediante el examen de cómo obtener un esquema de s de la base de datos y el contenido desde el entorno de desarrollo al entorno de producción, mientras que el tutorial posterior se examina los cambios de configuración necesarios. Después de que se explorará desafíos de la implementación de una base de datos que utiliza los servicios de aplicación (pertenencia, Roles, perfil y así sucesivamente).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examen de la aplicación Web de revisiones de libro actualizado

Para demostrar la implementación de una aplicación web controlada por datos, se ha actualizado la aplicación web de reseñas de libros desde un sitio Web simple, estática a una controlada por datos. Como antes, existen dos versiones de la aplicación en la descarga de este tutorial s: uno que usa el modelo de proyecto de aplicación Web y otro que usa el modelo de proyecto de sitio Web.

Las reseñas de libros actualizados web aplicación usa un [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) base de datos que se almacena en el sitio s `App_Data` carpeta (`~/App_Data/Reviews.mdf`). Si tiene instalado en el equipo de SQL Server 2008, a continuación, debe ejecutar la demostración sin errores. Si tiene una versión anterior de SQL Server, puede instalar libre SQL Server 2008 Express Edition o puede usar la base de datos de secuencias de comandos disponibles en este tutorial s descargar para crear la base de datos usted mismo.

El `Reviews.mdf` base de datos contiene cuatro tablas:

- `Genres`-incluye un registro para cada género, como la tecnología, ficción y Business.
- `Books`-incluye un registro para cada revisión, con columnas como `Title`, `GenreId`, `ReviewDate`, y `Review`, entre otros.
- `Authors`-incluye información acerca de cada autor que ha contribuido a un libro de revisión.
- `BooksAuthors`-una tabla de la combinación de varios a varios que especifica qué autores escribieron qué libros.
  

La figura 1 muestra un diagrama de ER de estos cuatro tablas.


[![La base de datos de aplicación Web de revisiones de libreta s es consta de cuatro tablas](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**Figura 1**: s base de datos de la aplicación Web de las revisiones del libro es consta de cuatro tablas ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image3.jpg))


La versión anterior del sitio Web de reseñas de libros tenía una página ASP.NET independiente para cada libro. Por ejemplo, se produjo una página denominada `~/Tech/TYASP35.aspx` que contiene la revisión de *enseñar a usted mismo ASP.NET 3.5 en 24 horas*. Esta nueva versión del sitio Web orientadas a datos tiene las revisiones que se almacenan en la base de datos y una sola página ASP.NET, Review.aspx?ID=*bookId*, que muestra la revisión para el libro especificado. Del mismo modo, hay un Genre.aspx?ID=*genreId* página que enumera los libros del género especificado revisados.

Las figuras 2 y 3 mostrar la `Genre.aspx` y `Review.aspx` páginas en acción. Tenga en cuenta la dirección URL en la barra de direcciones para cada página. En la figura 2 TI s Genre.aspx? ID = c 47-82a0-c8ec75de7e0e de la 85d164ba-1123-4. Dado que es 85d164ba-1123-4c47-82a0-c8ec75de7e0e la `GenreId` valor para el género de tecnología, las lecturas de encabezado de página s "Tecnología revisa" y la lista con viñetas enumera las revisiones en el sitio que se encuentran en este género.


[![La página de género de tecnología](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**Figura 2**: la página de tecnología de género ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image6.jpg))


[![La revisión de Aprenda ASP.NET 3.5 en 24 horas](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**Figura 3**: la revisión de *enseñar a usted mismo ASP.NET 3.5 en 24 horas* ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image9.jpg))


La aplicación web libreta revisa también incluye una sección de administración donde los administradores pueden agregar, editar y eliminar géneros, revisiones y la información de autor. Actualmente, cualquier visitante puede tener acceso a la sección de administración. En un tutorial posterior comenzaremos agregar compatibilidad para las cuentas de usuario y permitir que sólo los usuarios autorizados en las páginas de administración.

Si descarga la aplicación de reseñas de libros por favor, tenga en cuenta que su objetivo es demostrar la implementación de una aplicación controlada por datos. No presenta los procedimientos recomendados al diseño de la aplicación. Por ejemplo, no hay ninguna capa de acceso de datos (DAL) independiente; las páginas ASP.NET se comunican directamente con la base de datos a través del control SqlDataSource o el código de ADO.NET en sus clases de código subyacente. Para obtener información más detallada en la creación de aplicaciones orientadas a datos con una arquitectura en capas, consulte mi [ *trabajar con datos* tutoriales](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bases de datos en desarrollo frente a producción

Cuando comience el desarrollo en una aplicación web orientadas a datos debe especificar una cadena de conexión de base de datos, que proporciona los detalles de la aplicación acerca de cómo conectarse a la base de datos. Esta cadena de conexión especifica, entre otras cosas, el servidor de base de datos, el nombre de la base de datos y la información de seguridad. A menudo, es diferente a la base de datos utiliza cuando la base de datos utilizado por la aplicación durante el desarrollo, s en producción. Hay numerosas ventajas del uso de bases de datos diferentes para el desarrollo y producción. Con otra base de datos en desarrollo significa don t que se tiene que preocuparse por accidente, modificar o eliminar datos en directo. También le permite colocar en los datos de prueba simulada o realizar cambios importantes en el modelo de datos sin tener que preocuparse sobre los efectos de la aplicación en producción. La desventaja de tener una base de datos diferentes en los entornos de desarrollo y producción es que cuando la aplicación implementa la base de datos y los cambios pertinentes en el esquema de base de datos s o datos también deben implementarse.

Antes de la primera implementación, hay solo una instancia de la base de datos y esa instancia se encuentra en el entorno de desarrollo. Al implementar la aplicación en producción por primera vez se debemos no sólo copiar los archivos necesarios del servidor y de cliente, sino también copiar la base de datos desde el entorno de desarrollo en el entorno de producción. Se trata de dónde estamos parados derecho ahora con la aplicación web de reseñas de libros, la base de datos reside en el `App_Data` carpeta en nuestro entorno de desarrollo pero que no tenga aún se ha insertado en el entorno de producción.

Una vez que se ha implementado la aplicación, hay dos copias de la base de datos. Ya que la aplicación ha evolucionado, pueden agregar nuevas características, lo que hace necesario un cambio en el modelo de datos (como agregar nuevas columnas a tablas existentes, realizar cambios en las columnas existentes, agregar nuevas tablas y así sucesivamente). Cuando a continuación se implementa la aplicación web, los cambios se aplican a la base de datos en el entorno de desarrollo desde la última implementación debe aplicarse a la base de datos de producción. Algunas estrategias para la administración de este proceso se tratan en un tutorial posterior. Este tutorial se centra en la implementación de la base de datos completo desde el entorno de desarrollo a producción.

## <a name="deploying-the-database-to-the-production-environment"></a>Implementación de la base de datos en el entorno de producción

El resto de este tutorial examina cómo implementar la base de datos desde el entorno de desarrollo al entorno de producción. Si está siguiendo necesita para asegurarse de que su cuenta con su proveedor de hospedaje web utiliza Microsoft SQL Server base de datos admite. También necesitará tener cierta información en cuestión, es decir, el nombre del servidor de base de datos, el nombre de la base de datos y el nombre de usuario y contraseña utilizada para conectarse a la base de datos.

Como se indicó anteriormente en este tutorial, la base de datos del sitio Web s reseñas de libros es una base de datos de SQL Server 2008 Express Edition almacenado en el `App_Data` carpeta. Llegar al motivo que implementar una base de datos debe ser tan sencillo como copiar el `App_Data` carpeta desde el entorno de desarrollo al entorno de producción. Sin embargo, la mayoría de los proveedores de host de web no admite bases de datos de hospedaje en el `App_Data` carpeta por razones de seguridad. En su lugar, los hosts de web proporcionan una cuenta en un servidor de base de datos de SQL Server dentro de su entorno. Implementación de la base de datos desde el entorno de desarrollo al entorno de producción requiere la obtención de la base de datos registrado en el servidor de base de datos de web host s.

Entonces, ¿cómo obtener la base de datos desde el entorno de desarrollo al entorno de producción? Hay un par de formas para realizar esta función de qué servicios las ofertas de host web. Con algunos servidores de host, como DiscountASP.NET, puede FTP una copia de seguridad de la base de datos o los datos reales `.mdf` de archivos a su sitio Web y, a continuación, en el Panel de Control, restaure el archivo de copia de seguridad o adjuntar el `.mdf` archivo en el servidor de base de datos de SQL Server. Con estas herramientas de implementación de la base de datos es tan sencillo como copiar el `App_Data` carpeta para el entorno de producción y, a continuación, conectarlo a través del Panel de Control. Esto es quizás la forma más fácil y rápida para publicar la base de datos por primera vez.

Otro enfoque consiste en utilizar al Asistente para publicación de base de datos. El Asistente para publicación de base de datos es una aplicación de escritorio de Windows que generará los comandos SQL para crear el esquema de s de la base de datos: tablas, procedimientos almacenados, vistas, funciones definidas por el usuario y así sucesivamente - y, opcionalmente, los datos en las tablas. Puede, a continuación, conectarse a su servidor de base de datos de web host proveedor s mediante SQL Server Management Studio y, a continuación, ejecutar este script para duplicar la base de datos de producción. Incluso mejor, si su proveedor de hospedaje web admite Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) puede tener el script generado por el Asistente para publicar bases de datos que se ejecuta automáticamente en el servidor de base de datos en su nombre. Dado que el Asistente para publicación de base de datos genera un script que crea el esquema de base de datos s y los datos, funcionará independientemente de si su proveedor de hospedaje web ofrece características como adjuntar una cargado `.mdf` archivo.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generar los comandos SQL para crear el esquema de base de datos y los datos mediante el Asistente para la publicación de la base de datos

Permiten s orientará sobre el uso del Asistente para publicación de base de datos para implementar la base de datos de reseñas de libros en producción. Si está utilizando Visual Studio 2008 o posterior, el Asistente para publicación de base de datos ya está instalado. Si está utilizando Visual Studio 2005, deberá primero [descargar e instalar](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) el asistente.

Abra Visual Studio y navegue hasta la `Reviews.mdf` base de datos. Si usas Visual Web Developer, vaya al explorador de base de datos; Si se utiliza Visual Studio, utilice el Explorador de servidores. La figura 4 muestra el `Reviews.mdf` base de datos en el Explorador de base de datos en Visual Web Developer. Como se muestra en la figura 4, la `Reviews.mdf` base de datos se compone de cuatro tablas, tres procedimientos almacenados y una función definida por el usuario.


[![Busque la base de datos en el Explorador de base de datos o el Explorador de servidores](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**Figura 4**: buscar la base de datos en el Explorador de base de datos o el Explorador de servidores ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image12.jpg))


Haga doble clic en el nombre de la base de datos y elija la opción "Publicar en proveedor" en el menú contextual. Esto inicia el Asistente para publicación de base de datos (Véase la figura 5). Haga clic en siguiente para ir más allá de la pantalla de presentación.


[![La pantalla de bienvenida del Asistente para la publicación de la base de datos](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**Figura 5**: base de datos de la pantalla de la presentación del Asistente para publicación ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image15.jpg))


La segunda pantalla del asistente muestra las bases de datos accesibles para el Asistente para publicación de base de datos y le permite elegir si desea incluir todos los objetos en la base de datos seleccionada o decidir qué objetos al script. Seleccione la base de datos adecuada y deje activada la opción "Script todos los objetos en la base de datos seleccionado".

> [!NOTE]
> Si se produce un error "no hay ningún objeto de base de datos *databaseName* de los tipos que permiten ejecutar scripts mediante este asistente" al hacer clic en siguiente en la pantalla se muestra en la figura 6, asegúrese de que la ruta de acceso al archivo de base de datos no es demasiado largo. Como se indicó en [este elemento de discusión](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014) en la página de proyecto de Asistente para publicación de base de datos, este error puede producirse si la ruta de acceso al archivo de base de datos es demasiado largo.


[![La pantalla de bienvenida del Asistente para la publicación de la base de datos](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**Figura 6**: base de datos de la pantalla de la presentación del Asistente para publicación ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image18.jpg))


En la pantalla siguiente puede generar un archivo de script o, si el host de web lo admite, publicar la base de datos directamente en el servidor de base de datos web s de proveedor de host. Como se muestra en la figura 7, tengo la secuencia de comandos que se escriben en el archivo `C:\REVIEWS.MDF.sql`.


[![La base de datos a un archivo de script o publicarlo directamente en el proveedor de hospedaje Web](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**Figura 7**: la base de datos a un archivo de Script o publicarlo directamente en el proveedor de hospedaje Web ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image21.jpg))


La pantalla subsiguientes en el que se le pide que para una amplia variedad de opciones de scripting. Puede especificar si la secuencia de comandos debe incluir las instrucciones drop para quitar estos objetos existentes. El valor predeterminado es True, que es un problema al implementar una base de datos por primera vez. También puede especificar si la base de datos de destino es SQL Server 2000, SQL Server 2005 o SQL Server 2008. Por último, puede indicar si se debe incluir el esquema y los datos, solo los datos, o simplemente el esquema. El esquema es la colección de objetos de base de datos, las tablas, procedimientos almacenados, vistas y así sucesivamente. Los datos son la información que reside en las tablas.

Como se muestra en la figura 8, ¿tenemos configurado para colocar los objetos de base de datos existente, el Asistente para generar el script para una base de datos de SQL Server 2008 y para publicar el esquema y los datos.


[![Especifique la publicación opciones](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**Figura 8**: especificar las opciones de publicación ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image24.jpg))


Las dos pantallas finales resumen las acciones que van a realizarse y, a continuación, mostrar el estado de la secuencia de comandos. El resultado de ejecutar al asistente es que tenemos un archivo de script que contiene los comandos SQL necesarios para crear la base de datos en producción y rellenarla con los mismos datos en el desarrollo.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Ejecutar los comandos SQL en la base de datos del entorno de producción

Ahora que tenemos es la secuencia de comandos que contiene los comandos SQL para crear la base de datos y sus datos que permanece ejecutar la secuencia de comandos en la base de datos de producción. Algunos proveedores de host web ofrecen un cuadro de texto en su Panel de Control, donde puede escribir comandos SQL que se ejecutarán en la base de datos. Si tiene un archivo de script muy grandes, a continuación, esta opción no funciona (la `REVIEWS.MDF.sql` archivo de script es más 425 KB de tamaño, por ejemplo).

Un enfoque más adecuado consiste en conectar directamente al servidor de base de datos de producción mediante SQL Server Management Studio (SSMS). Si tiene una no - Express Edition de SQL Server instalada en el equipo, a continuación, es probable que ya tiene instalado de SSMS. En caso contrario, puede [descargar e instalar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) una copia gratuita de SQL Server Management Studio Express Edition.

Inicie SSMS y conéctese a su servidor de base de datos de web host s usando la información proporcionada por el proveedor de host web.


[![Conectarse a su servidor de base de datos de Web Host proveedor s](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**Figura 9**: conectarse a su proveedor de hospedaje Web s. el servidor de base de datos ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image27.jpg))


Expanda la pestaña bases de datos y busque la base de datos. Haga clic en el botón nueva consulta en la esquina superior izquierda de la barra de herramientas, pegue en los comandos SQL desde el archivo de script creado por el Asistente para publicación de base de datos y haga clic en el botón Execute para ejecutar estos comandos en el servidor de base de datos de producción. Si el archivo de script es muy grande puede tardar varios minutos para ejecutar los comandos.


[![Conectarse a su servidor de base de datos de Web Host proveedor s](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**Figura 10**: conectarse a su proveedor de hospedaje Web s. el servidor de base de datos ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image30.jpg))


S todo es a él. En este momento se duplicó la base de datos de desarrollo a producción. Si actualiza la base de datos en SSMS debería ver los nuevos objetos de base de datos. Figura 11 muestra las tablas de s de la base de datos de producción, los procedimientos almacenados y funciones definidas por el usuario, que reflejan los de la base de datos de desarrollo. Y dado que se le solicita el Asistente para publicación de base de datos para publicar los datos, las tablas de s de base de datos de producción tienen los mismos datos que las tablas de s de base de datos de desarrollo en el momento en que se ejecutó el asistente. La figura 12 muestra los datos en el `Books` tabla en la base de datos de producción.


[![Los objetos de base de datos se han duplicado en la base de datos de producción](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**Figura 11**: la base de datos de objetos se han duplicado en la base de datos de producción ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image33.jpg))


[![La base de datos de producción contiene los mismos datos que en la base de datos de desarrollo](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**Figura 12**: la base de datos de producción contiene los mismos datos que en la base de datos de desarrollo ([haga clic aquí para ver la imagen a tamaño completo](deploying-a-database-vb/_static/image36.jpg))


En este momento sólo hemos implementado la base de datos de desarrollo a producción. Aún no nos hemos examinando la implementación de la propia aplicación web o examinar los cambios de configuración son necesarios para que la aplicación en producción y utilice la base de datos de producción. ¡Hablaremos sobre estos problemas en el siguiente tutorial!

## <a name="summary"></a>Resumen

Implementar una aplicación web controlada por datos requiere copiar la base de datos que se utiliza durante el desarrollo en el entorno de producción. Muchos proveedores de host de web ofrecen herramientas para simplificar el proceso de implementación de una base de datos. Por ejemplo, con DiscountASP.NET puede FTP la base de datos `.mdf` archivo (o una copia de seguridad) y, a continuación, adjuntar la base de datos en el servidor de base de datos desde el Panel de Control. Otra opción que funciona con independencia de qué características ofrece su proveedor de host web es la herramienta de Asistente para publicación de base de datos de Microsoft s, que genera un script de comandos SQL para crear el esquema de s de base de datos de desarrollo y los datos. Una vez que se ha generado esta secuencia de comandos se pueden ejecutar en la base de datos de producción.

Ahora que la base de datos de reseñas de libros web aplicación s está en producción y se puede implementar la aplicación. Sin embargo, la información de la configuración de la aplicación s web especifica la cadena de conexión a la base de datos, y esa cadena de conexión hace referencia a la base de datos de desarrollo. Necesitamos actualizar esta información de la cadena de conexión cuando se implemente el sitio de producción. El siguiente tutorial examina estas diferencias de configuración y le guía a través de los pasos necesarios para publicar el sitio de reseñas de libros controladas por datos en producción.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Descargar la base de datos de Microsoft SQL Server 1.1 de Asistente para publicación](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Descargar Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

>[!div class="step-by-step"]
[Anterior](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
[Siguiente](configuring-the-production-web-application-to-use-the-production-database-vb.md)
