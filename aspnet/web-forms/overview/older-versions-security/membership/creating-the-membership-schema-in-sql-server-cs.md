---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Crear el esquema de pertenencia en SQL Server (C#) | Documentos de Microsoft
author: rick-anderson
description: Este tutorial se inicia mediante el examen de técnicas para agregar el esquema necesario para la base de datos para poder usar SqlMembershipProvider. A continuación, se wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 4fa0476ca8336b56340dd177f9816acbe015ef7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Crear el esquema de pertenencia en SQL Server (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Este tutorial se inicia mediante el examen de técnicas para agregar el esquema necesario para la base de datos para poder usar SqlMembershipProvider. A continuación, agregaremos examine las tablas de clave en el esquema y describe su finalidad ni su importancia. Este tutorial finaliza con un vistazo a cómo saber qué proveedor debe usar el marco de trabajo de la pertenencia de una aplicación ASP.NET.


## <a name="introduction"></a>Introducción

Los dos tutoriales anteriores examinados utilizando la autenticación de formularios para identificar los visitantes del sitio Web. El marco de trabajo de autenticación de formularios facilita a los desarrolladores a un usuario inicie sesión en un sitio Web y recordar a ellos a través de visitas de página mediante el uso de vales de autenticación. La `FormsAuthentication` clase incluye métodos para generar el vale y éste se agrega a las cookies del visitante. El `FormsAuthenticationModule` examina todas las solicitudes entrantes y, para aquellos con un vale de autenticación válido, crea y asocia un `GenericPrincipal` y un `FormsIdentity` objeto con la solicitud actual. Autenticación mediante formularios es simplemente un mecanismo para conceder un vale de autenticación para un visitante al iniciar sesión en y, en las solicitudes subsiguientes, analizar ese vale para determinar la identidad del usuario. Para que una aplicación web admitir las cuentas de usuario, todavía es necesario implementar un almacén de usuario y agregar funcionalidad para validar las credenciales, registre los nuevos usuarios y los cientos de otras tareas relacionadas con la cuenta de usuario.

Antes de ASP.NET 2.0, los desarrolladores se encontraban en el enlace para la implementación de todas estas tareas relacionadas con la cuenta de usuario. Afortunadamente, el equipo ASP.NET reconoce este inconveniente e incorporó el marco de trabajo de pertenencia con ASP.NET 2.0. El marco de trabajo de pertenencia es un conjunto de clases de .NET Framework que proporcionan una interfaz de programación para realizar tareas relacionadas con la cuenta de usuario principales. Este marco de trabajo se crea a partir del [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite a los desarrolladores conecte una implementación personalizada a una API estándar.

Como se describe en el <a id="Tutorial1"> </a> [ *principios básicos de seguridad y compatibilidad con ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) tutorial, .NET Framework incluye dos proveedores de pertenencia integrados: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) y [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Como su nombre implica, el `SqlMembershipProvider` utiliza una base de datos de Microsoft SQL Server como almacén de usuario. Para utilizar este proveedor en una aplicación, es necesario indicar al proveedor de la base de datos que se usará como el almacén. Como puede imaginar, la `SqlMembershipProvider` espera que la base de datos del almacén de usuario tienen ciertas tablas de base de datos, vistas y procedimientos almacenados. Tenemos que agregar este esquema esperado para la base de datos seleccionada.

Este tutorial se inicia mediante el examen de técnicas para agregar el esquema necesario para la base de datos para poder usar el `SqlMembershipProvider`. A continuación, agregaremos examine las tablas de clave en el esquema y describe su finalidad ni su importancia. Este tutorial finaliza con un vistazo a cómo saber qué proveedor debe usar el marco de trabajo de la pertenencia de una aplicación ASP.NET.

Comencemos.

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Paso 1: Decidir dónde ubicar el almacén de usuario

Datos de una aplicación ASP.NET se almacenan normalmente en un número de tablas en una base de datos. Al implementar el `SqlMembershipProvider` debemos decidir si se debe colocar el esquema de pertenencia en la misma base de datos como los datos de aplicación o en una base de datos alternativa de esquema de base de datos.

Recomienda buscar el esquema de la pertenencia a la misma base de datos como los datos de aplicación por las razones siguientes:

- **Mantenimiento** una aplicación cuyos datos se encapsulan en una base de datos es más fácil de entender, mantener e implementar que una aplicación que tiene dos bases de datos.
- **Integridad relacional** al ubicar las tablas basadas en la pertenencia a la misma base de datos como tablas de la aplicación es posible establecer [restricciones foreign key](http://en.wikipedia.org/wiki/Foreign_key) entre las claves principales en el Tablas relacionadas con la pertenencia y tablas de aplicación relacionada.

Separar los datos de almacén y aplicación de usuario en las bases de datos independientes solo tiene sentido, si tiene varias aplicaciones que cada uno de ellos utilizar bases de datos independientes, pero tienen que compartir un almacén de usuario común.

### <a name="creating-a-database"></a>Crear una base de datos

La aplicación que compilamos dado que el segundo tutorial no necesita aún una base de datos. Necesitamos uno ahora, sin embargo, para el almacén del usuario. Vamos a crear uno y, a continuación, agregarle el esquema que necesita la `SqlMembershipProvider` proveedor (vea el paso 2).

> [!NOTE]
> A lo largo de esta serie de tutoriales se va a utilizar un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) base de datos para almacenar las tablas de la aplicación y la `SqlMembershipProvider` esquema. Esta decisión se toma por dos motivos: en primer lugar, debido a su costo - libre - Express Edition es la versión más readably accesible de SQL Server 2005; en segundo lugar, se pueden colocar las bases de datos de SQL Server 2005 Express Edition directamente en la aplicación web `App_Data` carpeta, lo que rápida y fácilmente a la base de datos del paquete y aplicación web juntos en un archivo ZIP y volver a implementar sin las instrucciones de instalación especiales u opciones de configuración. Si prefiere seguir el tutorial utiliza una versión no - Express Edition de SQL Server, no dude. Los pasos son prácticamente idénticos. El `SqlMembershipProvider` esquema trabajará con cualquier versión de Microsoft SQL Server 2000 y una copia de seguridad.


En el Explorador de soluciones, haga doble clic en el `App_Data` carpeta y elija Agregar nuevo elemento. (Si no ve una `App_Data` carpeta en el proyecto, haga doble clic en el proyecto en el Explorador de soluciones, seleccione Agregar carpeta ASP.NET y elegir `App_Data`.) En el cuadro de diálogo Agregar nuevo elemento, optar por agregar una nueva base de datos de SQL denominado `SecurityTutorials.mdf`. En este tutorial agregaremos la `SqlMembershipProvider` esquema para esta base de datos; en los tutoriales posteriores crearemos más tablas para capturar los datos de aplicación.


[![Agregar una nueva base de datos SQL con el nombre de base de datos de SecurityTutorials.mdf a la carpeta App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: agregar una nueva base de datos de SQL denominada `SecurityTutorials.mdf` base de datos a la `App_Data` carpeta ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Agregar una base de datos a la `App_Data` carpeta incluye automáticamente en la vista del explorador de base de datos. (En la versión no - Express Edition de Visual Studio, el Explorador de base de datos se denomina el Explorador de servidores). Vaya al explorador de base de datos y expanda el recién agregados `SecurityTutorials` base de datos. Si no ve el Explorador de base de datos en pantalla, vaya al menú Ver y elija Explorador de base de datos o Ctrl + Alt + S de llamadas. Como se muestra en la figura 2, el `SecurityTutorials` base de datos está vacía: contiene ninguna tabla, no hay ninguna vista y ningún procedimiento almacenado.


[![La base de datos de SecurityTutorials está vacío](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: el `SecurityTutorials` base de datos está vacía actualmente ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Paso 2: Agregar el`SqlMembershipProvider`esquema para la base de datos

El `SqlMembershipProvider` requiere un conjunto determinado de tablas, vistas y procedimientos almacenados que se instalen en la base de datos de almacén de usuario. Estos objetos de base de datos necesarios se pueden agregar utilizando la [ `aspnet_regsql.exe` herramienta](https://msdn.microsoft.com/library/ms229862.aspx). Este archivo se encuentra en la `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` carpeta.

> [!NOTE]
> El `aspnet_regsql.exe` herramienta ofrece funcionalidad de línea de comandos y una interfaz gráfica de usuario. La interfaz gráfica es más descriptivo y es lo que se examinará en este tutorial. La interfaz de línea de comandos es útil cuando la adición de la `SqlMembershipProvider` esquema debe ser automatizada, como se muestra en la compilación de scripts o automatizar escenarios de prueba.


El `aspnet_regsql.exe` herramienta se usa para agregar o quitar *los servicios de aplicación de ASP.NET* a una base de datos de SQL Server especificada. Los servicios de aplicación de ASP.NET abarcan los esquemas para la `SqlMembershipProvider` y `SqlRoleProvider`, junto con los esquemas para los proveedores de basado en SQL para otros marcos de trabajo de ASP.NET 2.0. Debemos proporcionar dos bits de información para el `aspnet_regsql.exe` herramienta:

- Si desea agregar o quitar servicios de aplicaciones, y
- La base de datos que se va a agregar o quitar el esquema de servicios de aplicación

En pedir la base de datos que se utiliza el `aspnet_regsql.exe` herramienta le pide que proporcione el nombre del servidor en que reside la base de datos, las credenciales de seguridad para conectarse a la base de datos y el nombre de base de datos. Si usas la no - Express Edition de SQL Server, ya debe conocer esta información, ya que es la misma información que se debe proporcionar a través de una cadena de conexión cuando se trabaja con la base de datos a través de una página web ASP.NET. Determinar el nombre del servidor y base de datos cuando se usa una base de datos de SQL Server 2005 Express Edition en el `App_Data` carpeta, sin embargo, es un poco más complicada.

En la siguiente sección examina una manera sencilla para especificar el nombre de servidor y base de datos para una base de datos de SQL Server 2005 Express Edition en el `App_Data` carpeta. Si no está usando SQL Server 2005 Express Edition Llamarme ir directamente a la instalación la sección de servicios de aplicaciones.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Determinar el servidor y el nombre de la base de datos para una base de datos SQL Server 2005 Express Edition en el`App_Data`carpeta

Para poder usar el `aspnet_regsql.exe` herramienta es necesario conocer los nombres de servidor y base de datos. El nombre del servidor es `localhost\InstanceName`. Probablemente, el *nombreDeInstancia* es `SQLExpress`. Sin embargo, si ha instalado SQL Server 2005 Express Edition manualmente (es decir, no instala automáticamente durante la instalación de Visual Studio), es posible que haya seleccionado un nombre de instancia diferente.

El nombre de la base de datos es un poco más complicado determinar. Las bases de datos en el `App_Data` carpeta suelen tener un nombre de base de datos que incluya un [identificador único global](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) junto con la ruta de acceso al archivo de base de datos. Es necesario determinar este nombre de base de datos con el fin de agregar el esquema de servicios de aplicación a través de `aspnet_regsql.exe`.

La manera más fácil de determinar el nombre de la base de datos es examinar mediante SQL Server Management Studio. SQL Server Management Studio proporciona una interfaz gráfica para administrar las bases de datos de SQL Server 2005, pero no se distribuye con Express Edition de SQL Server 2005. Las buenas noticias son que [puede descargar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) la libre Express Edition de SQL Server Management Studio.

> [!NOTE]
> Si también tiene una versión no - Express Edition de SQL Server 2005 instalado en el escritorio, es probable que se instala la versión completa de Management Studio. Puede usar la versión completa para determinar el nombre de la base de datos, siguiendo los mismos pasos tal como se describe a continuación para la edición Express.


Empiece por cerrar Visual Studio para asegurarse de que se cierran los bloqueos impuestos por Visual Studio en el archivo de base de datos. A continuación, inicie SQL Server Management Studio y conéctese a la `localhost\InstanceName` base de datos de SQL Server 2005 Express Edition. Como se indicó anteriormente, lo más probable es que el nombre de instancia es `SQLExpress`. Para la opción de autenticación, seleccione autenticación de Windows.


[![Conectarse a la instancia SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: conectarse a la instancia de SQL Server 2005 Express Edition ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Después de conectarse a la instancia de SQL Server 2005 Express Edition, Management Studio muestra las carpetas para las bases de datos, la configuración de seguridad, los objetos de servidor y así sucesivamente. Si expande la pestaña bases de datos, verá que el `SecurityTutorials.mdf` base de datos es *no* registrada en la instancia de base de datos - necesitamos adjuntar la base de datos en primer lugar.

Haga doble clic en la carpeta de bases de datos y elija adjuntar en el menú contextual. Se mostrará el cuadro de diálogo Adjuntar bases de datos. Desde aquí, haga clic en el botón Agregar, vaya a la `SecurityTutorials.mdf` la base de datos y haga clic en Aceptar. La figura 4 muestra el cuadro de diálogo Adjuntar bases de datos después de la `SecurityTutorials.mdf` se ha seleccionado la base de datos. Figura 5 se muestra el Explorador de objetos de Management Studio después de que se haya adjuntado correctamente la base de datos.


[![Adjunte la base de datos SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: adjuntar el `SecurityTutorials.mdf` base de datos ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![La base de datos de SecurityTutorials.mdf aparece en la carpeta de bases de datos](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: el `SecurityTutorials.mdf` base de datos aparece en la carpeta de bases de datos ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Como se muestra en la figura 5, el `SecurityTutorials.mdf` base de datos tiene un nombre en su lugar ocultas. Vamos a cambiarlo a una más fácil de recordar (y más fácil de escribir) nombre. Haga doble clic en la base de datos, elija el cambio de nombre en el menú contextual y cámbiele el nombre `SecurityTutorialsDatabase`. Esto no cambia el nombre de archivo, solo el nombre de la base de datos se usa para identificarse a SQL Server.


[![Cambiar el nombre de la base de datos SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: cambiar el nombre de la base de datos `SecurityTutorialsDatabase`([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


En este momento se conocen los nombres de servidor y base de datos para la `SecurityTutorials.mdf` archivo de base de datos: `localhost\InstanceName` y `SecurityTutorialsDatabase`, respectivamente. Ahora estamos listos instalar los servicios de aplicación a través de la `aspnet_regsql.exe` herramienta.

### <a name="installing-the-application-services"></a>Instalar los servicios de aplicación

Para iniciar el `aspnet_regsql.exe` herramienta, vaya al menú Inicio y elija ejecutar. Escriba `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` en el cuadro de texto y haga clic en Aceptar. Como alternativa, puede usar el Explorador de Windows para profundizar en la carpeta correspondiente y haga doble clic en el `aspnet_regsql.exe` archivo. Cualquier enfoque cumplirá los mismos resultados.

Ejecuta el `aspnet_regsql.exe` herramienta sin ningún argumento de línea de comandos inicia la interfaz gráfica de usuario de Asistente para la instalación de ASP.NET SQL Server. El asistente facilita agregar o quitar los servicios de aplicación de ASP.NET en una base de datos especificada. La primera pantalla del asistente, que se muestra en la figura 7, describe el propósito de la herramienta.


[![Use el hace de Asistente para el programa de instalación de ASP.NET SQL Server para agregar el esquema de pertenencia](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: usar ASP.NET SQL Server el programa de instalación asistente facilita para agregar el esquema de pertenencia ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


El segundo paso en el Asistente para nosotros le pregunta si desea quitar o agregar los servicios de aplicación. Puesto que deseamos agregar tablas, vistas y procedimientos almacenados necesarios para el `SqlMembershipProvider`, elija Configurar SQL Server para la opción de servicios de aplicación. Más adelante, si desea quitar este esquema de la base de datos, vuelva a ejecutar a este asistente, pero en su lugar, elija la información de servicios de aplicación de quitar de una opción de base de datos existente.


[![Elija la configurar SQL Server para la opción de servicios de aplicación](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: elija Configurar SQL Server para la opción de servicios de aplicación ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


El tercer paso solicita la información de la base de datos: el nombre del servidor, información de autenticación y el nombre de la base de datos. Si después de este tutorial y ha agregado el `SecurityTutorials.mdf` a la base de datos `App_Data`, adjunta a `localhost\InstanceName`y cambia su nombre a `SecurityTutorialsDatabase`, a continuación, utilice los siguientes valores:

- Servidor: `localhost\InstanceName`
- Autenticación de Windows
- Base de datos: `SecurityTutorialsDatabase`


[![Escriba la información de la base de datos](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: escriba la información de la base de datos ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Después de escribir la información de la base de datos, haga clic en siguiente. El último paso resume los pasos que se van a realizar. Haga clic en siguiente para instalar los servicios de aplicación y, a continuación, Finalizar para completar al asistente.

> [!NOTE]
> Si utiliza Management Studio para adjuntar la base de datos y el nombre del archivo de base de datos, asegúrese de separar la base de datos y cierre Management Studio antes de volver a abrir Visual Studio. Para separar el `SecurityTutorialsDatabase` de base de datos, haga doble clic en el nombre de la base de datos y, en el menú de tareas, elija Desasociar.


Cuando se complete el asistente, vuelva a Visual Studio y navegue hasta el Explorador de base de datos. Expanda la carpeta tablas. Debería ver una serie de tablas cuyos nombres empiezan por el prefijo `aspnet_`. Del mismo modo, una variedad de vistas y procedimientos almacenados puede encontrarse en las carpetas de vistas y procedimientos almacenados. Estos objetos de base de datos forman el esquema de servicios de aplicación. Examinaremos los objetos de base de datos de pertenencia y rol específico en el paso 3.


[![Una variedad de tablas, vistas y procedimientos almacenados se han agregado a la base de datos](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: una variedad de tablas, vistas y almacenan los procedimientos se han agregado a la base de datos ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> El `aspnet_regsql.exe` interfaz gráfica de usuario de la herramienta instala el esquema de servicios de toda la aplicación. Pero cuando se ejecuta `aspnet_regsql.exe` desde la línea de comandos puede especificar qué aplicación determinada de servicios de componentes para instalar (o quitar). Por lo tanto, si desea agregar solo las tablas, vistas y se almacenan los procedimientos necesarios para la `SqlMembershipProvider` y `SqlRoleProvider` proveedores, ejecutar `aspnet_regsql.exe` desde la línea de comandos. Como alternativa, puede ejecutar manualmente el subconjunto de T-SQL, crear scripts usados por `aspnet_regsql.exe`. Estos scripts se encuentran en el `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` carpeta con nombres como `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`, y así sucesivamente.


En este punto hemos creado los objetos de base de datos que necesitan la `SqlMembershipProvider`. Sin embargo, todavía necesitamos indicar que el marco de trabajo de pertenencia que debe usar el `SqlMembershipProvider` (frente a, diga, el `ActiveDirectoryMembershipProvider`) y que la `SqlMembershipProvider` debe utilizar el `SecurityTutorials` base de datos. Analizaremos cómo especificar qué proveedor se debe usar y cómo personalizar la configuración del proveedor seleccionado en el paso 4. Pero primero, echemos un vistazo más profundo de los objetos de base de datos que acaba de crear.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Paso 3: Un vistazo a las tablas del esquema principal

Cuando se trabaja con los marcos de pertenencia y los Roles en una aplicación ASP.NET, los detalles de implementación están encapsulados por el proveedor. En el futuro los tutoriales que se va a interactuar con estos marcos de trabajo a través de .NET Framework `Membership` y `Roles` clases. Al usar estas API de alto nivel no necesitamos concernientes a nosotros mismos con los detalles de bajo nivel, como las consultas que se ejecutan o se modifican las tablas mediante la `SqlMembershipProvider` y `SqlRoleProvider`.

Por tanto, podríamos usar los marcos de pertenencia y los Roles con confianza sin necesidad de explorar el esquema de base de datos creado en el paso 2. Sin embargo, al crear las tablas para almacenar datos de aplicación, podemos necesitamos crear entidades relacionadas con usuarios o roles. Resulta útil para tener algunos conocimientos sobre la `SqlMembershipProvider` y `SqlRoleProvider` esquemas al establecer externa clave restricciones entre las tablas de datos de aplicación y las tablas que creó en el paso 2. Además, en determinadas circunstancias poco habituales podemos necesitar interactuar con el usuario y rol almacena directamente en el nivel de base de datos (en lugar de mediante la `Membership` o `Roles` clases).

### <a name="partitioning-the-user-store-into-applications"></a>Creación de particiones el almacén del usuario en aplicaciones

Los marcos de pertenencia y los Roles se diseñan de forma que un único almacén de usuario y el rol se puede compartir entre muchas aplicaciones diferentes. Una aplicación de ASP.NET que usa los marcos de pertenencia o Roles debe especificar qué partición de aplicación que se utilizará. En resumen, varias aplicaciones web pueden usar los mismos almacenes de usuario y el rol. Figura 11 muestra los almacenes de usuario y el rol que se dividen en tres aplicaciones: HRSite, CustomerSite y SalesSite. Estas aplicaciones tres web tienen sus propios usuarios únicos y roles, pero todas ellas almacenan físicamente su información de cuenta y el rol de usuario de las mismas tablas de base de datos.


[![Las cuentas de usuario pueden dividirse en varias aplicaciones](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: cuentas pueden ser particiones a través de varias aplicaciones de usuario ([haga clic aquí para ver la imagen a tamaño completo](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


El `aspnet_Applications` tabla es lo que define estas particiones. Cada aplicación que utiliza la base de datos para almacenar información de la cuenta de usuario está representado por una fila en esta tabla. El `aspnet_Applications` tabla tiene cuatro columnas: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, y `Description`. `ApplicationId` es de tipo [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) y es la clave principal de la tabla; `ApplicationName` proporciona un nombre fácil de usar único para cada aplicación.

Vinculan las tablas relacionadas con el rol y la pertenencia a la `ApplicationId` campo `aspnet_Applications`. Por ejemplo, el `aspnet_Users` tabla, que contiene un registro para cada cuenta de usuario, tiene un `ApplicationId` campo de clave externa; ditto para el `aspnet_Roles` tabla. El `ApplicationId` campo en estas tablas especifica la partición de aplicación de la cuenta de usuario o rol al que pertenece.

### <a name="storing-user-account-information"></a>Almacenar información de la cuenta de usuario

Información de la cuenta de usuario se aloja en dos tablas: `aspnet_Users` y `aspnet_Membership`. El `aspnet_Users` tabla contiene campos que contienen la información de la cuenta de usuario esenciales. Las tres columnas más importantes son:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` es la clave principal (y del tipo `uniqueidentifier`). `UserName` es de tipo `nvarchar(256)` y, junto con la contraseña constituye las credenciales del usuario. (Contraseña de un usuario se almacena en la `aspnet_Membership` tabla.) `ApplicationId` la cuenta de usuario se vincula a una aplicación determinada en `aspnet_Applications`. Hay un compuesto [ `UNIQUE` restricción](https://msdn.microsoft.com/library/ms191166.aspx) en el `UserName` y `ApplicationId` columnas. Esto garantiza que en una aplicación determinada, cada nombre de usuario es único, aunque permite para el mismo `UserName` para su uso en aplicaciones diferentes.

El `aspnet_Membership` tabla incluye información de la cuenta de usuario adicionales, como la contraseña del usuario, dirección de correo electrónico, la última fecha de inicio de sesión y tiempo y así sucesivamente. Hay una correspondencia exacta entre los registros de la `aspnet_Users` y `aspnet_Membership` tablas. Esta relación se garantiza mediante la `UserId` campo `aspnet_Membership`, que actúa como clave principal de la tabla. Al igual que el `aspnet_Users` tabla, `aspnet_Membership` incluye un `ApplicationId` campo que une esta información a una partición de aplicación en particular.

### <a name="securing-passwords"></a>Proteger las contraseñas

Información de contraseña se almacena en la `aspnet_Membership` tabla. El `SqlMembershipProvider` permite que las contraseñas que se almacenará en la base de datos mediante uno de las tres técnicas siguientes:

- **Desactive** -la contraseña se almacena en la base de datos como texto sin formato. Desaconseja con esta opción. Si la base de datos se ve comprometido - ya sea por un usuario malintencionado que busca una puerta trasera o un empleado descontento que tiene acceso de la base de datos - las credenciales de cada usuario solo existen para la toma.
- **Aplica un algoritmo hash** -las contraseñas se envían con hash utilizando un algoritmo hash unidireccional y un valor salt generado aleatoriamente. Este valor de hash (junto con el valor "salt") se almacena en la base de datos.
- **Cifrado** -una versión cifrada de la contraseña se almacena en la base de datos.

En función de la técnica de almacenamiento de la contraseña utilizada en el `SqlMembershipProvider` configuración especificada en `Web.config`. Examinaremos personalizar el `SqlMembershipProvider` configuración en el paso 4. El comportamiento predeterminado consiste en almacenar el hash de la contraseña.

Las columnas responsables de almacenar la contraseña son `Password`, `PasswordFormat`, y `PasswordSalt`. `PasswordFormat` es un campo de tipo `int` cuyo valor indica la técnica que se utiliza para almacenar la contraseña: 0 para borrar; 1 para Hashed; 2 para el cifrado. `PasswordSalt` se asigna una cadena generada de forma aleatoria, independientemente de la técnica de almacenamiento de la contraseña utilizada; el valor de `PasswordSalt` solo se usa al calcular el hash de la contraseña. Por último, la `Password` columna contiene los datos de la contraseña real, ya sea la contraseña de texto sin formato, el hash de la contraseña o la contraseña cifrada.

Tabla 1 se muestran estas tres columnas quedará para las distintas técnicas de almacenamiento al almacenar la contraseña MiSecreto! .

| **Técnica de almacenamiento&lt;\_o3a\_p /&gt;** | **Password&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | ¡MiSecreto! | 0 | tTnkPlesqissc2y2SMEygA== |
| Aplica un algoritmo hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Cifrado | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabla 1**: valores de ejemplo para los campos relacionados con la contraseña al almacenar la contraseña MiSecreto!

> [!NOTE]
> El cifrado determinado o el algoritmo hash utilizado por el `SqlMembershipProvider` viene determinado por la configuración en el `<machineKey>` elemento. Analizamos este elemento de configuración en el paso 3 de la <a id="Tutorial3"> </a> [ *configuración de autenticación de formularios y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutorial.


### <a name="storing-roles-and-role-associations"></a>Almacenar los Roles y las asociaciones de rol

El marco de trabajo de Roles permite a los desarrolladores definir un conjunto de roles y especificar qué usuarios pertenecen a qué funciones. Esta información se captura en la base de datos a través de dos tablas: `aspnet_Roles` y `aspnet_UsersInRoles`. Cada registro de la `aspnet_Roles` tabla representa un rol para una aplicación concreta. Muy parecida a la `aspnet_Users` tabla, el `aspnet_Roles` tabla tiene tres columnas pertinentes para nuestro análisis:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` es la clave principal (y del tipo `uniqueidentifier`). `RoleName` es de tipo `nvarchar(256)`. Y `ApplicationId` la cuenta de usuario se vincula a una aplicación determinada en `aspnet_Applications`. Hay un compuesto `UNIQUE` restricción en la `RoleName` y `ApplicationId` columnas, asegurándose de que en una aplicación determinada cada nombre de rol es único.

El `aspnet_UsersInRoles` tabla actúa como una asignación entre usuarios y roles. Hay solo dos columnas - `UserId` y `RoleId` - y juntas constituyen una clave principal compuesta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Paso 4: Especificar el proveedor y personalizar su configuración

Todos los marcos que admiten el modelo de proveedor, como los marcos de pertenencia y los Roles - carecen de detalles de implementación por sí mismos y en su lugar delegar esa responsabilidad a una clase de proveedor. En el caso del marco de pertenencia, el `Membership` clase define la API para administrar cuentas de usuario, pero no interactúa directamente con ningún almacén de usuario. En su lugar, el `Membership` métodos entregar la clase la solicitud al proveedor configurado - utilizaremos el `SqlMembershipProvider`. Cuando se invoca uno de los métodos en el `Membership` (clase), ¿cómo el marco de trabajo de pertenencia sabe para delegar la llamada a la `SqlMembershipProvider`?

El `Membership` clase tiene un [ `Providers` propiedad](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) que contiene una referencia a todas las clases de proveedor registrado disponibles para su uso por el marco de trabajo de pertenencia. Cada proveedor registrado tiene un nombre de asociado y el tipo. El nombre proporciona una manera fácil de usar para hacer referencia a un proveedor determinado en el `Providers` colección, mientras que el tipo identifica la clase de proveedor. Además, cada proveedor registrado puede incluir valores de configuración. Valores de configuración para el marco de trabajo de pertenencia incluyen `passwordFormat` y `requiresUniqueEmail`, entre otros muchos. Consulte la tabla 2 para obtener una lista completa de valores de configuración usados por la `SqlMembershipProvider`.

La `Providers` contenido de la propiedad se especifica mediante los valores de configuración de la aplicación web. De forma predeterminada, todas las aplicaciones web tienen un proveedor denominado `AspNetSqlMembershipProvider` de tipo `SqlMembershipProvider`. Este proveedor de pertenencia predeterminado está registrado en `machine.config` (ubicado en `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Como el marcado anterior muestra, la [ `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx) define los valores de configuración para el marco de trabajo de la pertenencia a la [ `<providers>` elemento secundario](https://msdn.microsoft.com/library/6d4936ht.aspx) especifica registrado proveedores. Los proveedores pueden agregar o quitar mediante el [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) o [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementos; utilice la [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elemento que se va a todos los quitar actualmente proveedores registrados. Como el marcado anterior muestra, `machine.config` agrega un proveedor denominado `AspNetSqlMembershipProvider` de tipo `SqlMembershipProvider`.

Además el `name` y `type` atributos, los `<add>` elemento contiene atributos que definen los valores de configuración distintos. Tabla 2 se enumeran los contadores `SqlMembershipProvider`-opciones de configuración específicas, junto con una descripción de cada uno.

> [!NOTE]
> Los valores predeterminados que se indican en la tabla 2 hacen referencia a los valores predeterminados definidos en la `SqlMembershipProvider` clase. Tenga en cuenta que no todos los valores de configuración de `AspNetSqlMembershipProvider` corresponden a los valores predeterminados de la `SqlMembershipProvider` clase. Por ejemplo, si no se especifica en un proveedor de pertenencia, el `requiresUniqueEmail` establecer los valores predeterminados en true. Sin embargo, el `AspNetSqlMembershipProvider` reemplaza este valor predeterminado mediante la especificación explícita de un valor de `false`.


| **Establecer&lt;\_o3a\_p /&gt;** | **Descripción&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Recuerde que el marco de pertenencia permite que un almacén de usuario único tener particiones en varias aplicaciones. Este valor indica el nombre de la partición de aplicaciones utilizada por el proveedor de pertenencia. Si este valor no se especifica explícitamente, se establece en tiempo de ejecución, en el valor de ruta de acceso de la raíz virtual de la aplicación. |
| `commandTimeout` | Especifica el valor de tiempo de espera de comando SQL (en segundos). El valor predeterminado es 30. |
| `connectionStringName` | El nombre de la cadena de conexión en el `<connectionStrings>` elemento que se va a usar para conectarse a la base de datos de almacén de usuario. Este valor es obligatorio. |
| `description` | Proporciona una descripción fácil de usar del proveedor registrado. |
| `enablePasswordRetrieval` | Especifica si los usuarios pueden recuperar su contraseña olvidada. El valor predeterminado es `false`. |
| `enablePasswordReset` | Indica si los usuarios pueden restablecer su contraseña. Tiene como valor predeterminado `true`. |
| `maxInvalidPasswordAttempts` | El número máximo de intentos de inicio de sesión incorrecto que puede producirse por un usuario determinado durante especificado `passwordAttemptWindow` antes de que el usuario está bloqueado. El valor predeterminado es 5. |
| `minRequiredNonalphanumericCharacters` | El número mínimo de caracteres no alfanuméricos que deben aparecer en una contraseña de usuario. Este valor debe estar entre 0 y 128; el valor predeterminado es 1. |
| `minRequiredPasswordLength` | El número mínimo de caracteres necesarios en una contraseña. Este valor debe estar entre 0 y 128; el valor predeterminado es 7. |
| `name` | El nombre del proveedor registrado. Este valor es obligatorio. |
| `passwordAttemptWindow` | El número de minutos durante los que no pudo se realiza el seguimiento de los intentos de inicio de sesión. Si un usuario proporciona credenciales de inicio de sesión no válido `maxInvalidPasswordAttempts` especifican de veces dentro de esta ventana, se les bloquea. El valor predeterminado es 10. |
| `PasswordFormat` | El formato de almacenamiento de contraseña: `Clear`, `Hashed`, o `Encrypted`. De manera predeterminada, es `Hashed`. |
| `passwordStrengthRegularExpression` | Si se proporciona, esta expresión regular se utiliza para evaluar la seguridad de la contraseña del usuario seleccionado al crear una nueva cuenta o al cambiar su contraseña. El valor predeterminado es una cadena vacía. |
| `requiresQuestionAndAnswer` | Especifica si un usuario debe responder su pregunta de seguridad al recuperar o restablecer su contraseña. El valor predeterminado es `true`. |
| `requiresUniqueEmail` | Indica si todas las cuentas de usuario en una partición determinada aplicación deben tener una dirección de correo electrónico única. El valor predeterminado es `true`. |
| `type` | Especifica el tipo del proveedor. Este valor es obligatorio. |

**Tabla 2**: pertenencia y `SqlMembershipProvider` valores de configuración

Además `AspNetSqlMembershipProvider`, otros proveedores de pertenencia pueden que esté registrados en una base de aplicación por aplicación mediante la adición de marcado similar a la `Web.config` archivo.

> [!NOTE]
> El marco de trabajo de Roles funciona en gran parte del mismo modo: no hay un proveedor de funciones registrado de forma predeterminada en `machine.config` y los proveedores registrados pueden personalizarse en una función de aplicación por aplicación en `Web.config`. Examinaremos el marco de trabajo de Roles y sus marcas de configuración en detalle en un tutorial posterior.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizar el`SqlMembershipProvider`configuración

El valor predeterminado `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) tiene su `connectionStringName` atributo establecido en `LocalSqlServer`. Al igual que el `AspNetSqlMembershipProvider` proveedor, el nombre de la cadena de conexión `LocalSqlServer` se define en `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Como puede ver, esta cadena de conexión define una base de datos se encuentra en SQL 2005 Express Edition | DataDirectory|aspnetdb. mdf. La cadena | DataDirectory | se traduce en tiempo de ejecución para que apunte a la `~/App_Data/` directorio, por lo que la ruta de acceso de base de datos | DataDirectory|aspnetdb. mdf"se traduce en `~/App_Data` / `aspnet.mdf`.

Si no ha especificado ninguna información de proveedor de pertenencia en nuestra aplicación `Web.config` archivo, la aplicación utiliza el proveedor de pertenencia predeterminado registrado, `AspNetSqlMembershipProvider`. Si el `~/App_Data/aspnet.mdf` base de datos no existe, el tiempo de ejecución ASP.NET creará automáticamente y agregue el esquema de servicios de aplicación. Sin embargo, no queremos usar el `aspnet.mdf` base de datos; en su lugar, desea usar el `SecurityTutorials.mdf` base de datos se creó en el paso 2. Esta modificación puede realizarse de dos maneras:

- <strong>Especifique un valor para el</strong><strong>`LocalSqlServer`</strong><strong>nombre de la cadena de conexión en</strong><strong>`Web.config`</strong><strong>.</strong> Sobrescribiendo el `LocalSqlServer` valor de nombre de la cadena de conexión en `Web.config`, podemos utilizar el proveedor de pertenencia predeterminado registrado (`AspNetSqlMembershipProvider`) y hacer que funcione correctamente con el `SecurityTutorials.mdf` base de datos. Este enfoque es adecuado si el contenido con los valores de configuración especificados por `AspNetSqlMembershipProvider`. Para obtener más información sobre esta técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [configurar servicios de aplicación de ASP.NET 2.0 para utilizar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Agregar un nuevo proveedor registrado del tipo</strong><strong>`SqlMembershipProvider`</strong><strong>y configurar su</strong><strong>`connectionStringName`</strong><strong>configuración para que apunte a la</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de datos.</strong> Este enfoque es útil en escenarios donde desea personalizar otras propiedades de configuración además de la cadena de conexión de base de datos. En Mis propio proyectos siempre use este enfoque debido a su flexibilidad y mejorar la legibilidad.

Antes de que podemos agregar un nuevo proveedor registrado que hace referencia a la `SecurityTutorials.mdf` base de datos, primero tenemos que agregar un valor de cadena de conexión adecuada en la `<connectionStrings>` sección `Web.config`. El marcado siguiente agrega una nueva cadena de conexión denominada `SecurityTutorialsConnectionString` que hace referencia a SQL Server 2005 Express Edition `SecurityTutorials.mdf` en la base de datos la `App_Data` carpeta.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Si se usa un archivo de base de datos alternativa, actualice la cadena de conexión según sea necesario. Para obtener más información sobre la formación de la cadena de conexión correcta, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).

A continuación, agregue el siguiente marcado de configuración de pertenencia a la `Web.config` archivo. Este marcado registra un proveedor nuevo denominado `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Además de registrar el `SecurityTutorialsSqlMembershipProvider` proveedor, el marcado anterior define la `SecurityTutorialsSqlMembershipProvider` como proveedor predeterminado (a través de la `defaultProvider` atributo el `<membership>` elemento). Recuerde que el marco de trabajo de suscripción puede tener varios de los proveedores registrados. Puesto que `AspNetSqlMembershipProvider` está registrado como el primer proveedor de `machine.config`, que actúa como el proveedor predeterminado a menos que se indique lo contrario.

En la actualidad, nuestra aplicación tiene dos proveedores registrados: `AspNetSqlMembershipProvider` y `SecurityTutorialsSqlMembershipProvider`. Sin embargo, antes de registrar el `SecurityTutorialsSqlMembershipProvider` proveedor se podríamos borraran todas previamente los proveedores registrados mediante la adición de un [ `<clear />` elemento](https://msdn.microsoft.com/library/t062y6yc.aspx) inmediatamente antes de nuestro `<add>` elemento. Esto podría borrar la `AspNetSqlMembershipProvider` en la lista de los proveedores registrados, lo que significa que el `SecurityTutorialsSqlMembershipProvider` sería el único proveedor de pertenencia registrado. Si se usa este enfoque, no se necesita marcar el `SecurityTutorialsSqlMembershipProvider` como el proveedor predeterminado, ya que es el único proveedor de pertenencia registrado. Para obtener más información sobre el uso de `<clear />`, consulte [mediante `<clear />` al agregar proveedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Tenga en cuenta que la `SecurityTutorialsSqlMembershipProvider`del `connectionStringName` establecer referencias a los recién agregados `SecurityTutorialsConnectionString` nombre de la cadena de conexión y que su `applicationName` configuración se ha establecido en un valor de SecurityTutorials. Además, el `requiresUniqueEmail` configuración se ha establecido en `true`. Todas las demás opciones de configuración son idénticos a los valores de `AspNetSqlMembershipProvider`. No dude en realice las modificaciones de configuración en este caso, si lo desea. Por ejemplo, puede reforzar la seguridad de la contraseña al requerir dos caracteres no alfanuméricos en lugar de uno, o mediante el aumento de la longitud de la contraseña a ocho caracteres en lugar de siete.

> [!NOTE]
> Recuerde que el marco de pertenencia permite que un almacén de usuario único tener particiones en varias aplicaciones. El proveedor de pertenencia `applicationName` valor indica que la aplicación utiliza el proveedor cuando se trabaja con el almacén del usuario. Es importante que establezca explícitamente un valor para el `applicationName` configuración porque si el `applicationName` no se establece explícitamente, se asigna a la ruta de acceso de la raíz virtual de la aplicación web en tiempo de ejecución. Esto funciona bien siempre y cuando no cambie la ruta de acceso de la raíz virtual de la aplicación, pero si mueve la aplicación a otra ruta de acceso, el `applicationName` configuración cambiará demasiado. Cuando esto ocurre, el proveedor de pertenencia comenzará a funcionar con una partición de aplicación diferente que se usó anteriormente. Cuentas de usuario creadas antes del traslado residen en una partición de aplicación diferente y los usuarios ya no podrá iniciar sesión en el sitio. Para obtener una explicación más detallada sobre este asunto, consulte [siempre establece la `applicationName` propiedad al configurar ASP.NET 2.0 pertenencia y otros proveedores](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Resumen

En este momento tenemos una base de datos con los servicios de aplicación configurada (`SecurityTutorials.mdf`) y ha configurado la aplicación web para que use el marco de trabajo de la pertenencia a la `SecurityTutorialsSqlMembershipProvider` acaba de registrar el proveedor. Este proveedor registrado es de tipo `SqlMembershipProvider` y tiene su `connectionStringName` establecido en la cadena de conexión adecuada (`SecurityTutorialsConnectionString`) y su `applicationName` valor explícitamente establecido.

Ahora estamos listos usar el marco de pertenencia de nuestra aplicación. En el siguiente tutorial examinaremos cómo crear nuevas cuentas de usuario. Después de que se explorará autenticar a los usuarios, realizar la autorización basada en usuario y almacenar información adicional relacionada con el usuario en la base de datos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Establezca siempre la `applicationName` propiedad al configurar ASP.NET 2.0 pertenencia y otros proveedores](https://weblogs.asp.net/scottgu/443634)
- [Servicios de aplicaciones configurar ASP.NET 2.0 para usar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Descargar SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examen de ASP.NET 2.0 s pertenencia, funciones y perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [El `<add>` elemento a los proveedores de pertenencia](https://msdn.microsoft.com/library/whae3t94.aspx)
- [El `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [El `<providers>` , elemento de pertenencia](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Usar `<clear />` cuando se agrega proveedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Trabajar directamente con el `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Aprendizaje mediante vídeo sobre los temas incluidos en este Tutorial

- [Descripción de las pertenencias a ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurar SQL para trabajar con esquemas de pertenencia](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Cambiar la configuración de la pertenencia en el esquema de pertenencia predeterminado](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Alicja Maziarz. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Siguiente](creating-user-accounts-cs.md)
