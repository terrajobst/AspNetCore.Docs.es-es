---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Inicio de sesión único (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Inicio de sesión único (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Existen diversos problemas de seguridad que preocuparse de si está desarrollando una aplicación de nube, pero para esta serie nos centraremos en una sola: inicio de sesión único. Personas de una pregunta a menudo se preguntan es: "Estoy principalmente compilar aplicaciones de los empleados de mi empresa; cómo hospedar estas aplicaciones en la nube y aún que puedan utilizar el mismo modelo de seguridad que Mis empleados conocer y utilizar en el entorno local cuando se ejecutan las aplicaciones que se hospedan dentro del firewall?" Una de las formas que se habilitan este escenario se llama Azure Active Directory (Azure AD). Azure AD le permite disponer de enterprise aplicaciones de línea de negocio (LOB) a través de Internet, y permite que estas aplicaciones estén disponibles para los socios comerciales así.

## <a name="introduction-to-azure-ad"></a>Introducción a Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) proporciona [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) en la nube. Características clave son los siguientes:

- Se integra con la instancia local de Active Directory.
- Permite un inicio de sesión único con sus aplicaciones.
- Admite estándares abiertos como [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), y [OAuth 2.0](http://oauth.net/2/).
- Es compatible con Enterprise [API de REST Graph](https://msdn.microsoft.com/library/hh974476.aspx).

Suponga que tiene un entorno de Windows Server Active Directory local que usas para permitir que los empleados iniciar sesión aplicaciones de Intranet:

![](single-sign-on/_static/image1.png)

¿Qué Azure AD le permite hacer es crear un directorio en la nube. Es una característica gratuita y fácil de configurar.

Puede ser completamente independiente de su Active Directory local; puede colocar cualquier persona que desee en él y realiza su autenticación en aplicaciones de Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

También puede integrar con sus instalaciones en AD.

![AD e integración de WAAD](single-sign-on/_static/image3.png)

Ahora todos los empleados que pueden realizar la autenticación local también pueden autenticar a través de Internet, sin tener que abrir un servidor de seguridad o implementar nuevos servidores en su centro de datos. Puede continuar aprovechar todo el entorno de Active Directory existente que conocer y usar hoy para proporcionar a su aplicaciones internas inicio de sesión único de capacidad.

Una vez que haya realizado esta conexión entre AD y Azure AD, también puede habilitar las aplicaciones web y los dispositivos móviles para autenticar a los empleados en la nube y puede habilitar las aplicaciones de terceros, como Office 365, SalesForce.com, aplicaciones o de Google, para aceptar el credenciales de los empleados. Si usa Office 365, está ya ha configurado con Azure AD porque Office 365 usa Azure AD para la autenticación y autorización.

![3ª aplicaciones de terceros](single-sign-on/_static/image4.png)

La belleza de este enfoque es que siempre que su organización agrega o elimina un usuario, o un usuario cambia una contraseña, utilice el mismo proceso que usa actualmente en su entorno local. Todos los de sus instalaciones AD cambios se propagan automáticamente al entorno de nube.

Si está utilizando o mover a Office 365, la ventaja es que su empresa, tendrá que Azure AD configura automáticamente porque Office 365 usa Azure AD para la autenticación. Por lo tanto, puede usar en sus propias aplicaciones de la misma autenticación que usa Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurar un inquilino de Azure AD

un directorio de Azure AD se conoce como un Azure AD [inquilino](https://technet.microsoft.com/library/jj573650.aspx), y la configuración de un inquilino es bastante fácil. Le mostraremos cómo se realiza en el Portal de administración de Azure para ilustrar los conceptos, pero por supuesto como las demás funciones portales también puede hacerlo mediante el uso de una secuencia de comandos o la API de administración.

En el portal de administración haga clic en la pestaña de Active Directory.

![WAAD en el portal](single-sign-on/_static/image5.png)

Automáticamente tiene un inquilino de Azure AD para su cuenta de Azure y puede hacer clic en el **agregar** situado en la parte inferior de la página para crear directorios adicionales. Conviene uno para un entorno de prueba y otro para producción, por ejemplo. Medite concienzudamente sobre lo que asigne un nombre a un nuevo directorio. Si usa el nombre para el directorio y, a continuación, usar el nombre nuevo para uno de los usuarios, que puede ser confusa.

![Agregar directorio](single-sign-on/_static/image6.png)

El portal es totalmente compatible con para crear, eliminar y administrar los usuarios dentro de este entorno. Por ejemplo, para agregar un usuario vaya a la **usuarios** ficha y haga clic en el **Agregar usuario** botón.

![Botón Agregar usuario](single-sign-on/_static/image7.png)

![Agregar cuadro de diálogo usuario](single-sign-on/_static/image8.png)

Puede crear un nuevo usuario que existe solo en este directorio, o puede registrar una Account de Microsoft como un usuario en este directorio, o registrar o un usuario de otro directorio de Azure AD como un usuario en este directorio. (En un directorio real, el dominio predeterminado sería ContosoTest.onmicrosoft.com. También puede usar un dominio de su propia elección, como contoso.com.)

![Tipos de usuario](single-sign-on/_static/image9.png)

![Agregar cuadro de diálogo usuario](single-sign-on/_static/image10.png)

Puede asignar al usuario a un rol.

![Perfil de usuario](single-sign-on/_static/image11.png)

Y la cuenta se crea con una contraseña temporal.

![Contraseña temporal](single-sign-on/_static/image12.png)

Los usuarios se crean de esta forma pueden inmediatamente iniciar sesión en las aplicaciones web mediante este directorio en la nube.

¿Qué es excelente para inicio de sesión único empresarial, sin embargo, es el **integración de directorios** ficha:

![Ficha de integración de directorio](single-sign-on/_static/image13.png)

Si habilita la integración de directorios, y [descargue una herramienta](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), puede sincronizar este directorio en la nube con su Active Directory existente de locales que ya está usando dentro de la organización. A continuación, todos los usuarios almacenados en el directorio se mostrarán en este directorio en la nube. Las aplicaciones de nube ya pueden autenticarse en todos los empleados con sus credenciales de Active Directory existentes. Y todo esto es libre: la herramienta de sincronización y Azure AD.

La herramienta es un asistente que es fácil de usar, como puede ver en estas capturas de pantalla. Estas no son instrucciones completas, solo un ejemplo que muestra el proceso básico. Para obtener más información de métodos-a--it, vea los vínculos de la [recursos](#resources) sección al final del capítulo.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image14.png)

Haga clic en **siguiente**y, a continuación, escriba sus credenciales de Azure Active Directory.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image15.png)

Haga clic en **siguiente**y, a continuación, escriba sus instalaciones en credenciales de AD.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image16.png)

Haga clic en **siguiente**y, a continuación, indique si desea almacenar un hash de las contraseñas de AD en la nube.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image17.png)

El hash de contraseña que se puede almacenar en la nube es un hash unidireccional; las contraseñas reales nunca se almacenan en Azure AD. Si decide almacenar valores hash en la nube, tendrá que usar [los servicios de federación de Active Directory](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). También hay [otros factores a tener en cuenta al elegir si se debe o no usar ADFS](https://technet.microsoft.com/library/jj573653.aspx). La opción de AD FS requiere algunos pasos de configuración adicionales.

Si decide almacenar valores hash en la nube, haya terminado y la herramienta inicia la sincronización de directorios al hacer clic en **siguiente**.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image18.png)

Y, dentro de unos minutos haya terminado.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image19.png)

Solamente debe ejecutar esto en un controlador de dominio en la organización, en Windows 2003 o posterior. Y no se necesita reiniciar. Cuando haya terminado, todos los usuarios están en la nube y puede realizar un inicio de sesión único desde cualquier web o aplicaciones móviles, con SAML, OAuth o WS-Fed.

A veces nos preguntan acerca de cómo proteger es: Microsoft usa para sus propios datos empresariales confidenciales? Y la respuesta es Sí, que hacemos. Por ejemplo, si va al sitio de SharePoint de Microsoft interno en [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), se le pida que inicie sesión.

![Inicio de sesión en Office 365](single-sign-on/_static/image20.png)

Microsoft ha habilitado ADFS, por lo que al especificar un identificador de Microsoft, se le redirigirá a una página de inicio de sesión ADFS.

![Inicio de sesión de ADFS](single-sign-on/_static/image21.png)

Y una vez que escriba las credenciales almacenadas en una cuenta de Microsoft AD interna, tener acceso a esta aplicación interno.

![Sitio de SharePoint de MS](single-sign-on/_static/image22.png)

Estamos usando un servidor de inicio de sesión de AD principalmente porque ya ha surgido ADFS configurar antes de Azure AD empezó a estar disponible, pero el proceso de registro se va a través de un directorio de Azure AD en la nube. Se colocan nuestros documentos importantes, control de código fuente, archivos de administración de rendimiento, los informes de ventas etc., en la nube y está usando esta misma solución exacta para protegerlas.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Crear una aplicación ASP.NET que usa Azure AD para inicio de sesión único

Visual Studio simplifica mucho la tarea crear una aplicación que usa Azure AD para inicio de sesión único, como puede ver en algunas capturas de pantalla.

Cuando se crea una nueva aplicación de ASP.NET MVC o formularios Web Forms, el método de autenticación predeterminado es ASP.NET Identity. Para cambiar a Azure AD, hacer clic en un **Cambiar autenticación** botón.

![Cambio de la autenticación](single-sign-on/_static/image23.png)

Seleccione las cuentas organizativas, escriba el nombre de dominio y, a continuación, seleccione Inicio de sesión único.

![Configurar el cuadro de diálogo autenticación](single-sign-on/_static/image24.png)

También puede proporcionar a la lectura de la aplicación o permiso para datos de directorio de lectura/escritura. Si lo hace, puede usar el [API de REST Graph de Azure](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) para buscar el número de teléfono de los usuarios, averiguar si se encuentran en la oficina, cuando última ha iniciado, etcetera.

Eso es todo lo que tiene que hacer - Visual Studio pide las credenciales para un administrador de inquilino de Azure AD y, a continuación, Establece el proyecto y el inquilino de Azure AD para la nueva aplicación.

Al ejecutar el proyecto, verá una página de inicio de sesión y puede iniciar sesión con credenciales de un usuario en su directorio Azure AD.

![Inicio de sesión de cuenta de organización](single-sign-on/_static/image25.png)

![Ha iniciado sesión](single-sign-on/_static/image26.png)

Al implementar la aplicación en Azure, lo único que debe hacer es seleccionar un **Habilitar autenticación organizativa** casilla de verificación y, de nuevo Visual Studio se encarga de toda la configuración para usted.

![Publicar sitio Web](single-sign-on/_static/image27.png)

Estas capturas de pantalla proceden de un tutorial paso a paso completo que muestra cómo crear una aplicación que utiliza autenticación de Azure AD: [desarrollar aplicaciones de ASP.NET con Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Resumen

En este capítulo se vio que Azure Active Directory, Visual Studio y ASP.NET, resulta fácil configurar el inicio de sesión único en aplicaciones de Internet para los usuarios de su organización. Los usuarios pueden iniciar sesión en las aplicaciones de Internet con las mismas credenciales que se usan para iniciar sesión a través de Active Directory en la red interna.

El [siguiente capítulo](data-storage-options.md) se examinan las opciones de almacenamiento de datos disponibles para una aplicación de nube.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

- [Documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Página del portal de documentación de Azure AD en el sitio windowsazure.com. Para ver los tutoriales paso a paso, consulte el **desarrollar** sección.
- [La autenticación multifactor Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Página del portal para obtener documentación sobre la autenticación multifactor en Azure.
- [Opciones de autenticación de cuenta profesional](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explicación de las opciones de autenticación de Azure AD en el cuadro de diálogo del nuevo proyecto de Visual Studio 2013.
- [Microsoft patrones y prácticas - federado modelo de identidad](https://msdn.microsoft.com/library/dn589790.aspx).
- [Cómo: Instalar la herramienta de sincronización de Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Servicios de federación de Active Directory 2.0 mapa de contenido](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Vínculos a documentación sobre AD FS 2.0.
- [Autorización basada en roles y basada en ACL en una aplicación de Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Aplicación de ejemplo.
- [Blog de Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Control de acceso en BYOD y la integración de directorios en una infraestructura de identidad híbrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 sesión vídeo Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Anterior](web-development-best-practices.md)
> [Siguiente](data-storage-options.md)
