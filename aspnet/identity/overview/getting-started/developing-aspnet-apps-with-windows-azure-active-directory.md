---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Desarrollo de aplicaciones ASP.NET con Azure Active Directory | Microsoft Docs
author: Rick-Anderson
description: Herramientas de Microsoft ASP.NET para Azure Active Directory simplifica habilitar la autenticación para aplicaciones web hospedadas en Azure. Puede usar Azure Authenti...
ms.author: aspnetcontent
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: bb12ed6c13105cf08412ecf969c215cc9004a0c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809078"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Desarrollo de aplicaciones ASP.NET con Azure Active Directory
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

Herramientas de Microsoft ASP.NET para Azure Active Directory simplifica al habilitar la autenticación para aplicaciones web hospedadas en [Azure](https://www.windowsazure.com/home/features/web-sites/). Puede usar la autenticación de Azure para autenticar a los usuarios de Office 365 de su organización, las cuentas corporativas sincronizadas desde Active Directory local o los usuarios creados en su propio dominio personalizado de Azure Active Directory. Habilitar la autenticación de Windows Azure configura la aplicación para autenticar usuarios mediante una sola [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) inquilino.

Este tutorial le mostrará cómo crear una aplicación ASP.NET que está configurada para inicio de sesión con [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). También obtendrá información sobre cómo llamar a Graph API para obtener información sobre el usuario ha iniciado sesión actualmente y cómo implementar la aplicación en Azure.

## <a name="prerequisites"></a>Requisitos previos

1. [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) o [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -se requiere actualización 3 o posterior.
3. Una cuenta de Azure. [Haga clic aquí](https://azure.microsoft.com/pricing/free-trial/) para una evaluación gratuita si ya no tiene una cuenta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Agregar un administrador Global en Active Directory

1. Inicie sesión en el [Portal de administración de Azure](https://manage.windowsazure.com/).
2. Todas las cuentas de Azure contienen un **directorio predeterminado** : haga clic en él, a continuación, haga clic en el **usuarios** ficha en la parte superior de la página (consulte la imagen siguiente).
3. Haga clic en Agregar usuario.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Crear un nuevo usuario con el **administrador Global** rol. Haga clic en **usuarios** en el menú superior y, a continuación, haga clic en el **Agregar usuario** botón en la barra de comandos.
5. En el **Agregar usuario** cuadro de diálogo, escriba un nombre para el nuevo usuario y, a continuación, haga clic en la flecha derecha.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Escriba el nombre de usuario y establezca el rol en **administrador Global**. Los administradores globales requieren una dirección de correo electrónico alternativa para fines de recuperación de contraseña. Cuando haya terminado, haga clic en la flecha derecha.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. En la siguiente página del cuadro de diálogo, haga clic en **crear**. Se creó para el nuevo usuario una contraseña temporal y se mostrará en el cuadro de diálogo.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Guardar la contraseña, se le pedirá que cambie la contraseña tras el primer registro en. La siguiente imagen muestra la nueva cuenta de administrador. Debe usar Azure Active Directory para iniciar sesión en su aplicación, no la cuenta de Microsoft también se muestran en esta página.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Crear una aplicación ASP.NET

Los pasos siguientes usan [Visual Studio Express 2013 para Web](https://www.microsoft.com/download/details.aspx?id=40747)y requiere [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. En Visual Studio, haga clic en **archivo** y, a continuación, **nuevo proyecto**. En el **nuevo proyecto** cuadro de diálogo, seleccione la Web de Visual C# del proyecto en el menú izquierdo y haga clic en **Aceptar**. También puede desactivar la **agregar Application Insights al proyecto** si no desea la funcionalidad de la aplicación.
2. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione **MVC**y, a continuación, haga clic en **Cambiar autenticación**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas organizativas**. Estas opciones pueden utilizarse para registrar automáticamente la aplicación con Azure AD, además de configurar automáticamente su aplicación para la integración con Azure AD. No tiene que usar el **Cambiar autenticación** cuadro de diálogo para registrar y configurar la aplicación, pero facilita en gran medida. Si utiliza Visual Studio 2012 por ejemplo, puede registrar la aplicación en el Portal de administración de Azure manualmente y actualizar su configuración para integrar con Azure AD.  
   En los menús de lista desplegable, seleccione **nube: organización única** y **inicio de sesión único, leer datos de directorio**. Escriba el dominio de su directorio de Azure AD, por ejemplo (en las imágenes siguientes) *aricka0yahoo.onmicrosoft.com*y, a continuación, haga clic en **Aceptar**. Puede obtener el nombre de dominio desde la ficha dominios para el directorio predeterminado en el portal de azure (consulte la siguiente imagen abajo).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   La siguiente imagen muestra el nombre de dominio desde el portal de Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Opcionalmente, puede configurar el URI de Id. de aplicación que se registrará en Azure AD, haga clic en **más opciones**. El URI de Id. de aplicación es el identificador único para una aplicación, que se registra en Azure AD y utilizado por la aplicación para identificarse al comunicarse con Azure AD. Para obtener más información sobre el URI de Id. de aplicación y otras propiedades de las aplicaciones registradas, consulte [en este tema](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Al hacer clic en la casilla de verificación debajo del campo de URI de Id. de aplicación, también puede sobrescribir un registro existente en Azure AD que usa el mismo URI de Id. de aplicación.
4. Después de hacer clic **Aceptar**, aparecerá un cuadro de diálogo Inicio de sesión y deberá iniciar sesión con una cuenta de administrador Global (no la cuenta Microsoft asociada con su suscripción). Si anteriormente creó una nueva cuenta de administrador, le pedirá que cambie la contraseña y, a continuación, iníciela con la nueva contraseña.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Una vez se haya autenticado correctamente, el **nuevo proyecto ASP.NET** cuadro de diálogo mostrará la opción de autenticación (**organizativo** ) y el directorio donde será la nueva aplicación registrada (*aricka0yahoo.onmicrosoft.com* en la imagen siguiente). Debajo de esta información, seleccione la casilla **Host en la nube**. Si se activa esta casilla, el proyecto se aprovisionará como una aplicación web de Azure y se habilitarán para la publicación fácil más adelante. Haga clic en **Aceptar**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. El **configurar el sitio Web Azure** aparecerá cuadro de diálogo con un nombre de sitio generado automáticamente y región. Tenga en cuenta también la cuenta que ha iniciado sesión actualmente en el cuadro de diálogo. Desea asegurarse de que esta cuenta es aquella que está conectada su suscripción de Azure, normalmente una cuenta de Microsoft.

    > [!NOTE]
    > Este proyecto requiere una base de datos. Deberá seleccionar una de las bases de datos existentes o crear uno nuevo. Una base de datos es necesario porque el proyecto ya usa un archivo de base de datos local para almacenar una pequeña cantidad de datos de configuración de autenticación. Al implementar la aplicación en un sitio Web de Azure, esta base de datos no se empaqueta con la implementación, por lo que deberá elegir una que sea accesible en la nube. Haga clic en **Aceptar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Se creará el proyecto y las opciones de autenticación y las opciones de aplicación web se configurarán automáticamente con el proyecto. Una vez que haya completado este proceso, ejecute el proyecto localmente presionando **^ F5**. Se le pedirá que inicie sesión con su cuenta profesional. Proporcione el nombre de usuario y la contraseña de la cuenta que creó anteriormente y haga clic en **inicie sesión en**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Después de iniciar sesión correctamente, mostrará el sitio de ASP.NET que se ha autenticado al mostrar el nombre de usuario en la esquina superior derecha de la página.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Si se produce un error:  
   Valor no puede ser nulo ni estar vacío. Nombre de parámetro: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   Consulte la [depurar](#dbg) sección al final del tutorial.

## <a name="basics-of-the-graph-api"></a>Aspectos básicos de la API Graph

[La API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) es la interfaz de programación utilizada para realizar CRUD y otras operaciones en objetos de directorio de Azure AD. Si selecciona una opción de cuenta de organización para la autenticación al crear un nuevo proyecto en Visual Studio 2013, la aplicación ya se configurarán para llamar a Graph API. Brevemente, esta sección muestra cómo funciona la API Graph.

1. En la aplicación en ejecución, haga clic en el nombre del usuario con sesión iniciada en la parte superior derecha de la página. Esto le llevará a la página de perfil de usuario, que es una acción en el controlador Home. Observará que la tabla contiene información de usuario acerca de la cuenta de administrador que creó anteriormente. Esta información se almacena en el directorio y se llama a la API Graph para recuperar esta información cuando se carga la página.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Vuelva a Visual Studio y expanda el **controladores** carpeta y, a continuación, abra el **HomeController.cs** archivo. Verá un **UserProfile()** acción que contiene código para recuperar un token y, a continuación, llamar a Graph API. Este código se duplica a continuación: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Para llamar a Graph API, primero deberá recuperar un token. Cuando se recupera el token, se debe anexar a su valor de cadena en el encabezado de autorización para todas las solicitudes posteriores a la API Graph. La mayoría del código anterior controla los detalles de autenticarse en Azure AD para obtener un token, con el token para realizar una llamada a Graph API y, a continuación, transformar la respuesta para que se pueden presentar en la vista.

    La parte más relevante para la discusión es la siguiente línea resaltada: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Esta línea representa el nombre del usuario, que se ha deserializado desde la respuesta JSON y se presenta en la vista.

    Se puede llamar a Graph API mediante HttpClient y controlar los datos sin procesar por sí mismo, pero una manera más fácil es usar el [biblioteca de cliente de Graph que está disponible a través de NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La biblioteca cliente controla las solicitudes HTTP sin formato y la transformación de los datos devueltos por usted y hace que sea mucho más fácil trabajar con la API Graph en un entorno. NET. Consulte los ejemplos de código relacionados de Graph API de [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Implementar la aplicación en Azure

Los pasos siguientes le mostrará cómo implementar la aplicación en Azure. En los pasos anteriores, se ha conectado el nuevo proyecto con una aplicación web en Azure, para que esté listo para publicarse en unos pocos pasos.

1. En Visual Studio, haga doble clic en el proyecto y seleccione **publicar**. El **publicación Web** aparecerá el cuadro de diálogo con cada configuración que ya ha configurado. Haga clic en el **siguiente** botón para ir a la **configuración** página. Se le pedirá autenticarse; Asegúrese de que se autentique con su cuenta de suscripción de Azure (normalmente una cuenta de Microsoft) y no la cuenta de organización que creó anteriormente.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Compruebe el **Habilitar autenticación organizativa** opción. En el **dominio** , escriba el dominio para su directorio. Desde el **nivel de acceso** lista desplegable, seleccione **inicio de sesión único, leer datos de directorio**. Observará que la anterior base de datos que ya se rellena en el **bases de datos** sección. Haga clic en **Publicar**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio empezará a implementar su sitio Web y, a continuación, aparecerá una nueva ventana del explorador. Se le pedirá autenticarse una vez más a su directorio. Una vez que se ha autenticado, se le redirigirá al sitio Web recién publicado en Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depuración de la aplicación

Si recibe el error siguiente:   
 Valor no puede ser nulo ni estar vacío. Nombre de parámetro: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Reemplace el código en el *Views\Shared\\_LoginPartial.cshtml* archivo por lo siguiente:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Después de ejecutar la aplicación, si "Usuario Null" muestra en el usuario que ha iniciado sesión, cierre la sesión y vuelva a iniciarla con la cuenta de Active Directory que creó anteriormente.

Un excelente tutorial seguir es de Rick Rainey [Deep Dive: Azure Websites y autenticación de organización con Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Más información

- [Profundización: Sitios Web de Azure y autenticación de organización con Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Introducción a la API de Azure AD Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Escenarios de autenticación de Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Ejemplos de código de Azure AD en GitHub](https://github.com/AzureADSamples)
