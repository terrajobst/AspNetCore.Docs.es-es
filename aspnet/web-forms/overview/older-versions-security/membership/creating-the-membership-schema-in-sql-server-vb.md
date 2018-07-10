---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Crear el esquema de pertenencia en SQL Server (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se inicia mediante el examen de las técnicas para agregar el esquema necesario para la base de datos para poder usar SqlMembershipProvider. A continuación, nos wi...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fa1e3f57c1218af6fec8bddeed6cdcbdffe1965
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831850"
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>Crear el esquema de pertenencia en SQL Server (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) o [descargar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> En este tutorial se inicia mediante el examen de las técnicas para agregar el esquema necesario para la base de datos para poder usar SqlMembershipProvider. A continuación, examinaremos las tablas de clave en el esquema y describa su propósito y la importancia. Este tutorial finaliza con un vistazo a cómo saber qué proveedor debe usar el marco de pertenencia de una aplicación ASP.NET.


## <a name="introduction"></a>Introducción

Los dos tutoriales anteriores examinados utilizando la autenticación de formularios para identificar a los visitantes del sitio Web. El marco de trabajo de autenticación de formularios facilita a los desarrolladores a un usuario inicie sesión en un sitio Web y recordarlos todas las visitas de la página mediante el uso de vales de autenticación. La `FormsAuthentication` clase incluye métodos para generar el vale y éste se agrega a las cookies del visitante. El `FormsAuthenticationModule` examina todas las solicitudes entrantes y, para aquellos con un vale de autenticación válido, se crea y asocia un `GenericPrincipal` y un `FormsIdentity` objeto con la solicitud actual. Autenticación mediante formularios es simplemente un mecanismo para conceder un vale de autenticación para un visitante al iniciar sesión en y, en las solicitudes subsiguientes, analizar ese vale para determinar la identidad del usuario. Para que una aplicación web admitir cuentas de usuario, todavía es necesario implementar un almacén de usuario y agregan funcionalidad al validar las credenciales, registre los nuevos usuarios y la gran variedad de otras tareas relacionadas con la cuenta de usuario.

Antes de ASP.NET 2.0, los desarrolladores se encontraban en el enlace para la implementación de todas estas tareas relacionadas con la cuenta de usuario. Afortunadamente, el equipo de ASP.NET reconoce este inconveniente e introdujo el marco de pertenencia con ASP.NET 2.0. El marco de pertenencia es un conjunto de clases de .NET Framework que proporcionan una interfaz de programación para llevar a cabo tareas relacionadas con la cuenta de usuario principales. Este marco de trabajo se basa en el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), lo que permite a los desarrolladores conectar una implementación personalizada a una API estándar.

Como se describe en el <a id="Tutorial1"> </a> [ *conceptos básicos de seguridad y compatibilidad de ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) , tutorial de .NET Framework se distribuye con dos proveedores de pertenencia integrados: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) y [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Como su nombre implica, el `SqlMembershipProvider` usa una base de datos de Microsoft SQL Server como almacén de usuario. Para poder usar este proveedor en una aplicación, es necesario indicar al proveedor qué base de datos que se usará como el almacén. Como puede imaginar, la `SqlMembershipProvider` espera que la base de datos de almacén de usuario para tener determinadas tablas de base de datos, vistas y procedimientos almacenados. Es necesario agregar este esquema esperado para la base de datos seleccionada.

En este tutorial se inicia mediante el examen de las técnicas para agregar el esquema necesario para la base de datos para poder usar el `SqlMembershipProvider`. A continuación, examinaremos las tablas de clave en el esquema y describa su propósito y la importancia. Este tutorial finaliza con un vistazo a cómo saber qué proveedor debe usar el marco de pertenencia de una aplicación ASP.NET.

Comencemos.

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Paso 1: Decidir dónde colocar el Store del usuario

Datos de una aplicación ASP.NET normalmente se almacenan en un número de tablas en una base de datos. Al implementar el `SqlMembershipProvider` debemos decidir si desea colocar el esquema de pertenencia en la misma base de datos como los datos de aplicación o en una base de datos alternativa de esquema de base de datos.

Recomienda ubicar el esquema de pertenencia en la misma base de datos como los datos de aplicación por las razones siguientes:

- **Mantenimiento** una aplicación cuyos datos se encapsulan en una base de datos es más fácil de entender, mantener e implementar a una aplicación que tiene dos bases de datos independientes.
- **Integridad relacional** localizando las tablas relacionadas con la pertenencia a la misma base de datos como tablas de la aplicación, es posible establecer [restricciones foreign key](http://en.wikipedia.org/wiki/Foreign_key) entre las claves principales en el Las tablas relacionadas con la pertenencia y las tablas de aplicación relacionados.

Separar los datos de almacén y aplicación de usuario en las bases de datos independientes solo tiene sentido si tiene varias aplicaciones que cada usa bases de datos independientes, pero debe compartir un almacén de usuario común.

### <a name="creating-a-database"></a>Creación de una base de datos

La aplicación que compilamos desde el segundo tutorial aún no necesita una base de datos. Se necesita uno ahora, sin embargo, para el almacén del usuario. Vamos a crear una y, a continuación, agregarle el esquema requerido por la `SqlMembershipProvider` proveedor (vea el paso 2).

> [!NOTE]
> A lo largo de esta serie de tutoriales que se usarán un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) almacenar nuestras tablas de la aplicación de base de datos y el `SqlMembershipProvider` esquema. Esta decisión se realizó por dos motivos: en primer lugar, debido a su costo - gratis - Express Edition es la versión de SQL Server 2005; readably más accesible en segundo lugar, las bases de datos de SQL Server 2005 Express Edition pueden colocarse directamente en la aplicación web `App_Data` carpeta, lo que rápida y fácilmente para empaquetar la base de datos y aplicación web juntos en un archivo ZIP y volver a implementarlo sin las instrucciones de instalación especiales o las opciones de configuración. Si prefiere seguir el tutorial usa una versión de edición no Express de SQL Server, no dude. Los pasos son prácticamente idénticos. El `SqlMembershipProvider` esquema funcionarán con cualquier versión de Microsoft SQL Server 2000 y de seguridad.

En el Explorador de soluciones, haga doble clic en el `App_Data` carpeta y optar por agregar nuevo elemento. (Si no ve una `App_Data` carpeta del proyecto, haga doble clic en el proyecto en el Explorador de soluciones, seleccione Agregar carpeta ASP.NET y elegir `App_Data`.) En el cuadro de diálogo Agregar nuevo elemento, optar por agregar una nueva base de datos de SQL denominada `SecurityTutorials.mdf`. En este tutorial, agregaremos el `SqlMembershipProvider` esquema para esta base de datos; en los tutoriales posteriores se va a crear más tablas para capturar los datos de aplicación.


[![Agregar una nueva base de datos SQL denominado SecurityTutorials.mdf base de datos en la carpeta App_Data](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Figura 1**: agregar una nueva base de datos de SQL denominada `SecurityTutorials.mdf` a la base de datos la `App_Data` carpeta ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Adición de una base de datos a la `App_Data` carpeta incluye automáticamente en la vista del explorador de base de datos. (En la versión de edición no Express de Visual Studio, el Explorador de base de datos se denomina el Explorador de servidores). Vaya al explorador de base de datos y expanda el recién agregados `SecurityTutorials` base de datos. Si no ve el Explorador de base de datos en pantalla, vaya al menú Ver y elija Explorador de base de datos o presione Ctrl + Alt + S. Como se muestra en la figura 2, el `SecurityTutorials` base de datos está vacía - contiene ninguna tabla, no hay ninguna vista y no hay procedimientos almacenados.


[![La base de datos SecurityTutorials está actualmente vacía](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Figura 2**: el `SecurityTutorials` base de datos está actualmente vacía ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Paso 2: Agregar el`SqlMembershipProvider`esquema a la base de datos

El `SqlMembershipProvider` requiere un determinado conjunto de tablas, vistas y procedimientos almacenados que se instalen en la base de datos de almacén de usuario. Estos objetos de base de datos necesarios se pueden agregar utilizando el [ `aspnet_regsql.exe` herramienta](https://msdn.microsoft.com/library/ms229862.aspx). Este archivo se encuentra en la `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` carpeta.

> [!NOTE]
> El `aspnet_regsql.exe` herramienta ofrece la funcionalidad de línea de comandos y una interfaz gráfica de usuario. La interfaz gráfica es más descriptivo y es lo que se examinará en este tutorial. La interfaz de línea de comandos es útil cuando la adición de la `SqlMembershipProvider` esquema debe ser automatizada, como se muestra en la compilación de scripts o automatizar escenarios de prueba.

El `aspnet_regsql.exe` herramienta se utiliza para agregar o quitar *servicios de aplicaciones ASP.NET* a una base de datos de SQL Server especificada. Los servicios de aplicación de ASP.NET abarcan los esquemas para el `SqlMembershipProvider` y `SqlRoleProvider`, junto con los esquemas para los proveedores basados en SQL para otros marcos de ASP.NET 2.0. Se debe proporcionar dos bits de información para el `aspnet_regsql.exe` herramienta:

- Si desea agregar o quitar servicios de aplicación, y
- La base de datos que se va a agregar o quitar el esquema de servicios de aplicación

En que se pida confirmación de la base de datos, el `aspnet_regsql.exe` herramienta nos pide que proporcione el nombre del servidor en que reside la base de datos, las credenciales de seguridad para conectarse a la base de datos y el nombre de la base de datos. Si usas la no - Express Edition de SQL Server, ya debe saber esta información, ya que es la misma información que debe proporcionar a través de una cadena de conexión cuando se trabaja con la base de datos a través de una página web ASP.NET. Determinar el nombre del servidor y base de datos cuando se usa una base de datos de SQL Server 2005 Express Edition en el `App_Data` carpeta, sin embargo, es un poco más complicada.

La siguiente sección examina una manera sencilla para especificar el nombre de servidor y base de datos para una base de datos de SQL Server 2005 Express Edition en el `App_Data` carpeta. Si no está utilizando SQL Server 2005 Express Edition si quiere, puede ir directamente a la instalación de la sección de servicios de aplicación.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Determinar el servidor y el nombre de la base de datos para una base de datos SQL Server 2005 Express Edition en el`App_Data`carpeta

Para poder usar el `aspnet_regsql.exe` herramienta necesitamos saber los nombres de servidor y base de datos. El nombre del servidor es `localhost\InstanceName`. Probablemente, el *nombreDeInstancia* es `SQLExpress`. Sin embargo, si ha instalado manualmente SQL Server 2005 Express Edition (es decir, no instalarla automáticamente durante la instalación de Visual Studio), es posible que ha seleccionado un nombre de instancia diferente.

El nombre de la base de datos es un poco más complicado determinar. Las bases de datos en el `App_Data` carpeta suelen tener un nombre de base de datos que incluye un [identificador único global](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) junto con la ruta de acceso al archivo de base de datos. Es necesario determinar este nombre de base de datos con el fin de agregar el esquema de servicios de aplicación a través de `aspnet_regsql.exe`.

Es la manera más fácil de determinar el nombre de la base de datos examinarlo mediante SQL Server Management Studio. SQL Server Management Studio proporciona una interfaz gráfica para administrar las bases de datos de SQL Server 2005, pero no se distribuye con Express Edition de SQL Server 2005. La buena noticia es que [puede descargar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) el libre Express Edition de SQL Server Management Studio.

> [!NOTE]
> Si también tiene una versión de edición no Express de SQL Server 2005 instalado en el escritorio, es probable que se instala la versión completa de Management Studio. Puede usar la versión completa para determinar el nombre de la base de datos, siguiendo los mismos pasos, tal como se describe a continuación para la edición Express.

Empiece por cerrar Visual Studio para asegurarse de que se cierran todos los bloqueos impuestos por Visual Studio en el archivo de base de datos. A continuación, inicie SQL Server Management Studio y conéctese a la `localhost\InstanceName` base de datos de SQL Server 2005 Express Edition. Como se indicó anteriormente, lo más probable es el nombre de instancia `SQLExpress`. Para la opción de autenticación, seleccione autenticación de Windows.


[![Conectarse a la instancia SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Figura 3**: conectarse a la instancia de SQL Server 2005 Express Edition ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


Después de conectarse a la instancia de SQL Server 2005 Express Edition, Management Studio muestra las carpetas para las bases de datos, la configuración de seguridad, los objetos de servidor y así sucesivamente. Si expande la pestaña bases de datos verá que la `SecurityTutorials.mdf` base de datos es *no* registrada en la instancia de base de datos - se necesita adjuntar la base de datos en primer lugar.

Haga doble clic en la carpeta de bases de datos y elija adjuntar en el menú contextual. Se mostrará el cuadro de diálogo Adjuntar bases de datos. Desde aquí, haga clic en el botón Agregar, vaya a la `SecurityTutorials.mdf` de base de datos y haga clic en Aceptar. Figura 4 se muestra el cuadro de diálogo Adjuntar bases de datos después de la `SecurityTutorials.mdf` se ha seleccionado la base de datos. Figura 5 muestra el Explorador de objetos de Management Studio después de la base de datos se haya adjuntado correctamente.


[![Adjunte la base de datos SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Figura 4**: adjuntar el `SecurityTutorials.mdf` base de datos ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![La base de datos SecurityTutorials.mdf aparece en la carpeta de bases de datos](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Figura 5**: el `SecurityTutorials.mdf` base de datos aparece en la carpeta de bases de datos ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Como se muestra en la figura 5, el `SecurityTutorials.mdf` base de datos tiene un nombre ocultas en su lugar. Vamos a cambiarla a una más fácil de recordar (y más sencillo escribir) nombre. Haga doble clic en la base de datos, elija el cambio de nombre en el menú contextual y cámbiele el nombre `SecurityTutorialsDatabase`. Esto no cambia el nombre de archivo, solo el nombre de la base de datos que se usa para identificarse en SQL Server.


[![Cambie el nombre de la base de datos a SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Figura 6**: cambiar el nombre de la base de datos `SecurityTutorialsDatabase`([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


En este momento se sabe los nombres de servidor y base de datos para el `SecurityTutorials.mdf` el archivo de base de datos: `localhost\InstanceName` y `SecurityTutorialsDatabase`, respectivamente. Ahora estamos preparados para instalar los servicios de aplicación a través de la `aspnet_regsql.exe` herramienta.

### <a name="installing-the-application-services"></a>Instalación de los servicios de aplicación

Para iniciar el `aspnet_regsql.exe` herramienta, vaya al menú Inicio y elija ejecutar. Escriba `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` en el cuadro de texto y haga clic en Aceptar. Como alternativa, puede usar el Explorador de Windows para profundizar en la carpeta correspondiente y haga doble clic en el `aspnet_regsql.exe` archivo. Cualquiera de los enfoques cumplirá los mismos resultados.

Ejecuta el `aspnet_regsql.exe` herramienta sin ningún argumento de línea de comandos inicia la interfaz gráfica de usuario de Asistente para la instalación de ASP.NET SQL Server. El asistente facilita agregar o quitar los servicios de aplicación de ASP.NET en una base de datos especificado. La primera pantalla del asistente, que se muestra en la figura 7, describe el propósito de la herramienta.


[![Usar hace que el Asistente de programa de instalación de ASP.NET SQL Server para agregar el esquema de pertenencia](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Figura 7**: Use el ASP.NET SQL Server el programa de instalación tomadas para agregar el esquema de pertenencia ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


El segundo paso del asistente nos pregunta si desea quitar o agregar los servicios de aplicación. Puesto que deseamos agregar tablas, vistas y procedimientos almacenados necesarios para el `SqlMembershipProvider`, elija Configurar SQL Server para la opción de servicios de aplicación. Más adelante, si desea quitar este esquema de la base de datos, vuelva a ejecutar a este asistente, pero en su lugar, elija la información de servicios de aplicación de quitar de una opción de base de datos existente.


[![Elija la configuración de SQL Server para la opción de servicios de aplicación](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Figura 8**: elija la configuración de SQL Server para la opción de servicios de aplicación ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


El tercer paso solicita la información de la base de datos: el nombre del servidor, la información de autenticación y el nombre de base de datos. Si ha ido siguiendo este tutorial y ha agregado el `SecurityTutorials.mdf` a la base de datos `App_Data`, adjuntó a `localhost\InstanceName`y el nombre para `SecurityTutorialsDatabase`, a continuación, use los siguientes valores:

- Servidor: `localhost\InstanceName`
- Autenticación de Windows
- Base de datos: `SecurityTutorialsDatabase`


[![Escriba la información de la base de datos](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Figura 9**: escriba la información de la base de datos ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Después de escribir la información de la base de datos, haga clic en siguiente. El último paso, resume los pasos que se van a realizar. Haga clic en siguiente para instalar los servicios de aplicación y después en Finalizar para completar al asistente.

> [!NOTE]
> Si utiliza Management Studio para adjuntar la base de datos y el nombre del archivo de base de datos, no olvide de separar la base de datos y cerrar de Management Studio antes de volver a abrir Visual Studio. Para desasociar el `SecurityTutorialsDatabase` de base de datos, haga doble clic en el nombre de la base de datos y, en el menú de tareas, elija Desasociar.

Tras la finalización del asistente, vuelva a Visual Studio y navegue hasta el Explorador de base de datos. Expanda la carpeta tablas. Debería ver una serie de tablas cuyos nombres empiezan por el prefijo `aspnet_`. Del mismo modo, se puede encontrar una variedad de vistas y procedimientos almacenados en las carpetas de las vistas y procedimientos almacenados. Estos objetos de base de datos constituyen el esquema de servicios de aplicación. Examinaremos los objetos de base de datos de pertenencia y rol específicos en el paso 3.


[![Una variedad de tablas, vistas y procedimientos almacenados se han agregado a la base de datos](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Figura 10**: una gran variedad de tablas, vistas y almacena los procedimientos se han agregado a la base de datos ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> El `aspnet_regsql.exe` interfaz gráfica de usuario de la herramienta instala el esquema de servicios de toda la aplicación. Pero al ejecutar `aspnet_regsql.exe` desde la línea de comandos puede especificar qué aplicación concreta de servicios de componentes para instalar (o quitar). Por lo tanto, si desea agregar sólo las tablas, vistas y se almacenan los procedimientos necesarios para la `SqlMembershipProvider` y `SqlRoleProvider` proveedores, ejecutar `aspnet_regsql.exe` desde la línea de comandos. Como alternativa, puede ejecutar manualmente el subconjunto de T-SQL para crear scripts que usan los `aspnet_regsql.exe`. Estos scripts se encuentran en el `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` carpetas con nombres como `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`, y así sucesivamente.

En este punto hemos creado los objetos de base de datos necesarios para la `SqlMembershipProvider`. Sin embargo, todavía debemos indicar que el marco de pertenencia que debe usar el `SqlMembershipProvider` (frente a, por ejemplo, el `ActiveDirectoryMembershipProvider`) y que la `SqlMembershipProvider` debe usar el `SecurityTutorials` base de datos. Veremos cómo especificar qué proveedor a utilizar y cómo personalizar la configuración del proveedor seleccionado en el paso 4. Pero primero, echemos un vistazo más profundo de los objetos de base de datos que acaba de crear.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Paso 3: Un vistazo a las tablas de núcleo del esquema

Cuando se trabaja con los marcos de pertenencia y funciones en una aplicación ASP.NET, los detalles de implementación están encapsulados por el proveedor. En tutoriales futuros se va a interactuar con estos marcos de trabajo a través de .NET Framework `Membership` y `Roles` clases. Al usar estas API de alto nivel no es necesario se refieren a nosotros mismos con los detalles de bajo nivel, como las consultas que se ejecutan o se modifican las tablas mediante el `SqlMembershipProvider` y `SqlRoleProvider`.

Por tanto, podríamos usar con confianza los marcos de pertenencia y funciones sin necesidad de explorar el esquema de base de datos creado en el paso 2. Sin embargo, al crear las tablas para almacenar datos de aplicación, podemos necesitamos crear entidades relacionadas con usuarios o roles. Ayuda a conocer el `SqlMembershipProvider` y `SqlRoleProvider` esquemas al establecer externa clave restricciones entre las tablas de datos de aplicación y las tablas creadas en el paso 2. Además, en determinadas circunstancias excepcionales podemos necesitar interactuar con el usuario y rol almacena directamente en el nivel de base de datos (en lugar de a través del `Membership` o `Roles` clases).

### <a name="partitioning-the-user-store-into-applications"></a>Dividir el Store del usuario en aplicaciones

Los marcos de pertenencia y funciones están diseñados para que un único almacén de usuario y el rol puede compartirse entre muchas aplicaciones diferentes. Una aplicación ASP.NET que usa los marcos de pertenencia o Roles debe especificar qué partición de aplicación que se utilizará. En resumen, varias aplicaciones web pueden usar los mismos almacenes de usuario y el rol. Figura 11 muestra los almacenes de usuario y el rol que se dividen en tres aplicaciones: HRSite, CustomerSite y SalesSite. Estas aplicaciones web de tres tienen sus propios usuarios únicos y roles, pero todas ellas físicamente almacenan su información de cuenta y el rol de usuario en las mismas tablas de base de datos.


[![Las cuentas de usuario pueden dividirse en varias aplicaciones](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Figura 11**: cuentas pueden ser particiones a través de varias aplicaciones de usuario ([haga clic aquí para ver imagen en tamaño completo](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


El `aspnet_Applications` tabla es lo que define estas particiones. Cada aplicación que utiliza la base de datos para almacenar información de la cuenta de usuario se representa mediante una fila en esta tabla. El `aspnet_Applications` tabla tiene cuatro columnas: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, y `Description`.`ApplicationId` es de tipo [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) y es la clave principal de la tabla; `ApplicationName` proporciona un nombre fácil de usar único para cada aplicación.

Las tablas relacionadas con roles y pertenencia se vinculan a la `ApplicationId` campo `aspnet_Applications`. Por ejemplo, el `aspnet_Users` tabla, que contiene un registro para cada cuenta de usuario, tiene un `ApplicationId` campo de clave externa; de lo mismo que para el `aspnet_Roles` tabla. El `ApplicationId` campo en estas tablas especifica la partición de aplicación de la cuenta de usuario o rol pertenece.

### <a name="storing-user-account-information"></a>Almacenar información de cuentas de usuario

Información de la cuenta de usuario está alojado en dos tablas: `aspnet_Users` y `aspnet_Membership`. El `aspnet_Users` tabla contiene campos que contienen la información de cuenta de usuario esenciales. Las tres columnas más pertinentes son:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` es la clave principal (y del tipo `uniqueidentifier`). `UserName` es de tipo `nvarchar(256)` y, junto con la contraseña, hace que las credenciales del usuario. (Una contraseña de usuario se almacena en el `aspnet_Membership` tabla.) `ApplicationId` vincula la cuenta de usuario a una aplicación determinada en `aspnet_Applications`. Hay un compuesto [ `UNIQUE` restricción](https://msdn.microsoft.com/library/ms191166.aspx) en el `UserName` y `ApplicationId` columnas. Esto garantiza que en una aplicación determinada, cada nombre de usuario es único, pero permite para el mismo `UserName` para usarse en aplicaciones diferentes.

El `aspnet_Membership` tabla incluye información de la cuenta de usuario adicionales, como la contraseña del usuario, dirección de correo electrónico, la última fecha de inicio de sesión y tiempo y así sucesivamente. Hay una correspondencia uno a uno entre los registros de la `aspnet_Users` y `aspnet_Membership` tablas. Esta relación se garantiza mediante la `UserId` campo `aspnet_Membership`, que actúa como clave principal de la tabla. Al igual que el `aspnet_Users` tabla, `aspnet_Membership` incluye un `ApplicationId` campo que enlaza esta información a una partición de aplicación en particular.

### <a name="securing-passwords"></a>Protección de contraseñas

Información de contraseña se almacena en el `aspnet_Membership` tabla. El `SqlMembershipProvider` permite que las contraseñas se almacenen en la base de datos mediante una de las tres técnicas siguientes:

- **Borrar** -la contraseña se almacena en la base de datos como texto sin formato. Desaconseja encarecidamente usar esta opción. Si la base de datos está en peligro: ya sea por un usuario malintencionado que encuentre una puerta trasera o un empleado descontento que tiene acceso de la base de datos, las credenciales de cada usuario solo existen para la toma.
- **Aplica un algoritmo hash** -las contraseñas están descodificadas mediante un algoritmo hash unidireccional y un valor salt generado aleatoriamente. Este valor de hash (junto con el valor de salt) se almacena en la base de datos.
- **Cifrado** -una versión cifrada de la contraseña se almacena en la base de datos.

La técnica de almacenamiento de contraseña utilizada depende del `SqlMembershipProvider` configuración especificada en `Web.config`. Veremos personalizar el `SqlMembershipProvider` configuración en el paso 4. El comportamiento predeterminado consiste en almacenar el hash de la contraseña.

Las columnas responsables de almacenar la contraseña son `Password`, `PasswordFormat`, y `PasswordSalt`. `PasswordFormat` es un campo de tipo `int` cuyo valor indica la técnica usada para almacenar la contraseña: 0 para borrar; 1 para Hashed; 2 para el cifrado. `PasswordSalt` se asigna una cadena generada aleatoriamente, independientemente de la técnica de almacenamiento de la contraseña usada; el valor de `PasswordSalt` solo se usa al calcular el hash de la contraseña. Por último, la `Password` columna contiene los datos de la contraseña real, ya sea la contraseña de texto sin formato, el hash de la contraseña o la contraseña cifrada.

Tabla 1 se muestran estas tres columnas aspecto que podría para las distintas técnicas de almacenamiento al almacenar la contraseña MiSecreto! .

| **Almacenamiento de información técnica&lt;\_o3a\_p /&gt;** | **Contraseña&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | ¡MiSecreto! | 0 | tTnkPlesqissc2y2SMEygA== |
| Aplica un algoritmo hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Cifrado | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabla 1**: valores de ejemplo para los campos relacionados con las contraseñas al almacenar la contraseña de MiSecreto!

> [!NOTE]
> El cifrado determinado o un algoritmo hash utilizado por el `SqlMembershipProvider` viene determinada por la configuración de la `<machineKey>` elemento. Hemos explicado en este elemento de configuración en el paso 3 de la <a id="Tutorial3"> </a> [ *configuración de autenticación de formularios y temas avanzados* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) tutorial.

### <a name="storing-roles-and-role-associations"></a>Almacenar los Roles y las asociaciones de rol

El marco de Roles permite a los desarrolladores definir un conjunto de roles y especificar qué usuarios pertenecen a qué roles. Esta información se captura en la base de datos a través de dos tablas: `aspnet_Roles` y `aspnet_UsersInRoles`. Cada registro de la `aspnet_Roles` tabla representa un rol para una aplicación concreta. De forma similar a la `aspnet_Users` tabla, el `aspnet_Roles` tabla tiene tres columnas pertinentes a nuestra explicación:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` es la clave principal (y del tipo `uniqueidentifier`). `RoleName` es de tipo `nvarchar(256)`. Y `ApplicationId` vincula la cuenta de usuario a una aplicación determinada en `aspnet_Applications`. Hay un compuesto `UNIQUE` restricción en el `RoleName` y `ApplicationId` columnas, lo que garantiza que, en una aplicación determinada cada nombre de rol es único.

El `aspnet_UsersInRoles` tabla actúa como una asignación entre usuarios y roles. Hay solo dos columnas: `UserId` y `RoleId` - y juntos constituyen una clave principal compuesta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Paso 4: Especificar el proveedor y personalizar su configuración

Todos los marcos que admiten el modelo de proveedor, como los marcos de pertenencia y funciones - carecen de los detalles de implementación a sí mismos y en su lugar delegación esa responsabilidad en una clase de proveedor. En el caso del marco de pertenencia, el `Membership` clase define la API para administrar cuentas de usuario, pero no interactúa directamente con cualquier almacén de usuario. En su lugar, el `Membership` métodos entregar de la clase la solicitud al proveedor configurado - vamos a usar el `SqlMembershipProvider`. Cuando se invoca uno de los métodos en el `Membership` (clase), ¿cómo el marco de pertenencia sabe para delegar la llamada a la `SqlMembershipProvider`?

El `Membership` clase tiene un [ `Providers` propiedad](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) que contiene una referencia a todas las clases de proveedor registrado disponibles para su uso por el marco de pertenencia. Cada proveedor registrado tiene un nombre de asociado y el tipo. El nombre ofrece una manera fácil de usar para hacer referencia a un proveedor determinado en el `Providers` colección, mientras que el tipo identifica la clase de proveedor. Además, cada proveedor registrado puede incluir opciones de configuración. Valores de configuración para el marco de pertenencia incluyen `PasswordFormat` y `requiresUniqueEmail`, entre muchas otras. Consulte la tabla 2 para obtener una lista completa de opciones de configuración utilizado por el `SqlMembershipProvider`.

El `Providers` contenido de la propiedad se especifica a través de los valores de configuración de la aplicación web. De forma predeterminada, todas las aplicaciones web tienen un proveedor denominado `AspNetSqlMembershipProvider` de tipo `SqlMembershipProvider`. Se registra este proveedor de pertenencia predeterminado en `machine.config` (ubicado en `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Como el marcado anterior se muestra cómo, el [ `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx) define los valores de configuración para el marco de pertenencia mientras el [ `<providers>` elemento secundario](https://msdn.microsoft.com/library/6d4936ht.aspx) especifica registrado proveedores. Los proveedores se pueden agregar o quitar mediante el [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) o [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementos; utilice la [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elemento para quitar todos los actualmente proveedores registrados. Como el marcado anterior se muestra cómo, `machine.config` agrega un proveedor denominado `AspNetSqlMembershipProvider` de tipo `SqlMembershipProvider`.

Además el `name` y `type` atributos, el `<add>` elemento contiene atributos que definen los valores para diferentes configuraciones de configuración. Tabla 2 se enumeran los contadores `SqlMembershipProvider`-configuración específica, junto con una descripción de cada uno.

> [!NOTE]
> Los valores predeterminados que se ha indicado en la tabla 2 que hacen referencia a los valores predeterminados definidos en el `SqlMembershipProvider` clase. Tenga en cuenta que no todos los valores de configuración `AspNetSqlMembershipProvider` corresponden a los valores predeterminados de la `SqlMembershipProvider` clase. Por ejemplo, si no se especifica en un proveedor de pertenencia, el `requiresUniqueEmail` establecer valores predeterminados en true. Sin embargo, el `AspNetSqlMembershipProvider` reemplaza este valor predeterminado especificando explícitamente un valor de `false`.

| **Establecer&lt;\_o3a\_p /&gt;** | **Descripción&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Recuerde que el marco de pertenencia permite que un almacén de usuario único se dividan entre varias aplicaciones. Este valor indica el nombre de la partición de aplicación utilizado por el proveedor de pertenencia. Si este valor no se especifica explícitamente, se establece en tiempo de ejecución, el valor de ruta de acceso de raíz virtual de la aplicación. |
| `commandTimeout` | Especifica el valor de tiempo de espera de comando SQL (en segundos). El valor predeterminado es 30. |
| `connectionStringName` | El nombre de la cadena de conexión en el `<connectionStrings>` elemento va a usar para conectarse a la base de datos de almacén de usuario. Este valor es obligatorio. |
| `description` | Proporciona una descripción fácil de usar el proveedor registrado. |
| `enablePasswordRetrieval` | Especifica si los usuarios pueden recuperar su contraseña olvidada. El valor predeterminado es `false`. |
| `enablePasswordReset` | Indica si los usuarios pueden restablecer su contraseña. Tiene como valor predeterminado `true`. |
| `maxInvalidPasswordAttempts` | El número máximo de intentos de inicio de sesión que puede producirse por un usuario determinado durante especificado `passwordAttemptWindow` antes de bloquea al usuario. El valor predeterminado es 5. |
| `minRequiredNonalphanumericCharacters` | El número mínimo de caracteres no alfanuméricos que deben aparecer en una contraseña de usuario. Este valor debe estar entre 0 y 128; el valor predeterminado es 1. |
| `minRequiredPasswordLength` | El número mínimo de caracteres necesarios en una contraseña. Este valor debe estar entre 0 y 128; el valor predeterminado es 7. |
| `name` | El nombre del proveedor registrado. Este valor es obligatorio. |
| `passwordAttemptWindow` | El número de minutos durante el cual no pudo se realiza un seguimiento de intentos de inicio de sesión. Si el usuario proporciona credenciales de inicio de sesión no válido `maxInvalidPasswordAttempts` especifican de veces dentro de esta ventana, que se bloquee. El valor predeterminado es 10. |
| `PasswordFormat` | El formato de almacenamiento de contraseña: `Clear`, `Hashed`, o `Encrypted`. De manera predeterminada, es `Hashed`. |
| `passwordStrengthRegularExpression` | Si se proporciona, esta expresión regular se usa para evaluar la intensidad de la contraseña del usuario seleccionado al crear una nueva cuenta o al cambiar su contraseña. El valor predeterminado es una cadena vacía. |
| `requiresQuestionAndAnswer` | Especifica si un usuario debe responder su pregunta de seguridad al recuperar o restablecer su contraseña. El valor predeterminado es `true`. |
| `requiresUniqueEmail` | Indica si todas las cuentas de usuario en una partición determinada aplicación deben tener una dirección de correo electrónico única. El valor predeterminado es `true`. |
| `type` | Especifica el tipo del proveedor. Este valor es obligatorio. |

**Tabla 2**: pertenencia y `SqlMembershipProvider` opciones de configuración

Además `AspNetSqlMembershipProvider`, otros proveedores de pertenencia se pueden registrar en la aplicación por aplicación mediante la adición de marcado similar a la `Web.config` archivo.

> [!NOTE]
> El marco de trabajo de Roles funciona en gran parte del mismo modo: hay un proveedor de roles registrados de forma predeterminada en `machine.config` y se pueden personalizar los proveedores registrados en una base de aplicación por aplicación en `Web.config`. Examinaremos el marco de Roles y su marcado de configuración en detalle en un futuro tutorial.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizar el`SqlMembershipProvider`configuración

El valor predeterminado `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) tiene su `connectionStringName` atributo establecido en `LocalSqlServer`. Al igual que el `AspNetSqlMembershipProvider` proveedor, el nombre de la cadena de conexión `LocalSqlServer` se define en `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Como puede ver, esta cadena de conexión define una base de datos ubicado en SQL 2005 Express Edition | DataDirectory|aspnetdb. mdf. La cadena | DataDirectory | se traduce en tiempo de ejecución para que apunte a la `~/App_Data/` directorio, por lo que la ruta de acceso de base de datos | DataDirectory|aspnetdb. mdf se traduce en `~/App_Data` / `aspnet.mdf`.

Si no ha especificado ninguna información de proveedor de pertenencia en nuestra aplicación `Web.config` archivo, la aplicación utiliza el proveedor de pertenencia predeterminado registrado, `AspNetSqlMembershipProvider`. Si el `~/App_Data/aspnet.mdf` base de datos no existe, el tiempo de ejecución ASP.NET creará automáticamente y agregue el esquema de servicios de aplicación. Sin embargo, no deseamos usar el `aspnet.mdf` base de datos; en su lugar, queremos usar el `SecurityTutorials.mdf` base de datos se creó en el paso 2. Esta modificación se puede lograr de dos maneras:

- <strong>Especifique un valor para el</strong><strong>`LocalSqlServer`</strong><strong>nombre de la cadena de conexión en</strong><strong>`Web.config`</strong><strong>.</strong> Sobrescribiendo el `LocalSqlServer` valor de nombre de la cadena de conexión en `Web.config`, podemos usar el proveedor de pertenencia predeterminado registrado (`AspNetSqlMembershipProvider`) y hacer que funcione correctamente con el `SecurityTutorials.mdf` base de datos. Este enfoque es adecuado si está satisfecho con las opciones de configuración especificadas por `AspNetSqlMembershipProvider`. Para obtener más información sobre esta técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [configurar servicios de aplicación de ASP.NET 2.0 para utilizar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Agregar un nuevo proveedor registrado del tipo</strong><strong>`SqlMembershipProvider`</strong><strong>y configurar su</strong><strong>`connectionStringName`</strong><strong>configuración para que apunte a la</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>base de datos.</strong> Este enfoque es útil en escenarios donde desea personalizar otras propiedades además de la cadena de conexión de base de datos de configuración. En mis propios proyectos siempre usar este enfoque debido a su flexibilidad y la legibilidad.

Antes de que podemos agregar un nuevo proveedor registrado que hace referencia a la `SecurityTutorials.mdf` base de datos, primero es necesario agregar un valor de cadena de conexión adecuado en el `<connectionStrings>` sección `Web.config`. El marcado siguiente agrega una nueva cadena de conexión denominada `SecurityTutorialsConnectionString` que hace referencia a SQL Server 2005 Express Edition `SecurityTutorials.mdf` en la base de datos la `App_Data` carpeta.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Si usa un archivo de base de datos alternativa, actualice la cadena de conexión según sea necesario. Para obtener más información sobre la formación de la cadena de conexión correcto, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).

A continuación, agregue el siguiente marcado de configuración de pertenencia a la `Web.config` archivo. Este marcado registra un proveedor nuevo denominado `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Además de registrar el `SecurityTutorialsSqlMembershipProvider` proveedor, el marcado anterior define el `SecurityTutorialsSqlMembershipProvider` como el proveedor predeterminado (a través de la `defaultProvider` atributo el `<membership>` elemento). Recuerde que el marco de pertenencia puede tener varios de los proveedores registrados. Puesto que `AspNetSqlMembershipProvider` está registrado como el primer proveedor en `machine.config`, actúa como el proveedor predeterminado a menos que se indique lo contrario.

Actualmente, nuestra aplicación tiene dos proveedores registrados: `AspNetSqlMembershipProvider` y `SecurityTutorialsSqlMembershipProvider`. Sin embargo, antes de registrar el `SecurityTutorialsSqlMembershipProvider` proveedor nos podríamos desactivaron todas previamente los proveedores registrados mediante la adición de un [ `<clear />` elemento](https://msdn.microsoft.com/library/t062y6yc.aspx) inmediatamente antes de nuestra `<add>` elemento. Esto podría borrar el `AspNetSqlMembershipProvider` en la lista de los proveedores registrados, lo que significa que el `SecurityTutorialsSqlMembershipProvider` sería el único proveedor de pertenencia registrado. Si se usa este enfoque, no sería necesario que marque el `SecurityTutorialsSqlMembershipProvider` como el proveedor predeterminado, ya que es el único proveedor de pertenencia registrado. Para obtener más información sobre el uso de `<clear />`, consulte [Using `<clear />` al agregar proveedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Tenga en cuenta que el `SecurityTutorialsSqlMembershipProvider`del `connectionStringName` establecer referencias a los recién agregados `SecurityTutorialsConnectionString` nombre de la cadena de conexión y que su `applicationName` configuración se ha establecido en un valor de SecurityTutorials. Además, el `requiresUniqueEmail` configuración se ha establecido en `true`. Todas las demás opciones de configuración son idénticos a los valores de `AspNetSqlMembershipProvider`. No dude en realizar ninguna modificación de la configuración en este caso, si lo desea. Por ejemplo, puede reforzar la seguridad de la contraseña al requerir dos caracteres no alfanuméricos en lugar de uno, o mediante el aumento de la longitud de contraseña a ocho caracteres en lugar de siete.

> [!NOTE]
> Recuerde que el marco de pertenencia permite que un almacén de usuario único se dividan entre varias aplicaciones. El proveedor de pertenencia `applicationName` valor indica qué aplicación el proveedor se utiliza cuando se trabaja con el almacén del usuario. Es importante que establezca explícitamente un valor para el `applicationName` configuración porque si el `applicationName` no se establece explícitamente, se asigna a la ruta de acceso de raíz virtual de la aplicación web en tiempo de ejecución. Esto funciona bien siempre y cuando no cambia la ruta de acceso de raíz virtual de la aplicación, pero si mueve la aplicación a otra ruta de acceso, el `applicationName` configuración cambiará demasiado. Cuando esto sucede, el proveedor de pertenencia comenzará a funcionar con una partición de aplicación diferente que se usó anteriormente. Las cuentas de usuario creadas antes del traslado residirá en una partición de aplicación diferente y esos usuarios ya no podrán iniciar sesión en el sitio. Para obtener una explicación más detallada sobre este asunto, consulte [siempre establece la `applicationName` propiedad al configurar ASP.NET 2.0 Membership y otros proveedores](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Resumen

En este momento tenemos una base de datos con los servicios de aplicación configurada (`SecurityTutorials.mdf`) y ha configurado nuestra aplicación web para que el marco de pertenencia se usa el `SecurityTutorialsSqlMembershipProvider` nos acaba de registrar el proveedor. Este proveedor registrado es de tipo `SqlMembershipProvider` y tiene su `connectionStringName` establecido en la cadena de conexión adecuada (`SecurityTutorialsConnectionString`) y su `applicationName` valor explícitamente establecido.

Ahora estamos listos usar el marco de pertenencia desde nuestra aplicación. En el siguiente tutorial, examinaremos cómo crear cuentas de usuario. Después de que se explorará la autenticación de usuarios, realizar la autorización basada en usuario y almacenar información adicional relacionada con el usuario en la base de datos.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Establezca siempre la `applicationName` propiedad al configurar ASP.NET 2.0 Membership y otros proveedores](https://weblogs.asp.net/scottgu/443634)
- [Configuración de ASP.NET 2.0 de servicios de aplicación a usar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Descargar SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examen de ASP.NET 2.0 s pertenencia, Roles y perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [El `<add>` elemento providers para Membership](https://msdn.microsoft.com/library/whae3t94.aspx)
- [El `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [El `<providers>` (elemento) para la suscripción](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Uso de `<clear />` al agregar proveedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Trabajar directamente con el `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>En los temas contenidos en este Tutorial en vídeo

- [Descripción de las pertenencias a ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurar SQL para trabajar con esquemas de pertenencia](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Cambiar la configuración de la pertenencia en el esquema de pertenencia predeterminado](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Puede ponerse en contacto Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Alicja Maziarz. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](storing-additional-user-information-cs.md)
> [Siguiente](creating-user-accounts-vb.md)
