---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Desarrollo de aplicaciones ASP.NET con Azure Active Directory | Documentos de Microsoft
author: Rick-Anderson
description: "Herramientas de Microsoft ASP.NET para Azure Active Directory simplifica el proceso de habilitar la autenticación para las aplicaciones web hospedadas en Azure. Puede usar Azure Authenti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 1ef0468d5f5c17480b23ac88983f30fe6f4979c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Desarrollo de aplicaciones ASP.NET con Azure Active Directory
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Herramientas de Microsoft ASP.NET para Azure Active Directory simplifica el proceso de habilitar la autenticación para las aplicaciones web hospedadas en [Azure](https://www.windowsazure.com/home/features/web-sites/). Puede utilizar autenticación de Azure para autenticar a los usuarios de Office 365 a su organización, las cuentas corporativas que se sincronizan desde su Active Directory local o los usuarios creados en su propio dominio personalizado de Azure Active Directory. Habilitar la autenticación de Windows Azure configura su aplicación para autenticar a los usuarios con una sola [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) inquilino.
> 
>  Este tutorial se escribió Rick Anderson[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


Este tutorial le mostrará cómo crear una aplicación de ASP.NET que está configurada para inicio de sesión con [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). También obtendrá información sobre cómo llamar a la API de Graph para obtener información sobre el usuario que inició sesión actualmente y cómo implementar la aplicación en Azure.

## <a name="prerequisites"></a>Requisitos previos

1. [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) o [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -se requiere actualización 3 o posterior.
3. Una cuenta de Azure. [Haga clic aquí](https://azure.microsoft.com/pricing/free-trial/) para una prueba gratuita si ya no dispone de una cuenta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Agregar un administrador Global en Active Directory

1. Inicie sesión en el [Portal de administración de Azure](https://manage.windowsazure.com/).
2. Todas las cuentas de Azure contienen un **directorio predeterminado** : haga clic en él, haga clic en el **a los usuarios** ficha en la parte superior de la página (consulte la imagen siguiente).
3. Haga clic en Agregar usuario.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Crear un nuevo usuario con el **administrador Global** rol. Haga clic en **usuarios** desde el menú superior y, a continuación, haga clic en el **Agregar usuario** botón en la barra de comandos.
5. En el **Agregar usuario** cuadro de diálogo, escriba un nombre para el nuevo usuario y, a continuación, haga clic en la flecha derecha.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Escriba el nombre de usuario y establezca el rol en **administrador Global**. Los administradores globales requieren una dirección de correo electrónico alternativa para fines de recuperación de contraseña. Cuando haya terminado, haga clic en la flecha derecha.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. En la siguiente página del cuadro de diálogo, haga clic en **crear**. Una contraseña temporal se crea para el nuevo usuario y se mostrará en el cuadro de diálogo.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
 Guardar la contraseña, será necesario cambiar la contraseña tras el primer registro en. La siguiente imagen muestra la nueva cuenta de administrador. Debe usar Azure Active Directory para iniciar sesión en la aplicación, no la cuenta de Microsoft también se muestran en esta página.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Crear una aplicación ASP.NET

Los siguientes pasos se usa [Visual Studio Express 2013 para Web](https://www.microsoft.com/download/details.aspx?id=40747)y requiere [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. En Visual Studio, haga clic en **archivo** y, a continuación, **nuevo proyecto**. En el **nuevo proyecto** cuadro de diálogo, seleccione la Web de Visual C# del proyecto en el menú izquierdo y haga clic en **Aceptar**. También puede desactivar la **agregar Application Insights al proyecto** si no desea la funcionalidad de la aplicación.
2. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione **MVC**y, a continuación, haga clic en **Cambiar autenticación**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas organizativas**. Estas opciones pueden utilizarse para registrar automáticamente la aplicación con Azure AD así como configurar automáticamente su aplicación para la integración con Azure AD. No tiene que usar el **Cambiar autenticación** cuadro de diálogo para registrar y configurar la aplicación, pero resulta mucho más fácil. Si usas Visual Studio 2012 por ejemplo, puede registrar la aplicación en el Portal de administración de Azure manualmente y actualizar su configuración para integrar con Azure AD.  
 En los menús de lista desplegable, seleccione **Cloud - única organización** y **inicio de sesión único, leer datos de directorio**. Escriba el dominio de su directorio de Azure AD, por ejemplo (en las siguientes imágenes) *aricka0yahoo.onmicrosoft.com*y, a continuación, haga clic en **Aceptar**. Puede obtener el nombre de dominio de la ficha dominios para el directorio predeterminado en el portal de azure (vea la siguiente imagen hacia abajo).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
 La siguiente imagen muestra el nombre de dominio desde el portal de Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Opcionalmente, puede configurar el URI de Id. de aplicación que se registrará en Azure AD, haga clic en **más opciones**. El URI de Id. de aplicación es el identificador único para una aplicación, que está registrado en Azure AD y utilizado por la aplicación para identificarse al comunicar con Azure AD. Para obtener más información sobre el URI de Id. de aplicación y otras propiedades de las aplicaciones registradas, consulte [en este tema](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Haciendo clic en la casilla de verificación debajo del campo de URI de Id. de aplicación, también puede elegir sobrescribir un registro existente en Azure AD que usa el mismo URI de Id. de aplicación.
4. Después de hacer clic **Aceptar**, aparecerá un cuadro de diálogo Inicio de sesión y deberá iniciar sesión con una cuenta de administrador Global (no la cuenta Microsoft asociada con su suscripción). Si ha creado una nueva cuenta de administrador de versiones anteriores, le pedirá que cambie la contraseña y vuelva a iniciarla de nuevo con la nueva contraseña.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Una vez haya autenticado correctamente, el **nuevo proyecto ASP.NET** cuadro de diálogo mostrará la opción de autenticación (**organización** ) y el directorio donde será la nueva aplicación registrada (*aricka0yahoo.onmicrosoft.com* en la imagen siguiente). Debajo de esta información, seleccione la casilla denominada **Host en la nube**. Si se activa esta casilla, el proyecto se aprovisionará como una aplicación web de Azure y se habilitarán para publicar fácilmente más adelante. Haga clic en **Aceptar**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. El **configurar el sitio Web Azure** aparecerá cuadro de diálogo con un nombre del sitio generados automáticamente y la región. Tenga en cuenta también la cuenta que ha iniciado sesión actualmente en el cuadro de diálogo. Desea asegurarse de que esta cuenta es el que se adjunta la suscripción de Azure, normalmente una cuenta de Microsoft.

    > [!NOTE]
    > Este proyecto requiere una base de datos. Debe seleccionar una de las bases de datos existentes o cree uno nuevo. Una base de datos es necesario porque el proyecto ya utiliza un archivo de base de datos local para almacenar una pequeña cantidad de datos de configuración de autenticación. Al implementar la aplicación en un sitio Web de Azure, esta base de datos no empaquetado con la implementación, por ello, debe elegir una que sea accesible en la nube. Haga clic en **Aceptar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Se creará el proyecto y las opciones de autenticación y opciones de aplicación web se configurarán automáticamente con el proyecto. Cuando haya completado este proceso, ejecute el proyecto localmente presionando **^ F5**. Se le pedirá que inicie sesión con su cuenta profesional. Proporcione el nombre de usuario y la contraseña de la cuenta que creó anteriormente y haga clic en **iniciar sesión en**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Después de haber inicio de sesión correcto, mostrará el sitio de ASP.NET que se ha autenticado al mostrar el nombre de usuario en la esquina superior derecha de la página.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
 Si se produce un error:  
 Valor no puede ser nulo ni estar vacío. Nombre del parámetro: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
 Consulte la [depurar](#dbg) sección al final del tutorial.

## <a name="basics-of-the-graph-api"></a>Conceptos básicos de la API Graph

[La API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) es la interfaz de programación que se utilizan para realizar CRUD y otras operaciones en objetos en el directorio de Azure AD. Si selecciona una opción de cuenta de la organización para la autenticación cuando se crea un nuevo proyecto en Visual Studio 2013, la aplicación ya se configurará para llamar a la API Graph. Brevemente en esta sección muestra cómo funciona la API Graph.

1. En la aplicación en ejecución, haga clic en el nombre del usuario con sesión iniciada en la parte superior derecha de la página. Esto le llevará a la página de perfil de usuario, que es una acción en el controlador Home. Observará que la tabla contiene información de usuario acerca de la cuenta de administrador que creó anteriormente. Esta información se almacena en el directorio y llama a la API de Graph para recuperar esta información cuando se carga la página.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Vuelva a Visual Studio y expanda el **controladores** carpeta y vuelva a abrir la **HomeController.cs** archivo. Verá un **UserProfile()** acción que contiene código para recuperar un token y, a continuación, llamar a la API de Graph. Este código se duplica a continuación: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Para llamar a la API Graph, primero debe recuperar un token. Cuando se recupera el token, su valor de cadena debe agregarse en el encabezado de autorización para todas las solicitudes posteriores a la API Graph. La mayoría del código anterior controla los detalles de autenticarse en Azure AD para obtener un token, con el token para realizar una llamada a la API Graph y, a continuación, transformar la respuesta para que se pueden presentar en la vista.

    La parte más relevante para el análisis es la siguiente línea resaltada: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Esta línea representa el nombre del usuario, que se ha deserializado desde la respuesta JSON y se presenta en la vista.

    Se puede llamar a la API de Graph mediante HttpClient y controlar los datos sin procesar por sí mismo, pero una manera más fácil es usar el [biblioteca de cliente de Graph que está disponible a través de NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La biblioteca de cliente controla las solicitudes HTTP sin formato y la transformación de los datos devueltos por usted y, resulta más fácil trabajar con la API de Graph en un entorno. NET. Vea los ejemplos de código de API de gráficos relacionados en [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Implementar la aplicación en Azure

Los siguientes pasos le enseñará a implementar la aplicación en Azure. En los pasos anteriores, ha conectado el nuevo proyecto con una aplicación web en Azure, por lo que está listo para publicarse en unos pocos pasos.

1. En Visual Studio, haga doble clic en el proyecto y seleccione **publicar**. El **Publicar Web** aparecerá el cuadro de diálogo con cada opción ya configurada. Haga clic en el **siguiente** botón para ir a la **configuración** página. Es podrán que deba autenticarse; Asegúrese de que se autentica con su cuenta de suscripción de Azure (normalmente una cuenta de Microsoft) y no la cuenta organizativa que creó anteriormente.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Compruebe el **Habilitar autenticación organizativa** opción. En el **dominio** , escriba el dominio de su directorio. Desde el **nivel de acceso** lista desplegable, seleccione **inicio de sesión único, leer datos de directorio**. Observará que ya se ha rellenado la base de datos anterior que usó en el **bases de datos** sección. Haga clic en **Publicar**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio empezará la implementación de su sitio Web y, a continuación, aparecerá una nueva ventana del explorador. Puede que deba autenticarse de nuevo a su directorio. Una vez que se ha autenticado, se le redirigirá al sitio Web recién publicado en Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Para depurar la aplicación

Si se produce el error siguiente:   
 Valor no puede ser nulo ni estar vacío. Nombre del parámetro: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Reemplace el código de la *Views\Shared\\_LoginPartial.cshtml* archivo con lo siguiente:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Después de ejecutar la aplicación, si "Usuario Null" muestra en la sesión del usuario, la sesión e iniciar sesión con la cuenta de Active Directory que creó anteriormente.

Un excelente tutorial seguir es de Rick Rainey [profundización: sitios Web de Azure y organizativa autenticación con Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Más información

- [Profundización: Sitios Web de Azure y organizativa autenticación con Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Introducción a la API Azure AD Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Escenarios de autenticación en Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Ejemplos de código de Azure AD en GitHub](https://github.com/AzureADSamples)
