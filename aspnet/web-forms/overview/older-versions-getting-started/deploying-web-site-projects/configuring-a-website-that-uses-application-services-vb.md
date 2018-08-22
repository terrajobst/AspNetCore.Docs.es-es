---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Configurar un sitio Web que usa servicios de aplicaciones (VB) | Microsoft Docs
author: rick-anderson
description: Versión ASP.NET 2.0 incorporó una serie de servicios de aplicación, que forman parte de .NET Framework y servir como un conjunto de bloques de creación de servicios que yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: d5fe8dc8486cf08e0aaf0e107069972eee7fbada
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828458"
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Configurar un sitio Web que usa servicios de aplicaciones (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) o [descargar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> Versión ASP.NET 2.0 incorporó una serie de servicios de aplicación, que forman parte de .NET Framework y actuar como un conjunto de servicios de bloques de creación que puede usar para agregarle funciones enriquecidas para la aplicación web. Este tutorial explora cómo configurar un sitio Web en el entorno de producción para usar servicios de aplicaciones y trata los problemas comunes con la administración de cuentas de usuario y funciones en el entorno de producción.


## <a name="introduction"></a>Introducción

Versión ASP.NET 2.0 incorporó una serie de *servicios de aplicación*, que forman parte de .NET Framework y servir como un conjunto de bloques de creación de servicios que se puede usar para agregar funciones enriquecidas para la aplicación web. Los servicios de aplicación incluyen:

- **Pertenencia** : una API para crear y administrar cuentas de usuario.
- **Roles** : una API para clasificar a los usuarios en grupos.
- **Perfil** : una API para almacenar el contenido personalizado, específicos del usuario.
- **Mapa del sitio** : una API para definir una estructura lógica del sitio s en forma de una jerarquía, lo que, a continuación, se puede mostrar a través de los controles de navegación, como menús y las rutas de navegación.
- **Personalización** : una API para el mantenimiento de las preferencias de personalización, se utiliza frecuentemente con [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Seguimiento de estado** : una API para la supervisión de rendimiento, seguridad, errores y otras métricas de mantenimiento del sistema para una aplicación web en ejecución.
  

Las API de servicios de aplicación no están asociadas a una implementación específica. En su lugar, indicar a los servicios de aplicación para usar un determinado *proveedor*, y dicho proveedor implementa el servicio mediante una tecnología determinada. Los proveedores más populares para aplicaciones web basadas en Internet hospedadas en una empresa de hospedaje web son los proveedores que utilizan una implementación de la base de datos de SQL Server. Por ejemplo, el `SqlMembershipProvider` es un proveedor para la API de pertenencia que almacena información de la cuenta de usuario en una base de datos de Microsoft SQL Server.

Mediante los servicios de aplicación y los proveedores de SQL Server agrega algunas dificultades al implementar la aplicación. Para empezar, los objetos de base de datos de servicios de aplicación deben crearse correctamente en el desarrollo y producción de bases de datos e inicializó correctamente. También hay opciones de configuración importantes que deben realizarse.

> [!NOTE]
> Los servicios de aplicaciones API se han diseñado mediante el [ *modelo de proveedor*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), un patrón de diseño que permite a los detalles de implementación debe proporcionarse en tiempo de ejecución de una API s. .NET Framework se distribuye con un número de proveedores de servicios de aplicación que se pueden usar, como la `SqlMembershipProvider` y `SqlRoleProvider`, que son proveedores de pertenencia y de implementación de base de datos de las API de Roles que usan un servidor SQL Server. También puede crear y complemento un proveedor personalizado. De hecho, la aplicación de web reseñas de libros ya contiene un proveedor personalizado para la API de mapa del sitio (`ReviewSiteMapProvider`), que construye el mapa del sitio de los datos en el `Genres` y `Books` tablas en la base de datos.


Este tutorial comienza con un vistazo a cómo ampliar la aplicación web de reseñas de libros para usar las API de Roles y pertenencia. A continuación, le guía a través de la implementación de una aplicación web que usa servicios de aplicaciones con una implementación de la base de datos de SQL Server y concluye con la solución de problemas comunes con la administración de cuentas de usuario y funciones en el entorno de producción.

## <a name="updates-to-the-book-reviews-application"></a>Actualizaciones de la aplicación de revisiones de libro

En los últimos tutoriales de que la aplicación web revisa el libro se ha actualizado de un sitio Web estático a una aplicación web dinámica controlada por datos se complete con un conjunto de páginas de administración para la administración de revisiones y géneros. Sin embargo, esta sección de administración no está protegida actualmente, cualquier usuario que sepa (o averiguará) la dirección URL de página de administración puede moverse alegremente en y crear, editar o eliminar las revisiones en nuestro sitio. Una forma habitual de proteger determinadas partes de un sitio Web es implementar cuentas de usuario y, a continuación, utilizar las reglas de autorización de dirección URL para restringir el acceso a determinados usuarios o roles. La aplicación web de reseñas de libros disponible para su descarga con este tutorial es compatible con cuentas de usuario y funciones. Tiene un solo rol definido denominado administrador y solo los usuarios de este rol pueden tener acceso a las páginas de administración.

> [!NOTE]
> Se ve crea tres cuentas de usuario en la aplicación web de reseñas de libros: Alice, Scott y Jisun. Tres todos los usuarios tienen la misma contraseña: **contraseña!** Scott y Jisun están en el rol de administrador, no es de Alice. Las páginas de administración que no son s sitio siguen siendo accesibles a los usuarios anónimos. Es decir, no es necesario iniciar sesión en el sitio, a menos que desee administrar, en cuyo caso debe iniciar sesión como un usuario en el rol de administrador.


La página principal de reseñas de libros aplicación s se ha actualizado para incluir una interfaz de usuario diferente para los usuarios autenticados y anónimos. Si un usuario anónimo visita el sitio ve un vínculo de inicio de sesión en la esquina superior derecha. Un usuario autenticado ve el mensaje, "Bienvenido de nuevo, *username*!" y un vínculo para cerrar sesión. Allí s también una página de inicio de sesión (`~/Login.aspx`), que contiene un control Web de inicio de sesión que proporciona la interfaz de usuario y la lógica para autenticar un visitante. Solo los administradores pueden crear nuevas cuentas. (Hay páginas para crear y administrar cuentas de usuario en el `~/Admin` carpeta.)

### <a name="configuring-the-membership-and-roles-apis"></a>Configuración de las API de Roles y pertenencia

La aplicación web de reseñas de libros usa las API de Roles y pertenencia para admitir cuentas de usuario y para agrupar esos usuarios en roles (es decir, el rol de administrador). El `SqlMembershipProvider` y `SqlRoleProvider` se utilizan las clases de proveedor porque queremos almacenar información de cuenta y el rol en una base de datos de SQL Server.

> [!NOTE]
> En este tutorial no pretende ser un examen detallado en la configuración de una aplicación web para admitir la pertenencia y funciones de API. Para una mirada exhaustiva a estas API y los pasos que debe seguir para configurar un sitio Web para poder utilizarlos, lea mi [ *tutoriales de seguridad del sitio Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Para usar los servicios de aplicación con una base de datos de SQL Server debe agregar primero los objetos de base de datos utilizados por estos proveedores para la base de datos donde desea que la cuenta de usuario y la información de rol almacenada. Estos objetos de base de datos necesarios incluyen una serie de tablas, vistas y procedimientos almacenados. A menos que se especifique lo contrario, el `SqlMembershipProvider` y `SqlRoleProvider` clases de proveedor utilizan una base de datos de SQL Server Express Edition denominada `ASPNETDB` ubicado en la aplicación s `App_Data` carpeta; si no existe una base de datos, se crea automáticamente con los objetos de base de datos necesarios por estos proveedores en tiempo de ejecución.

Es posible, y servicios de la aplicación normalmente ideal, para crear objetos de base de datos en la misma base de datos donde se almacenan los datos de sitio Web s específicos de la aplicación. .NET Framework incluye una herramienta denominada `aspnet_regsql.exe` que instala los objetos de base de datos en una base de datos especificado. He continué y usar esta herramienta para agregar estos objetos a la `Reviews.mdf` en la base de datos la `App_Data` carpeta (la base de datos de desarrollo). Veremos cómo usar esta herramienta más adelante en este tutorial, cuando se agregan estos objetos a la base de datos de producción.

Si agrega los objetos de base de datos a una base de datos distinto de la aplicación servicios `ASPNETDB` necesite personalizar la `SqlMembershipProvider` y `SqlRoleProvider` configuraciones de clases de proveedor para que usen la base de datos adecuado. Para personalizar agregar el proveedor de pertenencia un [  *&lt;pertenencia&gt; elemento* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) dentro de la `<system.web>` sección `Web.config`; utilice la [  *&lt;roleManager&gt; elemento* ](https://msdn.microsoft.com/library/ms164660.aspx) para configurar el proveedor de Roles. El fragmento de código siguiente se toma del s aplicación reseñas de libros `Web.config` y muestra las opciones de configuración para la pertenencia y funciones de API. Tenga en cuenta que ambos registren un nuevo proveedor - `ReviewMembership` y `ReviewRole` -que usan el `SqlMembershipProvider` y `SqlRoleProvider` proveedores, respectivamente.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

El `Web.config` archivo s `<authentication>` elemento también se ha configurado para admitir la autenticación basada en formularios.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitar el acceso a las páginas de administración

ASP.NET facilita la tarea conceder o denegar el acceso a un archivo o carpeta determinado usuario o rol a través de su *autorización de URL* característica. (Hemos explicado brevemente la autorización de URL en el *diferencias principales entre IIS y el servidor de desarrollo de ASP.NET* tutorial y se ha explicado cómo IIS y el servidor de desarrollo de ASP.NET aplican las reglas de autorización de dirección URL diferente para estático en comparación con contenido dinámico.) Puesto que deseamos prohibir el acceso a la `~/Admin` carpeta excepto los usuarios de la función Admin, tenemos que agregar reglas de autorización de dirección URL para esta carpeta. En concreto, las reglas de autorización de dirección URL deben permitir que a los usuarios en el rol de administrador y denegar a todos los otros usuarios. Esto se logra agregando una `Web.config` del archivo a la `~/Admin` carpeta con el siguiente contenido:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Para obtener más información sobre la característica de autorización de URL de ASP.NET s y cómo usarlo para explicar las reglas de autorización para usuarios y roles, no olvide leer el [ *autorización basada en usuario* ](../../older-versions-security/membership/user-based-authorization-vb.md) y [ *Autorización basada en roles* ](../../older-versions-security/roles/role-based-authorization-vb.md) tutoriales desde mi [ *tutoriales de seguridad del sitio Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Implementar una aplicación Web que usa servicios de aplicaciones

Al implementar un sitio Web que usa servicios de aplicación y un proveedor que almacena la información de servicios de aplicación en una base de datos, es imperativo que se crean los objetos de base de datos necesarios por los servicios de aplicación en la base de datos de producción. Inicialmente la base de datos de producción no contiene estos objetos, lo cuando la aplicación se implementó por primera vez (o cuando se implementa por primera vez después de han agregado los servicios de aplicación), debe seguir pasos adicionales para que estos objetos de base de datos necesarios el base de datos de producción.

Otro desafío puede surgir al implementar un sitio Web que usa servicios de aplicaciones si va a replicar las cuentas de usuario creadas en el entorno de desarrollo al entorno de producción. Según la configuración de pertenencia y funciones, es posible que incluso si copia correctamente las cuentas de usuario que se crearon en el entorno de desarrollo para la base de datos de producción, estos usuarios no pueden iniciar sesión en la aplicación web en producción. Se examinará la causa de este problema y se describe cómo evitar que suceda.

ASP.NET incluye una agradable [ *herramienta de administración del sitio Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) que pueden iniciarse desde Visual Studio y permite al usuario de cuenta, roles, reglas de autorización y administrarse a través de basada en web interfaz. Lamentablemente, el WSAT solo funciona para los sitios Web locales, lo que significa que no puede utilizarse para administrar las cuentas de usuario, roles y las reglas de autorización para la aplicación web en el entorno de producción de forma remota. Veremos diferentes formas de implementar un comportamiento similar a WSAT desde su sitio Web de producción.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Adición de objetos de base de datos mediante aspnet\_regsql.exe

El *implementar una base de datos* tutorial le ha mostrado cómo copiar las tablas y datos de la base de datos de desarrollo para la base de datos de producción, y estas técnicas sin duda pueden utilizarse para copiar los objetos de base de datos de servicios de aplicación del base de datos de producción. Otra opción es la `aspnet_regsql.exe` herramienta, que agrega o quita los objetos de base de datos de servicios de aplicación de una base de datos.

> [!NOTE]
> El `aspnet_regsql.exe` herramienta crea los objetos de base de datos en una base de datos especificado. No migra los datos en los objetos de base de datos de la base de datos de desarrollo para la base de datos de producción. Si desea copiar la información de cuenta y el rol de usuario de la base de datos de desarrollo en la base de datos de producción, use las técnicas descritas en la *implementar una base de datos* tutorial.


Permiten s veamos cómo agregar los objetos de base de datos a la base de datos de producción mediante el `aspnet_regsql.exe` herramienta. Iniciar, abrir el Explorador de Windows y navegue hasta el directorio de la versión 2.0 de .NET Framework en el equipo, %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Debería encontrar la `aspnet_regsql.exe` herramienta. Esta herramienta puede usarse desde la línea de comandos, pero también incluye una interfaz gráfica de usuario; Haga doble clic en el `aspnet_regsql.exe` archivo para iniciar el componente de gráficos.

La herramienta se inicia al mostrar una pantalla de presentación que explica su propósito. Haga clic en siguiente para pasar a la pantalla "Seleccionar una opción de configuración", que se muestra en la figura 1. Desde aquí puede agregar objetos de base de datos de los servicios de aplicación o quitarlos de una base de datos. Puesto que deseamos agregar estos objetos a la base de datos de producción, seleccione la opción "Configurar SQL Server para servicios de aplicación" y haga clic en siguiente.


[![Elija configurar SQL Server para servicios de aplicación](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Figura 1**: optar por configurar SQL Server para servicios de aplicación ([haga clic aquí para ver imagen en tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


En "Seleccionar el servidor y base de datos" pantalla pide información para conectarse a la base de datos. Escriba el nombre de la base de datos proporcionado por su empresa de hospedaje web, las credenciales de seguridad y el servidor de base de datos y haga clic en siguiente.

> [!NOTE]
> Después de escribir el servidor de base de datos y las credenciales puede obtener un error al expandir la lista desplegable de bases de datos. El `aspnet_regsql.exe` herramienta consultas el `sysdatabases` tabla del sistema para recuperar una lista de bases de datos en el servidor, pero algunos web que hospeda los servidores de base de datos de bloqueo de las empresas para que esta información no está disponible públicamente. Si recibe este error puede escribir el nombre de base de datos directamente en la lista desplegable.


[![Proporcionar a la herramienta con la información de conexión de base de datos s](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Figura 2**: proporcione la información de conexión de herramienta con la base de datos s ([haga clic aquí para ver imagen en tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


La pantalla posterior resume las acciones que se va a tener lugar, es decir que los objetos de base de datos de servicios de aplicación se van a agregar a la base de datos especificado. Haga clic en siguiente para completar esta acción. Transcurridos unos instantes, se muestra la última pantalla, teniendo en cuenta que se han agregado los objetos de base de datos (consulte la figura 3).


[![¡Success! Los objetos de base de datos de servicios de aplicación se agregaron a la base de datos de producción](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Figura 3**: Success! La aplicación Servicios de base de datos de objetos se agregaron a la base de datos de producción ([haga clic aquí para ver imagen en tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Para comprobar que los objetos de base de datos de servicios de aplicación se agregaron correctamente a la base de datos de producción, abra SQL Server Management Studio y conéctese a la base de datos de producción. Como se muestra en la figura 4, ahora debería ver las tablas de base de datos de servicios de aplicación en la base de datos `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, y así sucesivamente.


[![Confirme que los objetos de base de datos se agregaron a la base de datos de producción](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Figura 4**: confirme que los objetos de base de datos se agregaron a la base de datos de producción ([haga clic aquí para ver imagen en tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Sólo deberá usar el `aspnet_regsql.exe` herramienta al implementar la aplicación web por primera vez o por primera vez después de haber iniciado mediante los servicios de aplicación. Una vez que estos objetos de base de datos se encuentran en la base de datos de producción ganó t necesidad de volver a added o modified.

### <a name="copying-user-accounts-from-development-to-production"></a>Copiar cuentas de usuario de desarrollo a producción

Cuando se usa el `SqlMembershipProvider` y `SqlRoleProvider` clases de proveedor para almacenar la información de servicios de aplicación en una base de datos de SQL Server, la información de cuenta y el rol de usuario se almacena en una variedad de las tablas de base de datos, incluidas `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, y `aspnet_UsersInRoles`, entre otros. Si durante el desarrollo de crea cuentas de usuario en el entorno de desarrollo puede replicar esas cuentas de usuario en producción mediante la copia de los registros correspondientes de las tablas de base de datos aplicable. Si usa al Asistente para publicación de base de datos para implementar los objetos de base de datos de servicios de aplicación es posible que también decidió copiar los registros, lo que daría lugar a las cuentas de usuario que creó en el desarrollo para estar también en producción. Sin embargo, dependiendo de las opciones de configuración, es posible que los usuarios cuyas cuentas se crearon en el desarrollo y copian en producción están posible iniciar sesión desde el sitio Web de producción. ¿Qué ventajas aporta?

El `SqlMembershipProvider` y `SqlRoleProvider` clases de proveedor se ha diseñado para que una sola base de datos podría servir como un almacén de usuario para varias aplicaciones, donde cada aplicación podría, en teoría, tienen usuarios con los nombres de usuario que se superponen y funciones con el mismo nombre. Para permitir esta flexibilidad, la base de datos mantiene una lista de aplicaciones en la `aspnet_Applications` tabla y cada usuario está asociado a una de estas aplicaciones. En concreto, el `aspnet_Users` tabla tiene un `ApplicationId` columna que vincula a un registro en cada usuario el `aspnet_Applications` tabla.

Además el `ApplicationId` columna, el `aspnet_Applications` tabla también incluye un `ApplicationName` columna, que proporciona un nombre más fácil de usar para la aplicación. Cuando un sitio Web intenta trabajar con una cuenta de usuario, como la validación de un credenciales de usuario s en la página de inicio de sesión, debe indicar el `SqlMembershipProvider` qué aplicación para que funcione con la clase. Lo normal esto proporcionando el nombre de la aplicación y este valor procede de la configuración del proveedor s en `Web.config` : específicamente a través de la `applicationName` atributo.

¿Pero qué sucede si el `applicationName` no se especifica el atributo en `Web.config`? En tal caso, la pertenencia a un sistema usa la ruta de acceso raíz de aplicación como el `applicationName` valor. Si el `applicationName` atributo no se establece explícitamente `Web.config`, a continuación, es posible que el entorno de desarrollo y el entorno de producción use una raíz de aplicación diferentes y, por tanto, se asociarán con otra aplicación nombres de los servicios de aplicación. Si se produce una discrepancia de este tipo, a continuación, los usuarios creados en el entorno de desarrollo tendrán un `ApplicationId` valor que no coincide con el `ApplicationId` valor para el entorno de producción. El resultado neto es que los usuarios que ganó t podrán iniciar sesión.

> [!NOTE]
> Si se encuentra en esta situación - con cuentas de usuario que se copian en producción con una que no coinciden `ApplicationId` valor - podría escribir una consulta para actualizar estos incorrecto `ApplicationId` valores para el `ApplicationId` utilizado en producción. Una vez actualizado, los usuarios cuyas cuentas se crearon en el entorno de desarrollo ahora podrán iniciar sesión en la aplicación web en producción.


La buena noticia es que hay un paso simple que puede seguir para garantizar que los dos entornos usan el mismo `ApplicationId` : establece explícitamente el `applicationName` atributo `Web.config` para todos los proveedores de servicios de aplicación. Establezca explícitamente la `applicationName` atributo para "BookReviews" en el `<membership>` y `<roleManager>` elementos como este fragmento de código `Web.config` muestra.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Para obtener más información sobre cómo el `applicationName` atributo y el razonamiento subyacente a, hacen referencia a [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) entrada de blog s [ *siempre establece el nombre de aplicación propiedad al configurar la pertenencia a ASP.NET y otros proveedores*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Administrar cuentas de usuario en el entorno de producción

La herramienta de administración del sitio Web de ASP.NET (WSAT) facilita las tareas de crear y administrar cuentas de usuario, definir y aplicar funciones y explicar las reglas de autorización basada en roles y usuarios. Puede iniciar el WSAT desde Visual Studio, vaya al explorador de soluciones y haciendo clic en el icono de configuración de ASP.NET, o bien, vaya a los menús del sitio Web o proyecto y seleccione el elemento de menú de configuración de ASP.NET. Lamentablemente, el WSAT sólo puede trabajar con sitios Web locales. Por lo tanto, no puede usar el WSAT desde su estación de trabajo para administrar el sitio Web en el entorno de producción.

La buena noticia es que toda la funcionalidad expuesta proporcionada por el WSAT está disponible mediante programación a través de la pertenencia y funciones de API; Además, muchas de las pantallas WSAT usan los controles relacionados con el inicio de sesión de ASP.NET estándar. En resumen, puede agregar páginas ASP.NET al sitio Web que ofrecen las funcionalidades de administración necesarios.

Recuerde que un tutorial anterior actualiza la aplicación web de reseñas de libros para incluir un `~/Admin` y esta carpeta se ha configurado para permitir solo a los usuarios en el rol de administrador. He agregado una página a la carpeta denominada `CreateAccount.aspx` desde que un administrador puede crear una nueva cuenta de usuario. Esta página usa el control CreateUserWizard para mostrar la lógica de back-end y de interfaz de usuario para crear una nueva cuenta de usuario. Novedades más, personalizar el control para incluir un control CheckBox que pregunta si también se debe agregar al nuevo usuario al rol de administrador (consulte la figura 5). Con un poco de trabajo puede crear un conjunto de páginas personalizado que implementa las tareas relacionadas con la administración usuarios y roles que el WSAT proporcionaría en caso contrario.

> [!NOTE]
> Para obtener más información sobre el uso de la pertenencia y funciones de API junto con los controles Web relacionados con el inicio de sesión de ASP.NET, no olvide leer mi [ *tutoriales de seguridad del sitio Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Para obtener más información sobre la personalización del control CreateUserWizard, consulte el [ *crear cuentas de usuario* ](../../older-versions-security/membership/creating-user-accounts-vb.md) y [ *almacenar información de usuario adicional* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) tutoriales o desproteger [ *Erich Peterson* ](http://www.erichpeterson.com/) artículo, [ *personalizar el CreateUserWizard Control* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Los administradores pueden crear nuevas cuentas de usuario](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Figura 5**: los administradores pueden crear nuevas cuentas de usuario ([haga clic aquí para ver imagen en tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Si necesita la funcionalidad completa de la desprotección WSAT [ *gradual su propio sitio Web de herramienta de administración*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), en que el autor Dan Clem le guía a través del proceso de creación de una herramienta similar a WSAT personalizada. Dan comparte su código de origen s aplicación (en C#) y proporciona instrucciones paso a paso para agregarlo a su sitio Web hospedado.

## <a name="summary"></a>Resumen

Al implementar una aplicación web que usa la implementación de base de datos de servicios de aplicación primero debe asegurarse de que la base de datos de producción tiene los objetos de base de datos necesaria. Estos objetos se pueden agregar mediante las técnicas descritas en la *implementar una base de datos* tutorial; también puede usar el `aspnet_regsql.exe` herramienta, como hemos visto en este tutorial. Otros desafíos que hicimos alusión se centran en el nombre de aplicación que se usan en los entornos de desarrollo y producción (lo que es importante si desea que los usuarios y roles creados en el entorno de desarrollo sea válido en producción) y las técnicas para la sincronización administración de los usuarios y roles en el entorno de producción.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [*Herramienta de registro de servidor SQL de ASP.NET (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Creación de la base de datos de servicios de aplicación para SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Crear el esquema de pertenencia en SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Examen de perfil, Roles y pertenencia ASP.NET s*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Implementar su propia herramienta de administración de sitios Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Tutoriales de seguridad del sitio Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Información general de herramienta de administración de sitio Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Anterior](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Siguiente](strategies-for-database-development-and-deployment-vb.md)
