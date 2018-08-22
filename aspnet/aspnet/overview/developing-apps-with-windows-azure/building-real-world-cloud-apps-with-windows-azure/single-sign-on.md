---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Inicio de sesión único (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: feace28c373a767453b1b984f6fdc5a3d97a9217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827736"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Inicio de sesión único (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Hay muchos problemas de seguridad que pensar cuando está desarrollando una aplicación de nube, pero en esta serie nos centraremos en uno solo: inicio de sesión único. Personas de una pregunta a menudo preguntan esto es: "Estoy principalmente creando aplicaciones para los empleados de mi empresa; cómo hospedar estas aplicaciones en la nube y que todavía puedan utilizar el mismo modelo de seguridad que Mis empleados conocen y usar en el entorno local cuando se ejecutan las aplicaciones que se hospedan dentro del firewall?" Una de las maneras en que se habilitan este escenario se denomina Azure Active Directory (Azure AD). Azure AD le permite tomar enterprise aplicaciones de línea de negocio (LOB) disponibles a través de Internet y permite hacer que estas aplicaciones estén disponibles para asociados de negocios también.

## <a name="introduction-to-azure-ad"></a>Introducción a Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) proporciona [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) en la nube. Las características clave incluyen lo siguiente:

- Se integra con Active Directory local.
- Permite el inicio de sesión único con sus aplicaciones.
- Admite estándares abiertos como [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), y [OAuth 2.0](http://oauth.net/2/).
- Es compatible con Enterprise [API de REST Graph](https://msdn.microsoft.com/library/hh974476.aspx).

Supongamos que tiene un entorno de Windows Server Active Directory local que usar para permitir que los empleados iniciar sesión aplicaciones de Intranet:

![](single-sign-on/_static/image1.png)

Lo que Azure AD le permite hacer es crear un directorio en la nube. Es una característica gratuita y fácil de configurar.

Puede ser completamente independiente de su Active Directory local; puede colocar cualquier persona que desee en ella y autenticarse en ellos en aplicaciones de Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

O puede integrarlo con sus instalaciones en AD.

![AD e integración de WAAD](single-sign-on/_static/image3.png)

Ahora también pueden autenticar todos los empleados que pueden autenticarse en el entorno local a través de Internet, sin tener que abrir un firewall o implementar nuevos servidores en su centro de datos. Aún puede aprovechar todo el entorno de Active Directory existente que conocen y usan hoy en día para ofrecer a su aplicaciones internas inicio de sesión único en capacidad.

Una vez que haya realizado esta conexión entre AD y Azure AD, también puede habilitar las aplicaciones web y los dispositivos móviles para autenticar a los empleados en la nube y puede permitir que las aplicaciones de terceros, como aplicaciones de Office 365, SalesForce.com o Google, para aceptar su credenciales de los empleados. Si usa Office 365, está ya ha configurado con Azure AD como Office 365 usa Azure AD para la autenticación y autorización.

![3 aplicaciones de terceros](single-sign-on/_static/image4.png)

La ventaja de este enfoque es siempre que su organización agrega o elimina un usuario, o un usuario cambia una contraseña, usar el mismo proceso que usa hoy en día en su entorno local. Todos los de sus instalaciones AD cambios se propagan automáticamente al entorno de nube.

Si su empresa está usando o mover a Office 365, la buena noticia es que tendrá que Azure AD configura automáticamente porque Office 365 usa Azure AD para la autenticación. Por lo que puede usar fácilmente en sus propias aplicaciones de la misma autenticación que usa Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configuración de un inquilino de Azure AD

un directorio de Azure AD se denomina una Azure AD [inquilino](https://technet.microsoft.com/library/jj573650.aspx), y la configuración de un inquilino es bastante fácil. Le mostraremos cómo se hace en el Portal de administración de Azure con el fin de ilustrar los conceptos, pero por supuesto, como las demás funciones de portal también puede hacer mediante un script o la API de administración.

En el portal de administración, haga clic en la pestaña Active Directory.

![WAAD en el portal](single-sign-on/_static/image5.png)

Tiene automáticamente un inquilino de Azure AD para su cuenta de Azure, y puede hacer clic en el **agregar** situado en la parte inferior de la página para crear directorios adicionales. Es posible que desee una para un entorno de prueba y otra para producción, por ejemplo. Medite concienzudamente sobre qué nombre de un nuevo directorio. Si usa el nombre para el directorio y, a continuación, usa el nombre nuevo para uno de los usuarios, que puede resultar confuso.

![Agregar directorio](single-sign-on/_static/image6.png)

El portal tiene compatibilidad total para crear, eliminar y administrar usuarios dentro de este entorno. Por ejemplo, para agregar un usuario ir a la **usuarios** ficha y haga clic en el **Agregar usuario** botón.

![Botón Agregar usuario](single-sign-on/_static/image7.png)

![Agregar cuadro de diálogo usuario](single-sign-on/_static/image8.png)

Puede crear un nuevo usuario que existe solo en este directorio, o puede registrar una Account de Microsoft como un usuario en este directorio, o de registro o un usuario de otro directorio de Azure AD como un usuario en este directorio. (En un directorio real, el dominio predeterminado sería Contosoprueba.enmicrosoft.com. También puede usar un dominio de su elección, como contoso.com.)

![Tipos de usuario](single-sign-on/_static/image9.png)

![Agregar cuadro de diálogo usuario](single-sign-on/_static/image10.png)

Puede asignar al usuario a un rol.

![Perfil de usuario](single-sign-on/_static/image11.png)

Y se crea la cuenta con una contraseña temporal.

![Contraseña temporal](single-sign-on/_static/image12.png)

Los usuarios se crean de esta forma pueden iniciar inmediatamente sesión a las aplicaciones web mediante este directorio en la nube.

¿Qué es excelente para enterprise single sign-on, no obstante, es el **integración de directorios** pestaña:

![Pestaña Integración de directorio](single-sign-on/_static/image13.png)

Si habilita la integración de directorios, y [descargue una herramienta](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), puede sincronizar este directorio en la nube con su Active Directory local existente que ya está usando dentro de la organización. A continuación, todos los usuarios almacenados en el directorio se mostrarán en este directorio en la nube. Las aplicaciones de nube ahora pueden autenticar todos los empleados con sus credenciales de Active Directory existentes. Y todo esto es gratis, la herramienta de sincronización y Azure AD.

La herramienta es un asistente que es fácil de usar, como puede ver desde estas capturas de pantalla. Estos no son instrucciones completas, solo un ejemplo que muestra el proceso básico. Para obtener más información de how-to--it, vea los vínculos de la [recursos](#resources) sección al final del segmento.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image14.png)

Haga clic en **siguiente**y, a continuación, escriba sus credenciales de Azure Active Directory.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image15.png)

Haga clic en **siguiente**y, a continuación, escriba local las credenciales de AD.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image16.png)

Haga clic en **siguiente**y, a continuación, indique si desea almacenar un hash de las contraseñas de AD en la nube.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image17.png)

El hash de contraseña que se puede almacenar en la nube es un hash unidireccional; las contraseñas reales nunca se almacenan en Azure AD. Si decide frente a almacenar los valores hash en la nube, tendrá que usar [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). También hay [otros factores a tener en cuenta al elegir si desea usar ADFS o no](https://technet.microsoft.com/library/jj573653.aspx). La opción de ADFS requiere algunos pasos de configuración adicionales.

Si decide almacenar valores hash en la nube, que haya terminado y se inicia la herramienta sincronización de directorios al hacer clic en **siguiente**.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image18.png)

Y en cuestión de minutos que haya terminado.

![Asistente para configuración de herramienta de sincronización de WAAD](single-sign-on/_static/image19.png)

Solo tiene que ejecutar esto en un controlador de dominio de la organización, en Windows 2003 o posterior. Y no es necesario reiniciar. Cuando haya terminado, todos los usuarios están en la nube y que puede hacer un inicio de sesión único desde cualquier aplicación web o móvil, con SAML, OAuth o WS-Fed.

A veces nos preguntan acerca de cómo proteger se trata, Microsoft usarlo para sus propios datos empresariales confidenciales? Y la respuesta es sí que lo hacemos. Por ejemplo, si va al sitio de SharePoint de Microsoft interno en [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), se le pida que inicie sesión.

![Inicio de sesión en Office 365](single-sign-on/_static/image20.png)

Microsoft ha habilitado ADFS, por lo que al especificar un identificador de Microsoft, se le redirigirá a una página de inicio de sesión ADFS.

![Inicio de sesión de ADFS](single-sign-on/_static/image21.png)

Y una vez que escriba las credenciales almacenadas en una cuenta de Microsoft AD interna, tendrá acceso a esta aplicación interno.

![Sitio de SharePoint de MS](single-sign-on/_static/image22.png)

Vamos a usar un servidor de inicio de sesión de AD principalmente porque ya teníamos ADFS configurados antes de Azure AD, empezó a estar disponible, pero el proceso de registro se va a través de un directorio de Azure AD en la nube. Incluimos nuestro documentos importantes, control de código fuente, archivos de administración de rendimiento, informes de ventas y mucho más, en la nube y está usando esta misma solución exacta para protegerlos.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Crear una aplicación ASP.NET que usa Azure AD para inicio de sesión único

Visual Studio resulta muy fácil crear una aplicación que usa Azure AD para el inicio de sesión único, como puede ver en algunas capturas de pantalla.

Cuando se crea una nueva aplicación de ASP.NET MVC o Web Forms, el método de autenticación predeterminado es ASP.NET Identity. Para cambiar a Azure AD, hacer clic en un **Cambiar autenticación** botón.

![Cambiar autenticación](single-sign-on/_static/image23.png)

Seleccione las cuentas de organización, escriba el nombre de dominio y, a continuación, seleccione Inicio de sesión único.

![Configurar el cuadro de diálogo de autenticación](single-sign-on/_static/image24.png)

También puede dar a la aplicación de lectura o lectura/escritura permiso para los datos de directorio. Si lo hace, puede usar el [Graph API de REST de Azure](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) para buscar el número de teléfono de los usuarios, averiguar si está en la oficina, cuando registra por última vez, etcetera.

Eso es todo lo que debe hacer lo siguiente: Visual Studio solicita las credenciales de administrador para el inquilino de Azure AD y, a continuación, configura el proyecto y el inquilino de Azure AD para la nueva aplicación.

Al ejecutar el proyecto, verá una página de inicio de sesión y puede iniciar sesión con credenciales de un usuario en el directorio de Azure AD.

![Inicio de sesión de cuenta de organización](single-sign-on/_static/image25.png)

![Ha iniciado sesión](single-sign-on/_static/image26.png)

Al implementar la aplicación en Azure, lo único que debe hacer es seleccionar un **Habilitar autenticación organizativa** casilla de verificación y, una vez más Visual Studio se encarga de toda la configuración para usted.

![Publicación Web](single-sign-on/_static/image27.png)

Estas capturas de pantalla proceden de un tutorial paso a paso completo que muestra cómo compilar una aplicación que usa autenticación de Azure AD: [desarrollar aplicaciones de ASP.NET con Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Resumen

En este capítulo se explicaba que Azure Active Directory, Visual Studio y ASP.NET, facilitan la configuración de inicio de sesión único en aplicaciones de Internet para los usuarios de su organización. Los usuarios pueden iniciar sesión en aplicaciones de Internet con las mismas credenciales que se usan para iniciar sesión a través de Active Directory en su red interna.

El [siguiente capítulo](data-storage-options.md) examina las opciones de almacenamiento de datos disponibles para una aplicación de nube.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

- [Documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Página de portal de documentación de Azure AD en el sitio windowsazure.com. Para ver tutoriales paso a paso, consulte el **desarrollar** sección.
- [Microsoft Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Página del portal para obtener documentación sobre la autenticación multifactor en Azure.
- [Las opciones de autenticación de cuenta de organización](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explicación de las opciones de autenticación de Azure AD en el cuadro de diálogo del nuevo proyecto de Visual Studio 2013.
- [Microsoft Patterns and Practices - Federated Identity patrón](https://msdn.microsoft.com/library/dn589790.aspx).
- [Cómo: Instalar la herramienta de Azure Directory Sync Active](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 mapa de contenido](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Vínculos a documentación sobre AD FS 2.0.
- [Autorización basada en roles y basada en ACL en una aplicación de Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Aplicación de ejemplo.
- [Blog de Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Control de acceso en BYOD y la integración de directorios en una infraestructura de identidad híbrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 sesión vídeo Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Anterior](web-development-best-practices.md)
> [Siguiente](data-storage-options.md)
