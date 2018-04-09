---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Conceptos básicos de seguridad y compatibilidad de ASP.NET (VB) | Documentos de Microsoft
author: rick-anderson
description: Este es el primer tutorial en una serie de tutoriales que se explorarán técnicas para autenticar a los visitantes a través de un formulario web Forms, autorizar el acceso a partic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: e62bb865e211a279b60f3120162ffc3c49cbdcc5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="security-basics-and-aspnet-support-vb"></a>Conceptos básicos de seguridad y compatibilidad de ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga de PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Este es el primer tutorial de una serie de tutoriales que se explorarán técnicas para autenticar a los visitantes a través de un formulario web Forms, autorizar el acceso a determinadas páginas y la funcionalidad y administrar cuentas de usuario en una aplicación ASP.NET.


## <a name="introduction"></a>Introducción

¿Qué es la foros de algo, sitios de comercio electrónico, sitios Web de correo electrónico en línea, sitios Web del portal y todos tienen en común de sitios de redes sociales? Todos los ofrecen *cuentas de usuario*. Los sitios que ofrecen las cuentas de usuario deben proporcionar un número de servicios. Como mínimo, nuevos visitantes deben poder crear una cuenta y devolver los visitantes deben ser capaz de iniciar sesión. Estas aplicaciones web pueden tomar decisiones basadas en la sesión del usuario: algunas páginas o acciones podrían estar restringidas solo se registran en los usuarios o a un determinado subconjunto de los usuarios; otras páginas podrían mostrar información específica a la sesión del usuario, o pueden mostrar más o menos información, dependiendo de qué usuario está viendo la página.

Este es el primer tutorial de una serie de tutoriales que se explorarán técnicas para autenticar a los visitantes a través de un formulario web Forms, autorizar el acceso a determinadas páginas y la funcionalidad y administrar cuentas de usuario en una aplicación ASP.NET. En el transcurso de estos tutoriales examinaremos cómo:

- Identificar y registrar los usuarios de un sitio Web
- Use ASP. Framework de pertenencia de .NET para administrar las cuentas de usuario
- Crear, actualizar y eliminar cuentas de usuario
- Limitar el acceso a una página web, el directorio o la funcionalidad específica en función de la sesión del usuario
- Use ASP. Framework de Roles de .NET para asociar cuentas de usuario a roles
- Administrar roles de usuario
- Limitar el acceso a una página web, el directorio o la funcionalidad específica según la función del usuario ha iniciado sesión
- Personalizar y extender ASP. Controles de Web de seguridad de red

Estos tutoriales están pensados para ser conciso y se proporcionan instrucciones paso a paso con una gran cantidad de capturas de pantalla para guiarle a través del proceso visualmente. Cada tutorial está disponible en versiones de Visual Basic y C# e incluye la descarga de código completo usado. (Este primer tutorial se centra en los conceptos de seguridad desde un punto de vista de alto nivel y, por tanto, no contiene ningún código asociado).

En este tutorial, veremos los conceptos de seguridad importantes y qué funciones están disponibles en ASP.NET para ayudar en la implementación de autenticación de formularios, autorización, cuentas de usuario y roles. Comencemos.

> [!NOTE]
> La seguridad es un aspecto importante de cualquier aplicación que abarca físicos, tecnológicos y las decisiones de directiva y requiere un alto grado de conocimientos de planeamiento y dominio. Esta serie de tutoriales no está pensada como una guía para el desarrollo de aplicaciones web seguras. En su lugar, se centra específicamente en la autenticación de formularios, autorización, cuentas de usuario y roles. Mientras que algunos conceptos de seguridad relacionados con estos problemas se tratan en esta serie, se dejan otras zonas inexploradas.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Autenticación, autorización, cuentas de usuario y Roles

Autenticación, autorización, cuentas de usuario y roles son los cuatro términos que se utilizará con frecuencia a lo largo de esta serie de tutoriales, por lo que me gustaría tardar unos instantes rápido para definir estos términos en el contexto de seguridad de la web. En un modelo cliente / servidor, como Internet, hay muchos escenarios en los que el servidor debe identificar al cliente que realiza la solicitud. *Autenticación* es el proceso de determinar la identidad del cliente. Se dice que un cliente que se ha identificado correctamente *autenticado*. Se dice que un cliente no identificado *no autenticadas* o *anónimo*.

Sistemas de autenticación segura incluyen al menos una de las tres facetas siguientes: [algo que usted sabe, algo que usted tiene o algo hay](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Mayoría de las aplicaciones web se basan en algo que el cliente conoce, por ejemplo, una contraseña o un PIN. La información utilizada para identificar a un usuario - su nombre de usuario y una contraseña, por ejemplo - se conocen como *credenciales*. Esta serie de tutoriales se centra en *autenticación mediante formularios*, que es un modelo de autenticación donde los usuarios iniciar sesión en el sitio al proporcionar sus credenciales en un formulario de página web. Todos los que hemos obtenido este tipo de autenticación antes de. Vaya a cualquier sitio de comercio electrónico. Cuando esté listo para desproteger deberá iniciar sesión escribiendo su nombre de usuario y contraseña en los cuadros de texto en una página web.

Además de identificar los clientes, un servidor que necesite limitar qué recursos o funcionalidades están accesibles dependiendo del cliente que realiza la solicitud. *Autorización* es el proceso de determinar si un usuario determinado tiene la autoridad para tener acceso a un recurso específico o funcionalidad.

A *cuenta de usuario* es un almacén para conservar información sobre un usuario determinado. Las cuentas de usuario como mínimo deben incluir información que identifica de forma única el usuario, como nombre de inicio de sesión del usuario y la contraseña. Junto con esta información esencial, cuentas de usuario pueden incluir elementos tales como: dirección de correo electrónico del usuario; la fecha y hora en que se creó la cuenta; la fecha y hora, última se registran en; nombre y apellido; número de teléfono; y dirección de correo. Cuando se utiliza la autenticación de formularios, normalmente se almacena la información de la cuenta de usuario en una base de datos relacional como Microsoft SQL Server.

Aplicaciones Web que admiten las cuentas de usuario, opcionalmente, pueden agrupar a los usuarios en *roles*. Un rol es simplemente una etiqueta que se aplica a un usuario y proporciona una abstracción para definir las reglas de autorización y la funcionalidad de nivel de página. Por ejemplo, un sitio Web puede incluir un rol de administrador con las reglas de autorización que prohíben cualquiera que no sea un administrador para obtener acceso a un conjunto determinado de páginas web. Además, una serie de páginas que son accesibles para todos los usuarios (incluidos los usuarios no administradores) puede mostrar datos adicionales u ofrecen una funcionalidad adicional cuando visita por los usuarios del rol de administradores. Uso de roles, podemos definir estas reglas de autorización en una base de rol por rol en lugar de por usuario.

## <a name="authenticating-users-in-an-aspnet-application"></a>Autenticar a los usuarios en una aplicación ASP.NET

Cuando un usuario escribe una dirección URL en su explorador ventana de dirección o hace clic en un vínculo, el explorador realiza una [protocolo de transferencia de hipertexto (HTTP)](http://en.wikipedia.org/wiki/HTTP) solicitar al servidor web para el contenido especificado, ya sea un ASP.NET página, una imagen, JavaScript archivo, o cualquier otro tipo de contenido. El servidor web se encarga de devolver el contenido solicitado. Si lo hace, debe determinar una serie de cuestiones acerca de la solicitud, como quién realizó la solicitud y si la identidad está autorizada para recuperar el contenido solicitado.

De forma predeterminada, los exploradores envían las solicitudes HTTP que no tienen a ningún tipo de información de identificación. Pero si el explorador incluye información de autenticación, a continuación, el servidor web inicia el flujo de trabajo de autenticación, que intenta identificar al cliente que realiza la solicitud. Los pasos del flujo de trabajo de autenticación dependen del tipo de autenticación utilizado por la aplicación web. ASP.NET admite tres tipos de autenticación: Windows, Passport y formularios. Esta serie de tutoriales se centra en la autenticación de formularios, pero Echemos un minuto para comparar y contrastar el flujo de trabajo y almacenes de usuario de autenticación de Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticación mediante la autenticación de Windows

El flujo de trabajo de autenticación de Windows usa una de las técnicas de autenticación siguientes:

- Autenticación básica
- Autenticación implícita
- Autenticación integrada de Windows

Todas las técnicas de tres funcionan aproximadamente la misma manera: cuando una no autorizado, llega una solicitud anónima, el servidor web devuelve una respuesta HTTP que indica que autorización es necesaria para continuar. El explorador, a continuación, muestra un cuadro de diálogo modal que pide al usuario su nombre de usuario y contraseña (consulte la figura 1). Esta información se envía al servidor web a través de un encabezado HTTP.


![Cuadro de diálogo Modal pide al usuario para sus credenciales](security-basics-and-asp-net-support-vb/_static/image1.png)

**Figura 1**: cuadro de diálogo Modal pide al usuario para sus credenciales


Las credenciales proporcionadas se validan con el almacén de usuario de Windows del servidor web. Esto significa que cada usuario autenticado en la aplicación web debe tener una cuenta de Windows en su organización. Esto es muy frecuente en escenarios de intranet. De hecho, cuando se usa autenticación integrada de Windows en un entorno de intranet, el explorador proporciona automáticamente el servidor web con las credenciales usadas para iniciar sesión en la red, suprimiendo así activaciones el cuadro de diálogo que se muestra en la figura 1. Mientras que la autenticación de Windows es muy útil para aplicaciones de intranet, es imposible normalmente para aplicaciones de Internet debido a que no desea crear cuentas de Windows para cada usuario que se registra en el sitio.

### <a name="authentication-via-forms-authentication"></a>Autenticación mediante la autenticación de formularios

Autenticación de formularios, por otro lado, es ideal para las aplicaciones web de Internet. Recuerde que la autenticación de formularios identifica al usuario que hace es pedirle que especifique sus credenciales a través de un formulario web Forms. Por lo tanto, cuando un usuario intenta tener acceso a un recurso no autorizado, se redirige automáticamente a la página de inicio de sesión en el que pueden especificar sus credenciales. A continuación, se validan las credenciales presentadas en un almacén de usuario personalizada - normalmente una base de datos.

Después de comprobar las credenciales presentadas, un *vale de autenticación de formularios* se crea para el usuario. Este vale indica que el usuario se ha autenticado e incluye información de identificación, como el nombre de usuario. El vale de autenticación de formularios (normalmente) se almacena como una cookie en el equipo cliente. Por lo tanto, las visitas posteriores al sitio Web incluyen el vale de autenticación de formularios en la solicitud HTTP, lo que permite la aplicación web identificar al usuario una vez que han iniciado sesión en.

Figura 2 muestra el flujo de trabajo de autenticación de formularios desde un punto de estratégico de alto nivel. Observe cómo los componentes de autenticación y autorización en ASP.NET actúan como entidades independientes. El sistema de autenticación de formularios identifica al usuario (o informes que son anónimos). El sistema de autorización es lo que determina si el usuario tiene acceso al recurso solicitado. Si el usuario no autorizado (como aparecen en la figura 2 al tratar de forma anónima visite ProtectedPage.aspx), el sistema de autorización indica que el usuario se ha denegado, haciendo que los formularios de sistema de autenticación para redirigir automáticamente al usuario a la página de inicio de sesión.

Una vez que el usuario ha iniciado sesión correctamente, las solicitudes HTTP subsiguientes incluyen el vale de autenticación de formularios. El sistema de autenticación de formularios simplemente identifica al usuario: es el sistema de autorización que determina si el usuario puede acceder al recurso solicitado.


![El flujo de trabajo de autenticación de formularios](security-basics-and-asp-net-support-vb/_static/image2.png)

**Figura 2**: el flujo de trabajo de autenticación de formularios


Profundizar en la autenticación de formularios con mayor detalle en los siguientes dos tutoriales,[una visión general de autenticación mediante formularios](an-overview-of-forms-authentication-vb.md) y [configuración de autenticación de formularios y temas avanzados](forms-authentication-configuration-and-advanced-topics-vb.md). Para obtener más información sobre ASP. Opciones de autenticación de red, consulte [la autenticación de ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitar el acceso a páginas Web, directorios y funcionalidad de la página

ASP.NET incluye dos métodos para determinar si un usuario determinado tiene autoridad para tener acceso a un determinado archivo o directorio:

- **Autorización de archivo** : puesto que las páginas ASP.NET y servicios web se implementan como archivos que residen en el sistema de archivos del servidor web, acceso a estos archivos se pueden especificar a través de listas de Control de acceso (ACL). Autorización de archivo normalmente se utiliza con la autenticación de Windows porque las ACL son los permisos que se aplican a las cuentas de Windows. Cuando se utiliza la autenticación de formularios, todas las solicitudes de nivel de sistema operativo system - y archivos se ejecutan con la misma cuenta de Windows, independientemente del usuario visita el sitio.
- **Autorización de URL**-con autorización de URL, el desarrollador de páginas especifica las reglas de autorización en Web.config. Estas reglas de autorización especifican qué usuarios o roles tienen acceso o se deniegan el acceso a ciertas páginas o directorios de la aplicación.

Autorización de archivo y la autorización de dirección URL definen reglas de autorización para tener acceso a una página ASP.NET determinada o para todas las páginas ASP.NET en un directorio determinado. Con estas técnicas se podemos indicar a ASP.NET para denegar las solicitudes a una página concreta de un usuario determinado, o permitir el acceso a un conjunto de usuarios y denegar el acceso a todos los demás. ¿Qué escenarios donde todos los usuarios pueden tener acceso a la página, pero la funcionalidad de la página depende del usuario? Por ejemplo, muchos sitios que admiten las cuentas de usuario tienen páginas que mostrar contenido diferente o datos para los usuarios autenticados frente a los usuarios anónimos. Un usuario anónimo podría ver un vínculo para iniciar sesión en el sitio, mientras que un usuario autenticado en su lugar, debería ver un mensaje, como o Bienvenido de nuevo, *nombre de usuario* junto con un vínculo para cerrar la sesión. Otro ejemplo: cuando esté viendo un elemento en un sitio de subasta verá una información diferente dependiendo de que sea un usuario o la organización de subastas el elemento.

Estos ajustes de nivel de página pueden realizarse mediante declaración o mediante programación. Para mostrar contenido diferente para anónimo a los usuarios autenticados, basta con arrastrar un [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) en la página y escriba el contenido adecuado en sus plantillas AnonymousTemplate y LoggedInTemplate. Como alternativa, se pueden determinar mediante programación si se autentica la solicitud actual, que es el usuario y los roles que pertenecen a (si existe). Puede utilizar esta información para, a continuación, mostrar u ocultar columnas en una cuadrícula o paneles en la página.

Esta serie incluye tres tutoriales que se centran en la autorización. ***Autorización basada en usuario***examina cómo limitar el acceso a una página o páginas en un directorio para las cuentas de usuario específico; ***Basado en el rol de autorización*** examina proporcionando las reglas de autorización en el rol de nivel; por último, el ***mostrar contenido tomando como base el actualmente en el usuario que inició*** tutorial explora la modificación de un determinado la página contenido y la funcionalidad basada en el usuario visita la página. Para obtener más información sobre ASP. Opciones de autorización de red, consulte [autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Cuentas de usuario y Roles

ASP. Autenticación de formularios de NET proporciona una infraestructura para que los usuarios iniciar sesión en un sitio y su estado autenticado que recuerdan a través de visitas de página. Y autorización para URL ofrece un marco para limitar el acceso a determinados archivos o carpetas en una aplicación ASP.NET. Ninguna característica, sin embargo, proporciona un medio para almacenar información de la cuenta de usuario o administrar los roles.

Antes de ASP.NET 2.0, los desarrolladores eran responsables de crear sus propios almacenes de usuario y el rol. También estuviera en el enlace para diseñar interfaces de usuario y escribir el código de usuario esencial páginas relacionadas con la cuenta como la página de inicio de sesión y la página para crear una nueva cuenta, entre otros. Sin cualquier marco de la cuenta de usuario integrada en ASP.NET, cada programador cuentas de usuario de implementación se tenían que llegan a sus propias decisiones de diseño en preguntas como, ¿cómo almacenan las contraseñas u otra información confidencial? ¿y qué instrucciones debo imponer en relación con intensidad y longitud de contraseña?

En la actualidad, la implementación de las cuentas de usuario en una aplicación ASP.NET es mucho más fácil gracias a la *framework pertenencia* y controles Web de inicio de sesión integrado. El marco de trabajo de pertenencia es una serie de clases en el [espacio de nombres System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) que proporcionan la funcionalidad para realizar tareas relacionadas con la cuenta de usuario esenciales. La clase clave en el marco de trabajo de pertenencia es el [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que tiene métodos como:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Utiliza el marco de trabajo de la pertenencia a la [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa claramente las API de la pertenencia del marco de trabajo de su implementación. Esto permite a los desarrolladores usar una API común, pero permite a ellos para que use una implementación que satisfaga las necesidades de la personalización de la aplicación. En resumen, la clase de pertenencia define la funcionalidad fundamental de framework (métodos, propiedades y eventos), pero no proporciona realmente los detalles de implementación. En su lugar, los métodos de la clase de pertenencia al invocan el proveedor configurado, que es lo que realiza el trabajo real. Por ejemplo, cuando se invoca el método de la clase de pertenencia CreateUser, la clase de pertenencia no conoce los detalles del almacén de usuario. No sabe si los usuarios a que se mantienen en una base de datos o en un archivo XML o en otro almacén. La clase de pertenencia examina la configuración de la aplicación web para determinar qué proveedor para delegar la llamada a, y esa clase de proveedor es responsable de crear realmente la nueva cuenta de usuario en el almacén de usuario correspondiente. En la figura 3 se muestra esta interacción.

Microsoft incluye dos clases de proveedor de pertenencia en .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementa la API de pertenencia en los servidores de Active Directory y Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementa la API de pertenencia en una base de datos de SQL Server.

Esta serie de tutoriales se centra exclusivamente en SqlMembershipProvider.


[![El modelo permite diferentes implementaciones de proveedor siendo perfectamente conectado en el marco de trabajo](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Figura 03**: el modelo permite diferentes implementaciones de proveedor siendo perfectamente conectado en el marco de trabajo ([haga clic aquí para ver la imagen a tamaño completo](security-basics-and-asp-net-support-vb/_static/image5.png))


La ventaja del modelo de proveedor es que los implementaciones alternativas se pueden desarrolladas por Microsoft, los proveedores de terceros o los programadores individuales y conectadas sin problemas en el marco de trabajo de pertenencia. Por ejemplo, Microsoft ha lanzado [un proveedor de pertenencia para las bases de datos de Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Para obtener más información sobre los proveedores de pertenencia, vea la [Kit de herramientas de proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx), que incluye un tutorial de los proveedores de pertenencia, los proveedores personalizados de ejemplo, más de 100 páginas de documentación en el modelo de proveedor y la completar código fuente para los proveedores de pertenencia integrados (es decir, ActiveDirectoryMembershipProvider y SqlMembershipProvider).

ASP.NET 2.0 incorporó también el marco de trabajo de Roles. Al igual que el marco de trabajo de pertenencia, el marco de Roles se basa sobre el modelo de proveedor. La API se expone a través de la [clase Roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) y .NET Framework se distribuye con tres clases de proveedor:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -administra información de funciones en un almacén de directivas del Administrador de autorización, como Active Directory o ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementa funciones en una base de datos de SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -asocia información de funciones basada en el grupo de Windows del visitante. Este método se utiliza normalmente con la autenticación de Windows.

Esta serie de tutoriales se centra exclusivamente en SqlRoleProvider.

Dado que el modelo de proveedor incluye una única API orientados hacia adelante (las clases de pertenencia y Roles), es posible crear funcionalidad para esa API sin tener que preocuparse sobre los detalles de implementación: las son administradas por los proveedores seleccionados por la página Developer. Esta API unificada permite Microsoft y otros fabricantes crear controles Web que se comunican con los marcos de pertenencia y los Roles. ASP.NET incluye una serie de [controla el inicio de sesión Web](https://msdn.microsoft.com/library/ms178329.aspx) para la implementación de interfaces de usuario de cuenta de usuario comunes. Por ejemplo, el [control de inicio de sesión](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) pide al usuario sus credenciales, valida y, a continuación, inicia sesión en a través de la autenticación de formularios. El [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) ofrece plantillas para mostrar un marcado diferente a los usuarios anónimos frente a los usuarios autenticados o un marcado diferente según el rol del usuario. Y el [control CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) proporciona una interfaz de usuario de paso a paso para crear una nueva cuenta de usuario.

Interiormente los distintos controles de inicio de sesión interactúan con los marcos de pertenencia y los Roles. La mayoría de los controles de inicio de sesión se pueden implementar sin tener que escribir una sola línea de código. Examinaremos estos controles con mayor detalle en tutoriales futuros, incluidas las técnicas de extender y personalizar su funcionalidad.

## <a name="summary"></a>Resumen

Todas las aplicaciones web que admiten las cuentas de usuario requieren características similares, como: la capacidad de los usuarios iniciar sesión y tener su registro en estado recuerda todas las visitas de página; una página web para los nuevos visitantes crear una cuenta; y la capacidad para el desarrollador de páginas para especificar qué recursos, los datos y funcionalidad están disponibles para los usuarios o roles. Las tareas de autenticar y autorizar a los usuarios y de la administración de roles y cuentas de usuario es bastante fácil llevar a cabo en las aplicaciones ASP.NET gracias a la autenticación de formularios, la autorización de dirección URL y los marcos de pertenencia y los Roles.

En el transcurso de la siguientes varios tutoriales examinaremos estos aspectos al generar una aplicación web de trabajo desde el principio de un modo paso a paso. En el tutorial siguiente dos exploraremos autenticación de formularios en detalle. Vemos el flujo de trabajo de autenticación de formularios en acción, analizar minuciosamente el vale de autenticación de formularios, tratan cuestiones de seguridad y vea cómo configurar el sistema de autenticación de formularios; todo ello mientras se compila una aplicación web que permite a los visitantes a iniciar sesión y cerrar la sesión.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ASP.NET 2.0 pertenencia, Roles, la autenticación de formularios y recursos de seguridad](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Directrices de seguridad 2.0 de ASP.NET](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticación de ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Información general sobre controles de inicio de sesión de ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examen de ASP.NET del 2.0 perfil, Roles y pertenencia](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [¿Cómo proteger I: Mi sitio mediante la pertenencia y los Roles?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Introducción a la suscripción](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centro para desarrolladores de seguridad de MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 seguridad, la pertenencia y la administración de roles](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Kit de herramientas de proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era este tutorial serie se revisó por varios revisores útiles. Los revisores iniciales para este tutorial incluyen Alicja Maziarz, John Suru y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](forms-authentication-configuration-and-advanced-topics-cs.md)
> [Siguiente](an-overview-of-forms-authentication-vb.md)
