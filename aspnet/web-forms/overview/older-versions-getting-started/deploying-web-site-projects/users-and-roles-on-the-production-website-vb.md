---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Los usuarios y Roles en el sitio Web de producción (VB) | Microsoft Docs
author: rick-anderson
description: La herramienta de administración de sitio Web de ASP.NET (WSAT) proporciona una interfaz de usuario basada en web para configurar las opciones de pertenencia y funciones y para crear, editar, un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 5b2a5ea84c5c16ad5bf4e3041d31ad29660d965f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380381"
---
<a name="users-and-roles-on-the-production-website-vb"></a>Los usuarios y Roles en el sitio Web de producción (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> La herramienta de administración de sitio Web de ASP.NET (WSAT) proporciona una interfaz de usuario basada en web para configurar las opciones de pertenencia y funciones y para crear, modificar y eliminar usuarios y roles. Lamentablemente, el WSAT solo funciona cuando visita desde localhost, lo que significa que no puede llegar la herramienta de administración del sitio Web de producción a través del explorador. La buena noticia es que hay soluciones alternativas que hacen posible administrar usuarios y roles en producción. Este tutorial trata estas soluciones alternativas y otros.


## <a name="introduction"></a>Introducción

ASP.NET 2.0 introdujo un número de *servicios de aplicación*, que son un conjunto de servicios de bloques de creación que se pueden agregar a la aplicación web. Agregamos los servicios de pertenencia y funciones para el sitio Web de reseñas de libros de nuevo el [ *configurar un sitio Web que usa servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md). El servicio de pertenencia facilita crear y administrar cuentas de usuario; el servicio de Roles ofrece una API para clasificar a los usuarios en grupos. El sitio de reseñas de libros tiene tres cuentas de usuario - Scott, Jisun y Alice - y una única función, administrador, con Scott y Jisun en el rol de administrador.

ASP. Servicios de aplicaciones de la red no están asociados a una implementación específica. En su lugar, indicar a los servicios de aplicación para usar un determinado *proveedor*, y dicho proveedor implementa el servicio mediante una tecnología determinada. Hemos configurado la aplicación web de reseñas de libros para usar el `SqlMembershipProvider` y `SqlRoleProvider` proveedores para los servicios de pertenencia y funciones. Estos dos proveedores de almacenan información de cuenta y el rol de usuario en una base de datos de SQL Server y son los proveedores más populares para aplicaciones web basadas en Internet hospedadas en una empresa de hospedaje web.

Administra un desafío común para los desarrolladores que usan los servicios de pertenencia y Roles de los usuarios y roles en el entorno de producción. ¿Cómo eliminar una cuenta de usuario desde el sitio Web de producción, agregar un nuevo rol o agregar un usuario existente a un rol existente? Este tutorial exploran técnicas diferentes para administrar usuarios y roles en el sitio Web de producción.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Con la herramienta de administración del sitio Web de ASP.NET

ASP.NET incluye un [Web Site Administration Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) que facilita la tarea para crear y administrar roles y cuentas de usuario y para especificar las reglas de autorización basada en roles y usuarios. Para usar el WSAT, haga clic en el icono de configuración de ASP.NET en el Explorador de soluciones o vaya al menú proyecto o sitio Web y elija la opción de configuración de ASP.NET. Cualquiera de los enfoques inicia un explorador web y apunta a la WSAT en una dirección, como: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

El WSAT se divide en tres secciones:

- **Seguridad** -administrar usuarios, roles y las reglas de autorización.
- **ApplicationConfiguration** -administrar el &lt;appSettings&gt; y configuración de SMTP desde aquí. Se puede también desconectar la aplicación y administrar la depuración y seguimiento de la configuración a partir de aquí, así como especificar la página de error personalizada predeterminada.
- **ProviderConfiguration** -configurar los proveedores utilizados por los servicios de aplicación.

La sección de seguridad (que se muestra en **figura 1**) incluye vínculos para crear nuevos usuarios, administración de usuarios, crear y administrar roles y crear y administrar las reglas de acceso. Desde aquí puede agregar un nuevo rol en el sistema, eliminar un usuario existente, o agregar o quitar roles de una cuenta de usuario en particular.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Figura 1**: la sección de seguridad WSAT incluye opciones para administrar usuarios y Roles  
([Haga clic aquí para ver imagen en tamaño completo](users-and-roles-on-the-production-website-vb/_static/image3.png))

Lamentablemente, el WSAT solo está disponible localmente. No se puede visitar el WSAT en su sitio Web de producción remoto; Si visita `www.yoursite.com/asp.netwebadminfiles/default.aspx` obtener una respuesta 404 no encontrado. El código que activa el usa WSAT el `Membership` y `Roles` las clases de .NET Framework para crear, editar y eliminar usuarios y roles. Estas clases consultar información de configuración de la aplicación web para determinar qué proveedor para su uso; en el [ *configurar un sitio Web que usa servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md) configuramos el sitio Web de reseñas de libros para usar el `SqlMembershipProvider` y `SqlRoleProvider` proveedores. Esto incluye agregar `<membership>` y `<roleManager>` secciones a `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Tenga en cuenta que el `<membership>` y `<roleManager>` secciones referencia el `SqlMembershipProvider` y `SqlRoleProvider` proveedores en su `type` atributo, respectivamente. Estos proveedores de almacenan la información de rol y usuario en una base de datos de SQL Server especificada. La base de datos utilizada estos proveedores se especifica mediante el `connectionStringName` atributo, `ReviewsConnectionString`, que se define en el `~/ConfigSections/databaseConnectionStrings.config` archivo. Recuerde que el `databaseConnectionStrings.config` archivo en el entorno de desarrollo contiene la cadena de conexión a la base de datos de desarrollo, mientras que la `databaseConnectionStrings.config` archivo de producción contiene la cadena de conexión a la base de datos de producción.

En pocas palabras, debe tener acceso el WSAT localmente a través del entorno de desarrollo, y funciona con la información de usuario y el rol de la base de datos especificada en el `databaseConnectionStrings.config` archivo. Por lo tanto, si se cambia la información de la cadena de conexión en el `databaseConnectionStrings.config` archivo en el entorno de desarrollo podemos usar el WSAT localmente para administrar usuarios y roles en el entorno de producción.

Para ilustrar esta funcionalidad, abra el `databaseConnectionStrings.config` de archivos en Visual Studio en el entorno de desarrollo y reemplace la cadena de conexión de base de datos de desarrollo con la cadena de conexión de base de datos de producción. A continuación, inicie el WSAT, vaya a la ficha seguridad y agregar un nuevo usuario denominado a Sam con contraseña "password". (menos las comillas). **Figura 2** muestra la pantalla WSAT al crear esta cuenta.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Figura 2**: crear un nuevo usuario denominado Sam en el entorno de producción  
([Haga clic aquí para ver imagen en tamaño completo](users-and-roles-on-the-production-website-vb/_static/image6.png))

Dado que se puede cambiar la cadena de conexión en `databaseConnectionStrings.config` para que apunte al servidor de base de datos de producción, Sam se agregó como un usuario en el entorno de producción. Para comprobar esto, cambie la cadena de conexión en el `databaseConnectionStrings.config` de archivo a la base de datos de desarrollo y, a continuación, visite la `Login.aspx` página en el entorno de desarrollo. Intente iniciar sesión en como Sam (consulte **figura 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Figura 3**: no puede iniciar sesión como Sam en el entorno de desarrollo  
([Haga clic aquí para ver imagen en tamaño completo](users-and-roles-on-the-production-website-vb/_static/image9.png))

No se puede iniciar sesión como Sam en el entorno de desarrollo porque la información de cuenta de usuario no existe en la base de datos local. Está en su lugar, se ha agregado a la base de datos de producción. Para comprobarlo, ver el contenido de la `aspnet_Users` tabla en las bases de datos de desarrollo y producción. En el entorno de desarrollo debe ser solo tres registros para los usuarios Alice, Scott y Jisun. Sin embargo, el `aspnet_Users` tabla en la base de datos de producción tiene cuatro registros: Scott, Jisun, Alicia y Sam. Por lo tanto, Sam puede iniciar sesión en el sitio Web en producción, pero no mediante el entorno de desarrollo.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Figura 4**: Sam puede iniciar sesión en el sitio Web de producción  
([Haga clic aquí para ver imagen en tamaño completo](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> No olvide cambiar la cadena de conexión en el `databaseConnectionStrings.config` archivo a la base de datos de desarrollo de la cadena de conexión cuando haya terminado trabajando con el WSAT si no va a trabajar con datos de producción al probar el sitio mediante el desarrollo entorno. También tenga en cuenta que mientras que la técnica que acabamos de describir nos permite utilizar el WSAT para administrar usuarios y roles de forma remota, los cambios realizados en cualquiera de las otras opciones de configuración de WSAT (reglas de acceso, SMTP configuración, depuración y traza configuración y así sucesivamente) modifican el `Web.config` archivo. Por lo tanto, los cambios realizados en la configuración se aplican al entorno de desarrollo y no al entorno de producción.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Creación de usuario personalizado y las páginas Web de administración de roles

El WSAT proporciona un fuera del sistema cuadro para administrar usuarios y roles, pero solo se puede iniciar localmente y necesita realizar cambios a la información de la cadena de conexión con el fin de administrar los usuarios y roles en producción. La mayoría de sitios Web que admiten las cuentas de usuario también incluyen un número de páginas de web de administración de funciones que permiten a los administradores administrar usuarios y roles de páginas del sitio y de usuario. Estas páginas de administración basada en web facilitan en gran medida administrar usuarios y roles y son esenciales para los sitios donde puede haber muchos administradores o administradores que no tienen acceso a o la experiencia técnica para usar Visual Studio para iniciar el WSAT.

ASP.NET incluye una serie de controles relacionados con el inicio de sesión Web integrados que hacen que implementar muchas de estas páginas web administrativas tan fácil como arrastrar y colocar. Por ejemplo, puede crear una página para los administradores crear una nueva cuenta de usuario arrastrando el control CreateUserWizard hasta la página y establecer algunas propiedades. De hecho, la página para crear usuarios en el WSAT se muestra en **figura 2** utiliza el mismo control CreateUserWizard que se puede agregar a sus páginas. Además, las funcionalidades de los servicios pertenencia y funciones están disponibles mediante programación a través de la `Membership` y `Roles` las clases de .NET Framework. Con estas clases puede escribir código para crear, editar y eliminar usuarios y roles, así como para agregar o quitar usuarios a roles, para determinar qué usuarios están en los roles y realizar otras tareas relacionadas con el rol y el usuario.

En el [ *configurar un sitio Web que usa servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md) he agregado una página a la `Admin` carpeta denominada `CreateAccount.aspx`. Esta página permite a un administrador para agregar una nueva cuenta de usuario para el sitio y especificar si el usuario recién creado está en el rol de administrador (consulte **figura 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Figura 5**: los administradores pueden crear nuevas cuentas de usuario  
([Haga clic aquí para ver imagen en tamaño completo](users-and-roles-on-the-production-website-vb/_static/image15.png))

Para una visión más detallada de creación de páginas de administración de usuarios y roles, junto con instrucciones paso a paso sobre el uso de la `Membership` y `Roles` clases y los controles Web relacionados con el inicio de sesión de ASP.NET, no olvide leer mi [seguridad del sitio Web Tutoriales](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Allí encontrará instrucciones sobre cómo crear páginas web para crear nuevas cuentas, crear y administrar roles, asignar a usuarios a roles y otras tareas administrativas comunes.

Para implementar funcionalidad similar a WSAT en el sitio Web de producción siempre puede crear su propia serie de páginas web que implementan características de WSAT. Para ayudar a empezar a trabajar, consulte el código fuente WSAT, que se encuentra en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Otra opción consiste en usar WSAT otra alternativa de Dan Clem, que comparte en su artículo, [gradual su propio sitio Web de herramienta de administración](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan los lectores a través del proceso de creación de una herramienta similar a WSAT personalizada, incluye código de fuente de su aplicación para su descarga (en C#) y proporciona instrucciones paso a paso para agregar su WSAT personalizado a un sitio Web hospedado.

## <a name="summary"></a>Resumen

La herramienta de administración del sitio Web de ASP.NET (WSAT) puede utilizarse junto con los servicios de aplicación de pertenencia y funciones para administrar la información de usuario y el rol para el sitio Web. Lamentablemente, el WSAT localmente solo es accesible y no puede consultarse desde su sitio Web de producción. Sin embargo, cambiando la cadena de conexión en el desarrollo de entorno para que apunte a la base de datos de producción puede utilizar el WSAT para administrar los usuarios y roles en el sitio Web de producción.

Aunque el enfoque WSAT ofrece una forma rápida y sencilla de administrar usuarios y roles, será necesario iniciar el WSAT desde Visual Studio, así como realizar cambios temporales a la información de la cadena de conexión. El WSAT ofrece una forma rápida para administrar usuarios y roles en producción, pero es complicado y no funciona bien para sitios Web con varios administradores o administradores que no tiene o no están familiarizado con Visual Studio y el WSAT. Por estos motivos, la mayoría de sitios Web que admiten las cuentas de usuario incluyen un conjunto de páginas web administrativas. Este tipo de un conjunto de páginas web elimina la necesidad de la WSAT y utilizado por varios usuarios administrativos desde cualquier equipo.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Examen de ASP. Perfil, Roles y pertenencia a la red](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implementar su propia herramienta de administración de sitios Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Información general de herramienta de administración de sitio Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](precompiling-your-website-vb.md)
