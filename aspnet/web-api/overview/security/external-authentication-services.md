---
uid: web-api/overview/security/external-authentication-services
title: Servicios de autenticación externo con ASP.NET Web API (C#) | Documentos de Microsoft
author: rmcmurray
description: Describe cómo utilizar los servicios de autenticación externos en ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 406a85db7055910cb7a4e15fec8ef68dff5a19dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Servicios de autenticación externo con ASP.NET Web API (C#)
====================
por [Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 y ASP.NET 4.5.1 expanden las opciones de seguridad para [aplicaciones de una única página](../../../single-page-application/index.md) (contraseña segura SPA) y [API Web](../../index.md) services para integrarse con los servicios de autenticación externo, que incluyen varios Servicios de autenticación de redes sociales y OAuth/OpenID: Google, Facebook, Twitter y Accounts de Microsoft.

### <a name="in-this-walkthrough"></a>En este tutorial

- [Uso de servicios de autenticación externos](#USING)
- [Crear la aplicación Web](#SAMPLE)
- [Habilitar la autenticación de Facebook](#FACEBOOK)
- [Habilitar la autenticación de Google](#GOOGLE)
- [Habilitar la autenticación de Microsoft](#MICROSOFT)
- [Habilitar la autenticación de Twitter](#TWITTER)
- [Información adicional](#MOREINFO)

    - [Combinación de los servicios de autenticación externo](#COMBINE)
    - [Configuración de IIS Express debe usar un nombre de dominio completo](#FQDN)
    - [Cómo obtener la configuración de la aplicación para la autenticación de Microsoft](#OBTAIN)
    - [Opcional: Deshabilitar el registro Local](#DISABLE)

### <a name="prerequisites"></a>Requisitos previos

Para seguir los ejemplos de este tutorial, necesita lo siguiente:

- Visual Studio 2013
- Una cuenta para al menos uno de los siguientes servicios de autenticación externos:

    - Una cuenta de usuario de Google
    - Una cuenta de desarrollador con el identificador de la aplicación y la clave secreta para uno de los siguientes servicios de autenticación de redes sociales:

        - Cuentas de Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Uso de servicios de autenticación externos

La gran cantidad de servicios de autenticación externos que están actualmente disponibles a la Ayuda de los desarrolladores web para reducir el desarrollo de tiempo al crear nuevas aplicaciones web. Los usuarios de Web suelen tengan varias cuentas existentes para servicios web populares y sitios Web de redes sociales, por lo tanto, cuando se implementa de aplicación de web de servicios de la autenticación de un servicio web externo o un sitio Web de redes sociales, guarda el tiempo de desarrollo que habría ha empleado en la creación de una implementación de autenticación. Uso de un servicio de autenticación externo guarda los usuarios finales de tener que crear otra cuenta para la aplicación web, además de tener que recordar otro nombre de usuario y contraseña.

En el pasado, los programadores han tenido dos opciones: crear su propia implementación de la autenticación y obtener información sobre cómo integrar un servicio de autenticación externo en sus aplicaciones. En el nivel más básico, el siguiente diagrama muestra un flujo de solicitud simple para un agente de usuario (explorador web) que está solicitando información desde una aplicación web que está configurada para usar un servicio de autenticación externos:

[![](external-authentication-services/_static/image2.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image1.png)

En el diagrama anterior, el agente de usuario (o el Explorador de web en este ejemplo) realiza una solicitud a una aplicación web, que redirige el explorador web a un servicio de autenticación externo. El agente de usuario envía sus credenciales para el servicio de autenticación externo, y si el agente de usuario se autentica correctamente, el servicio de autenticación externo redirigirá el agente de usuario a la aplicación web original con algún tipo de símbolo (token) que el agente de usuario enviará a la aplicación web. La aplicación web usará el token para comprobar que el agente de usuario se ha autenticado correctamente por el servicio de autenticación externo y la aplicación web puede usar el token para obtener más información sobre el agente de usuario. Una vez que se realiza la aplicación de procesamiento de información del agente de usuario, la aplicación web devolverá la respuesta adecuada para el agente de usuario en función de su configuración de autorización.

En este segundo ejemplo, el agente de usuario negocia con la aplicación web y el servidor de autorización externas y la aplicación web lleva a cabo la comunicación con el servidor de autorización externas para recuperar información adicional sobre el usuario adicionales agente:

[![](external-authentication-services/_static/image4.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image3.png)

Visual Studio 2013 y ASP.NET 4.5.1 facilitan la integración con servicios de autenticación externos para desarrolladores proporcionando integración incorporada para los servicios de autenticación siguientes:

- Facebook
- Google
- Microsoft Accounts (Windows Live ID)
- Twitter

Los ejemplos de este tutorial se muestran cómo configurar cada uno de los servicios de autenticación externo compatible con la nueva plantilla de aplicación Web ASP.NET que se distribuye con Visual Studio 2013.

> [!NOTE]
> Si es necesario, debe agregar el FQDN a la configuración para el servicio de autenticación externo. Este requisito se basa en las restricciones de seguridad para algunos servicios de autenticación externos que requieren el FQDN en la configuración de la aplicación para que coincida con el FQDN que es utilizado por los clientes. (Los pasos de este varían en gran medida para cada servicio de autenticación externo; lo necesitará consultar la documentación para cada servicio de autenticación externo para ver si esto es necesario y cómo configurar estas opciones.) Si necesita configurar IIS Express se utiliza un FQDN para este entorno de pruebas, consulte el [configurar IIS Express debe usar un nombre de dominio completo](#FQDN) sección más adelante en este tutorial.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Crear una aplicación Web de ejemplo

Los siguientes pasos le llevará por la creación de una aplicación de ejemplo mediante la plantilla de aplicación Web ASP.NET y va a usar esta aplicación de ejemplo para cada uno de los servicios de autenticación externo más adelante en este tutorial.

Inicie Visual Studio 2013 seleccione **nuevo proyecto** desde la página de inicio. O bien, en la **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

[![](external-authentication-services/_static/image6.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image5.png)

Cuando el **nuevo proyecto** aparece el cuadro de diálogo, seleccione **instalado** **plantillas** y expanda **Visual C#**. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web ASP.NET**. Escriba un nombre para el proyecto y haga clic en **Aceptar**.

[![](external-authentication-services/_static/image8.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image7.png)

Cuando el **nuevo proyecto ASP.NET** aparece, seleccione la **SPA** plantilla y haga clic en **crear proyecto**.

[![](external-authentication-services/_static/image10.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image9.png)

Espere a que Visual Studio 2013 crea el proyecto.

[![](external-authentication-services/_static/image12.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image11.png)

Cuando Visual Studio 2013 ha terminado de crear el proyecto, abra el *Startup.Auth.cs* archivo que se encuentra en la **aplicación\_iniciar** carpeta.

[![](external-authentication-services/_static/image14.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image13.png)

Cuando se crea el proyecto por primera vez, ninguno de los servicios de autenticación externos están habilitado en *Startup.Auth.cs* archivo; muestra lo que el código puede ser similar a lo siguiente, con las secciones resaltadas de dónde permitiría un servicio de autenticación externo y los valores de configuración relevantes para utilizar la autenticación de Microsoft Accounts, Twitter, Facebook o Google con la aplicación de ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Cuando se presiona F5 para compilar y depurar la aplicación web, mostrará una pantalla de inicio de sesión donde podrá ver que no se han definido ningún servicio de autenticación externo.

[![](external-authentication-services/_static/image16.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image15.png)

En las siguientes secciones, obtendrá información sobre cómo habilitar cada uno de los servicios de autenticación externo que se proporcionan con ASP.NET en Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Habilitar la autenticación de Facebook

Con Facebook autenticación requiere crear una cuenta de desarrollador de Facebook y el proyecto requerirá un Id. de aplicación y la clave secreta de Facebook para poder funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Facebook y obtener el identificador de la aplicación y la clave secreta, vea [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Una vez ha obtenido el identificador de la aplicación y la clave secreta, siga estos pasos para habilitar la autenticación de Facebook para la aplicación web:

1. Cuando el proyecto está abierto en Visual Studio 2013, abra el *Startup.Auth.cs* archivo:

    [![](external-authentication-services/_static/image18.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image17.png)
2. Busque la sección resaltada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Quitar el &quot; // &quot; caracteres que se quite el comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de la aplicación y la clave secreta. Cuando haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Cuando se presiona F5 para abrir la aplicación web en el explorador web, verá que Facebook se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image20.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image19.png)
5. Al hacer clic en el **Facebook** botón, el explorador se redirigirá a la página de inicio de sesión de Facebook:

    [![](external-authentication-services/_static/image22.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image21.png)
6. Después de escribir sus credenciales de Facebook y haga clic en **sesión**, el explorador web se redirigirán a la aplicación web, que le solicitará la **nombre de usuario** que desea asociar a su Cuenta de Facebook:

    [![](external-authentication-services/_static/image24.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image23.png)
7. Después de haber escrito el nombre de usuario y hace clic en el **registrarse** botón, la aplicación web mostrará el valor predeterminado **página principal** para su cuenta de Facebook:

    [![](external-authentication-services/_static/image26.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Habilitar la autenticación de Google

Google es con diferencia el más fácil de los servicios de autenticación externo habilitación, ya que no requiere una cuenta de desarrollador, ni requiere información adicional como el identificador de la aplicación o la clave secreta, como los demás servicios de autenticación externo precisan.

Para habilitar la autenticación de Google para la aplicación web, siga estos pasos:

1. Cuando el proyecto está abierto en Visual Studio 2013, abra el *Startup.Auth.cs* archivo:

    [![](external-authentication-services/_static/image28.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image27.png)
2. Busque la sección resaltada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Quitar el &quot; // &quot; caracteres que se quite el comentario de la línea de código resaltada y, a continuación, vuelva a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Cuando se presiona F5 para abrir la aplicación web en el explorador web, verá que Google se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image30.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image29.png)
5. Al hacer clic en el **Google** botón, el explorador se redirigirá a la página de inicio de sesión de Google:

    [![](external-authentication-services/_static/image32.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image31.png)
6. Después de escribir sus credenciales de Google y haga clic en **iniciar sesión en**, Google le pedirá que compruebe que la aplicación web tiene permisos para acceder a su cuenta de Google:

    [![](external-authentication-services/_static/image34.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image33.png)
7. Al hacer clic en **Accept**, el explorador web se redirigirán a la aplicación web, que le solicitará la **nombre de usuario** que desea asociar a su cuenta de Google:

    [![](external-authentication-services/_static/image36.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image35.png)
8. Después de haber escrito el nombre de usuario y hace clic en el **registrarse** botón, la aplicación web mostrará el valor predeterminado **página principal** para su cuenta de Google:

    [![](external-authentication-services/_static/image38.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Habilitar la autenticación de Microsoft

Autenticación de Microsoft, tendrá que crear una cuenta de desarrollador y requiere un Id. de cliente y el secreto del cliente para poder funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Microsoft y obtener el Id. de cliente y el secreto de cliente, consulte [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Una vez obtenida la clave de consumidor y el secreto del consumidor, siga estos pasos para habilitar la autenticación de Microsoft para la aplicación web:

1. Cuando el proyecto está abierto en Visual Studio 2013, abra el *Startup.Auth.cs* archivo:

    [![](external-authentication-services/_static/image40.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image39.png)
2. Busque la sección resaltada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Quitar el &quot; // &quot; caracteres que se quite el comentario de las líneas de código resaltadas y, a continuación, agregue el identificador de cliente y el secreto de cliente. Cuando haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Cuando se presiona F5 para abrir la aplicación web en el explorador web, verá que Microsoft se ha definido como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image42.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image41.png)
5. Al hacer clic en el **Microsoft** botón, el explorador se redirigirá a la página de inicio de sesión de Microsoft:

    [![](external-authentication-services/_static/image44.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image43.png)
6. Después de escribir sus credenciales de Microsoft y haga clic en **iniciar sesión en**, se le pedirá para comprobar que la aplicación web tiene permisos para acceder a su cuenta de Microsoft:

    [![](external-authentication-services/_static/image46.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image45.png)
7. Al hacer clic en **Sí**, el explorador web se redirigirán a la aplicación web, que le solicitará la **nombre de usuario** que desea asociar a su cuenta de Microsoft:

    [![](external-authentication-services/_static/image48.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image47.png)
8. Después de haber escrito el nombre de usuario y hace clic en el **registrarse** botón, la aplicación web mostrará el valor predeterminado **página principal** para su cuenta de Microsoft:

    [![](external-authentication-services/_static/image50.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Habilitar la autenticación de Twitter

Twitter autenticación requiere crear una cuenta de desarrollador, y una clave de consumidor y el secreto de consumidor para poder funcionar. Para obtener información sobre cómo crear una cuenta de desarrollador de Twitter y obtener su clave de consumidor y el secreto del consumidor, consulte [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Una vez obtenida la clave de consumidor y el secreto del consumidor, siga estos pasos para habilitar la autenticación de Twitter para la aplicación web:

1. Cuando el proyecto está abierto en Visual Studio 2013, abra el *Startup.Auth.cs* archivo:

    [![](external-authentication-services/_static/image52.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image51.png)
2. Busque la sección resaltada de código:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Quitar el &quot; // &quot; caracteres que se quite el comentario de las líneas de código resaltadas y, a continuación, agregue su clave de consumidor y el secreto del consumidor. Cuando haya agregado esos parámetros, puede volver a compilar el proyecto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Cuando se presiona F5 para abrir la aplicación web en el explorador web, verá que se ha definido Twitter como un servicio de autenticación externo:

    [![](external-authentication-services/_static/image54.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image53.png)
5. Al hacer clic en el **Twitter** botón, el explorador se redirigirá a la página de inicio de sesión de Twitter:

    [![](external-authentication-services/_static/image56.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image55.png)
6. Después de escribir sus credenciales de Twitter y haga clic en **autorizar aplicaciones**, el explorador web se redirigirán a la aplicación web, que le solicitará la **nombre de usuario** que desea asociar con su cuenta de Twitter:

    [![](external-authentication-services/_static/image58.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image57.png)
7. Después de haber escrito el nombre de usuario y hace clic en el **registrarse** botón, la aplicación web mostrará el valor predeterminado **página principal** para su cuenta de Twitter:

    [![](external-authentication-services/_static/image60.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Información adicional

Para obtener información adicional acerca de cómo crear aplicaciones que usan OAuth y OpenID, consulte las siguientes direcciones URL:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Combinación de los servicios de autenticación externo

Para una mayor flexibilidad, puede definir varios servicios de autenticación externos al mismo tiempo: Esto permite que su sitio web a los usuarios de la aplicación usar una cuenta de cualquiera de los servicios de autenticación externo habilitado:

[![](external-authentication-services/_static/image62.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Configuración de IIS Express debe usar un nombre de dominio completo

Algunos proveedores de autenticación externa no admiten probar la aplicación con una dirección HTTP como `http://localhost:port/`. Para solucionar este problema, puede agregar una asignación de nombre de dominio completo (FQDN) estática para el archivo de HOSTS y configurar las opciones de proyecto en Visual Studio 2013 para usar el FQDN para probar y depurar. Para ello, siga estos pasos:

- Agregue un FQDN estático asignar el archivo de HOSTS:

  1. Abra un símbolo del sistema con privilegios elevados en Windows.
  2. Escriba el comando siguiente:

      <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Agregue una entrada similar al siguiente al archivo de HOSTS:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Guarde y cierre el archivo de HOSTS.

- Configurar el proyecto de Visual Studio para que usen el FQDN:

  1. Cuando el proyecto está abierto en Visual Studio 2013, haga clic en el **proyecto** menú y, a continuación, seleccione las propiedades del proyecto. Por ejemplo, puede seleccionar **WebApplication1 propiedades**.
  2. Seleccione el **Web** ficha.
  3. Escriba el FQDN para el <strong>dirección Url del proyecto</strong>. Por ejemplo, escribiría <kbd> <http://www.wingtiptoys.com> </kbd> si ese era la asignación de FQDN que ha agregado al archivo de HOSTS.

- Configurar IIS Express para que usen el FQDN de la aplicación:

    1. Abra un símbolo del sistema con privilegios elevados en Windows.
    2. Escriba el siguiente comando para cambiar a la carpeta de IIS Express:

        <kbd>CD /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Escriba el siguiente comando para agregar el FQDN a la aplicación:

        <kbd>establecer la configuración de appcmd.exe-section:system.applicationHost/sites /+&quot;[nombre='WebApplication1'].bindings.[protocolo='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  Donde **WebApplication1** es el nombre del proyecto y **bindingInformation** contiene el número de puerto y FQDN que desea usar para las pruebas.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Cómo obtener la configuración de la aplicación para la autenticación de Microsoft

La vinculación de una aplicación para Windows Live de Microsoft Authentication es un proceso sencillo. Si no se ha vinculado ya una aplicación para Windows Live, puede usar los siguientes pasos:

1. Vaya a [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) y escriba el nombre de la cuenta de Microsoft y la contraseña cuando se le solicite, a continuación, haga clic en **iniciar sesión en**:

    [![](external-authentication-services/_static/image64.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image63.png)
2. Escriba el nombre y el idioma de la aplicación cuando se le pida y, a continuación, haga clic en **acepto**:

    [![](external-authentication-services/_static/image66.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image65.png)
3. En el **configuración de la API** página de la aplicación, escriba el dominio de redirección para la aplicación y copia el **Id. de cliente** y **secreto de cliente** para el proyecto y, a continuación, Haga clic en **guardar**:

    [![](external-authentication-services/_static/image68.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcional: Deshabilitar el registro Local

La funcionalidad de registro local de ASP.NET actual impedir que programas automatizados (bots) crear miembro cuentas; Por ejemplo, mediante una tecnología de validación y la prevención de bot como [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Por este motivo, debe quitar el vínculo de formulario y el registro de inicio de sesión local en la página de inicio de sesión. Para ello, abra el  *\_Login.cshtml* página en el proyecto y, a continuación, comente las líneas para el panel de inicio de sesión local y el vínculo de registro. La página resultante debería ser similar en el siguiente ejemplo de código:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Una vez que se ha deshabilitado el panel de inicio de sesión local y el vínculo de registro, la página de inicio de sesión mostrará solo los proveedores de autenticación externo que se han habilitado:

[![](external-authentication-services/_static/image70.png "Haga clic para expandir la imagen")](external-authentication-services/_static/image69.png)
