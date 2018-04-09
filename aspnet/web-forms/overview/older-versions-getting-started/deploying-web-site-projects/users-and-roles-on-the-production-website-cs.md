---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Los usuarios y Roles en el sitio Web de producción (C#) | Documentos de Microsoft
author: rick-anderson
description: La herramienta de administración de sitios Web de ASP.NET (WSAT) proporciona una interfaz de usuario basada en web para configurar las opciones de pertenencia y los Roles y para crear, editar, un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: e3e1165959ae47715e0037db7a3bc6ac58807653
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="users-and-roles-on-the-production-website-c"></a>Los usuarios y Roles en el sitio Web de producción (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga de PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> La herramienta de administración de sitios Web de ASP.NET (WSAT) proporciona una interfaz de usuario basada en web para configurar las opciones de pertenencia y los Roles y para crear, modificar y eliminar usuarios y roles. Desafortunadamente, la WSAT solo funciona cuando se visitan de localhost, lo que significa que no se puede alcanzar la herramienta de administración del sitio Web de producción a través del explorador. Las buenas noticias son que hay soluciones alternativas que le permite administrar usuarios y roles en producción. Este tutorial examina estas soluciones alternativas y otros usuarios.


## <a name="introduction"></a>Introducción

ASP.NET 2.0 incorpora una serie de *servicios de aplicación*, que son un conjunto de servicios de bloques de creación que se pueden agregar a la aplicación web. Hemos agregado los servicios de pertenencia y los Roles para el sitio Web de reseñas de libros de nuevo en el [ *configurar un sitio Web que utiliza servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-cs.md). El servicio de pertenencia facilita crear y administrar cuentas de usuario; el servicio de Roles ofrece una API para clasificar a los usuarios en grupos. El sitio de reseñas de libros tiene tres cuentas de usuario - Scott, Jisun y Alice - y un solo rol, Admin, Scott y Jisun en el rol de administrador.

ASP. Los servicios de aplicaciones de red no están asociados a una implementación específica. En su lugar, se indica a los servicios de aplicación que se va a usar un determinado *proveedor*, y ese proveedor implementa el servicio mediante una tecnología concreta. Hemos configurado la aplicación web de reseñas de libros para utilizar el `SqlMembershipProvider` y `SqlRoleProvider` proveedores para los servicios de pertenencia y los Roles. Estos dos proveedores de almacenan información de cuenta y el rol de usuario en una base de datos de SQL Server y son los proveedores de usados más frecuente para aplicaciones web basadas en Internet hospedadas en una empresa de hospedaje web.

Un desafío común para los desarrolladores que utilizan los servicios de pertenencia y los Roles está administrando los usuarios y roles en el entorno de producción. ¿Cómo se elimina una cuenta de usuario desde el sitio Web de producción, agregar un nuevo rol o agregar un usuario existente a un rol existente? Este tutorial explora técnicas diferentes para administrar usuarios y roles en el sitio Web de producción.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Con la herramienta de administración del sitio Web de ASP.NET

ASP.NET incluye una [herramienta de administración de sitios Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) que facilita el proceso para crear y administrar roles y cuentas de usuario y para especificar las reglas de autorización basada en roles y usuarios. Para utilizar el WSAT, haga clic en el icono de configuración de ASP.NET en el Explorador de soluciones, o vaya al sitio Web o proyecto de menú y elija la opción de configuración de ASP.NET. Cualquier enfoque inicia un explorador web y apunta a la WSAT en una dirección como: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

El WSAT se divide en tres secciones:

- **Seguridad** -administrar usuarios, roles y las reglas de autorización.
- **ApplicationConfiguration** -administrar el &lt;appSettings&gt; y la configuración de SMTP desde aquí. Se puede también poner la aplicación sin conexión y administrar de depuración y traza configuración desde aquí, así como especificar la página de errores personalizada predeterminada.
- **ProviderConfiguration** -configurar los proveedores utilizados por los servicios de aplicación.

La sección de seguridad (se muestra en **figura 1**) incluye vínculos para crear nuevos usuarios, administrar usuarios, crear y administrar los roles y la creación y administración de las reglas de acceso. Desde aquí puede agregar un nuevo rol en el sistema, eliminar un usuario existente, o agregar o quitar roles de una cuenta de usuario determinada.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Figura 1**: la sección de seguridad WSAT incluye opciones para administrar usuarios y Roles  
([Haga clic aquí para ver la imagen a tamaño completo](users-and-roles-on-the-production-website-cs/_static/image3.png))

Desafortunadamente, la WSAT solo está disponible localmente. No se puede visitar el WSAT en el sitio Web de producción remoto; Si visita `www.yoursite.com/asp.netwebadminfiles/default.aspx` obtener una respuesta 404 no encontrado. El código que alimenta el utiliza WSAT el `Membership` y `Roles` las clases de .NET Framework para crear, editar y eliminar usuarios y roles. Estas clases Consulte información de configuración de la aplicación web para determinar qué proveedor a utilizar; en el [ *configurar un sitio Web que utiliza servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-cs.md) configuramos el sitio Web de reseñas de libros para usar el `SqlMembershipProvider` y `SqlRoleProvider` proveedores. Esto implicaba tener que agregar `<membership>` y `<roleManager>` secciones a `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Tenga en cuenta que la `<membership>` y `<roleManager>` secciones de referencia de la `SqlMembershipProvider` y `SqlRoleProvider` proveedores en su `type` atributo, respectivamente. Estos proveedores de almacenarán el usuario y la información de las funciones en una base de datos de SQL Server especificada. La base de datos utilizado por estos proveedores se especifica mediante la `connectionStringName` atributo, `ReviewsConnectionString`, que se define en el `~/ConfigSections/databaseConnectionStrings.config` archivo. Recuerde que el `databaseConnectionStrings.config` archivo en el entorno de desarrollo contiene la cadena de conexión a la base de datos de desarrollo, mientras que la `databaseConnectionStrings.config` archivo en producción y contiene la cadena de conexión a la base de datos de producción.

En pocas palabras, la WSAT debe obtenerse acceso localmente a través del entorno de desarrollo, y funciona con la información de usuario y el rol de la base de datos especificada en el `databaseConnectionStrings.config` archivo. Por lo tanto, si se cambia la información de la cadena de conexión en el `databaseConnectionStrings.config` archivo en el entorno de desarrollo, podemos utilizar la WSAT localmente para administrar usuarios y roles en el entorno de producción.

Para ilustrar esta funcionalidad, abra el `databaseConnectionStrings.config` de archivo en Visual Studio en el entorno de desarrollo y reemplace la cadena de conexión de base de datos de desarrollo con la cadena de conexión de base de datos de producción. A continuación, inicie el WSAT, vaya a la pestaña seguridad y agregar un nuevo usuario con el nombre a Sam con la contraseña "password". (menos las comillas). **Figura 2** muestra la pantalla WSAT al crear esta cuenta.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Figura 2**: crear un nuevo usuario con el nombre Sam en el entorno de producción  
([Haga clic aquí para ver la imagen a tamaño completo](users-and-roles-on-the-production-website-cs/_static/image6.png))

Dado que se ha cambiado la cadena de conexión en `databaseConnectionStrings.config` para que señale al servidor de base de datos de producción, Sam se agregó como un usuario en el entorno de producción. Para comprobar esto, cambie la cadena de conexión en el `databaseConnectionStrings.config` de archivo a la base de datos de desarrollo y, a continuación, visite la `Login.aspx` página en el entorno de desarrollo. Intente iniciar sesión como Sam (consulte **figura 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Figura 3**: no se puede iniciar sesión en como Sam en el entorno de desarrollo  
([Haga clic aquí para ver la imagen a tamaño completo](users-and-roles-on-the-production-website-cs/_static/image9.png))

No se puede iniciar sesión como Sam en el entorno de desarrollo porque la información de la cuenta de usuario no existe en la base de datos local. Está en su lugar, se ha agregado a la base de datos de producción. Para comprobarlo, ver el contenido de la `aspnet_Users` tabla en las bases de datos de desarrollo y de producción. En el entorno de desarrollo debe ser solo tres registros para los usuarios Scott y Jisun, Alicia. Sin embargo, la `aspnet_Users` tabla en la base de datos de producción tiene cuatro registros: Scott, Jisun, Alicia y Sam. Por lo tanto, Sam puede iniciar sesión en el sitio Web en producción, pero no mediante el entorno de desarrollo.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Figura 4**: Sam puede iniciar sesión el sitio Web de producción  
([Haga clic aquí para ver la imagen a tamaño completo](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> No olvide cambiar la cadena de conexión en el `databaseConnectionStrings.config` archivo a la base de datos de desarrollo de la cadena de conexión cuando haya terminado trabajar con el WSAT en caso contrario, trabajará con datos de producción cuando se prueba el sitio a través del desarrollo entorno. Tenga en cuenta que mientras la técnica que acabamos de describir nos permite usar el WSAT para administrar de forma remota a los usuarios y roles, cambios en cualquiera de las otras opciones de configuración de WSAT (reglas de acceso, SMTP configuración, depuración y traza configuración y así sucesivamente) modifican el `Web.config` archivo. Por lo tanto, los cambios realizados en la configuración se aplican al entorno de desarrollo y no al entorno de producción.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Creación de usuario personalizado y páginas Web de administración de roles

El WSAT proporciona un fuera del sistema cuadro para administrar usuarios y roles, pero únicamente lo podrán iniciar localmente y necesita realizar cambios en la información de la cadena de conexión con el fin de administrar los usuarios y roles en producción. La mayoría de sitios Web que admiten las cuentas de usuario también incluye una serie de usuario y páginas de web de administración de funciones que permiten a los administradores administrar usuarios y roles de páginas en el sitio. Estas páginas de administración basada en web hacerla son mucho más fáciles de administrar usuarios y roles y son esenciales para los sitios donde puede haber muchos administradores o administradores que no tienen acceso a o la técnica básica se usa Visual Studio para iniciar el WSAT.

ASP.NET incluye una serie de controles relacionados con el inicio de sesión Web integrados que permiten implementar muchas de estas páginas web administrativas tan fácil como arrastrar y colocar. Por ejemplo, puede crear una página para que los administradores crear una nueva cuenta de usuario arrastrando el control CreateUserWizard en la página y establecer algunas propiedades. De hecho, la página para crear usuarios en el WSAT se muestra en **figura 2** utiliza el mismo control CreateUserWizard que se puede agregar a las páginas. Además, las funcionalidades de los servicios pertenencia y los Roles están disponibles mediante programación a través de la `Membership` y `Roles` las clases de .NET Framework. Con estas clases puede escribir código para crear, editar y eliminar usuarios y roles, así como para agregar o quitar usuarios a funciones, para determinar qué usuarios están en los roles y para realizar otras tareas relacionadas con el rol y el usuario.

En el [ *configurar un sitio Web que utiliza servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-cs.md) se ha agregado una página a la `Admin` carpeta denominada `CreateAccount.aspx`. Esta página permite a un administrador para agregar una nueva cuenta de usuario para el sitio y especificar si es o no el usuario recién creado en el rol de administrador (consulte **figura 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Figura 5**: los administradores pueden crear nuevas cuentas de usuario  
([Haga clic aquí para ver la imagen a tamaño completo](users-and-roles-on-the-production-website-cs/_static/image15.png))

Para obtener una visión más detallada en la creación de páginas de administración de usuario y el rol, así como instrucciones paso a paso sobre cómo utilizar el `Membership` y `Roles` las clases y los controles relacionados con el inicio de sesión Web de ASP.NET, no olvide leer mi [seguridad del sitio Web Tutoriales](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Encontrará instrucciones sobre cómo crear páginas web para crear nuevas cuentas, crear y administrar roles, asignar a usuarios a roles y otras tareas administrativas comunes.

Para implementar una funcionalidad similar WSAT en el sitio Web de producción siempre puede crear su propia serie de páginas web que implementan características del WSAT. Como ayuda para empezar a trabajar, extraer del repositorio el código de origen WSAT, que se encuentra en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Otra opción consiste en usar alternativa, WSAT de Dan Clem, que se comparte en su artículo, [poner su propio Web Site Administration Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan le guía a los lectores a través del proceso de creación de una herramienta similar a WSAT personalizada, incluye código de fuente de su aplicación para su descarga (en C#) y proporciona instrucciones paso a paso para agregar su WSAT personalizado a un sitio Web hospedado.

## <a name="summary"></a>Resumen

La herramienta de administración del sitio Web de ASP.NET (WSAT) puede utilizarse junto con los servicios de aplicación de pertenencia y los Roles para administrar la información de usuario y el rol para el sitio Web. Desafortunadamente, la WSAT solo está disponible localmente y no se visitan su sitio Web de producción. Sin embargo, si cambia la cadena de conexión en el desarrollo entorno para que apunte a la base de datos de producción puede usar el WSAT para administrar los usuarios y roles en el sitio Web de producción.

Aunque el enfoque WSAT ofrece una forma rápida y sencilla para administrar usuarios y roles, necesita iniciar el WSAT desde Visual Studio, así como realizar cambios temporales en la información de la cadena de conexión. El WSAT ofrece una forma rápida de administrar usuarios y roles en producción, pero es complicado y no funciona bien para sitios Web con varios administradores o con los administradores que no tiene o no están familiarizado con Visual Studio y la WSAT. Por estos motivos, la mayoría de sitios Web que admiten las cuentas de usuario incluye un conjunto de páginas web administrativas. Este tipo de un conjunto de páginas web elimina la necesidad de la WSAT y utilizado por varios usuarios administrativos desde cualquier equipo.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Examen de ASP. Pertenencia, funciones y perfil de red](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Poner su propia herramienta de administración de sitios Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Información general de herramienta de administración de sitios Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Tutoriales de seguridad de sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](precompiling-your-website-cs.md)
> [Siguiente](asp-net-hosting-options-vb.md)
