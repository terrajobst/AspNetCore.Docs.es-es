---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Usar proveedores de OAuth con MVC 4 | Documentos de Microsoft
author: tfitzmac
description: "Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 4 que permite a los usuarios iniciar sesión con credenciales de un proveedor externo, como Facebo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="using-oauth-providers-with-mvc-4"></a>Usar proveedores de OAuth con MVC 4
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 4 que permite a los usuarios iniciar sesión con credenciales de un proveedor externo, como Facebook, Twitter, Microsoft o Google y, a continuación, integrar parte de la funcionalidad de cada uno de ellos en su aplicación Web. Por simplicidad, este tutorial se centra en trabajar con las credenciales de Facebook.
> 
> Para usar credenciales externas en una aplicación web ASP.NET MVC 5, consulte [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja importante porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden ser más dispuestos a registrarse para el sitio si no tienen que crear y recordar a un nuevo conjunto de credenciales. Además, después de que un usuario ha iniciado la sesión a través de uno de estos proveedores, puede incorporar operaciones sociales desde el proveedor.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En este tutorial, hay dos objetivos principales:

1. Permitir a los usuarios iniciar sesión con credenciales de un proveedor de OAuth.
2. Recuperar información de la cuenta del proveedor e integrar esa información en el registro de cuenta para el sitio.

Aunque los ejemplos de este tutorial se centran en el uso de Facebook como proveedor de autenticación, puede modificar el código para utilizar cualquiera de los proveedores. Los pasos para implementar cualquier proveedor son muy similares a los pasos que se verá en este tutorial. Solo observará diferencias significativas al realizar llamadas directas a la API del proveedor de conjunto.

## <a name="prerequisites"></a>Requisitos previos

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) o [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

O bien

- Microsoft Visual Studio 2010 SP1 o [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Además, en este tema se supone que tiene conocimientos básicos acerca de MVC de ASP.NET y Visual Studio. Si necesita una introducción a ASP.NET MVC 4, consulte [Introducción a ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Crear el proyecto

En Visual Studio, cree una nueva aplicación Web de ASP.NET MVC 4 y asígnele el nombre &quot;OAuthMVC&quot;. Puede tener como destino .NET Framework 4.5 o 4.

![Crear proyecto](using-oauth-providers-with-mvc/_static/image1.png)

En la ventana nuevo proyecto de MVC de ASP.NET 4, seleccione **aplicación de Internet** y deje **Razor** como el motor de vista.

![Seleccione la aplicación de Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Permiten a un proveedor

Cuando se crea una aplicación web de MVC 4 con la plantilla de aplicación de Internet, el proyecto se crea con un archivo denominado AuthConfig.cs en la aplicación\_carpeta de inicio.

![Archivo AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

El archivo AuthConfig contiene código para registrar a clientes para los proveedores de autenticación externo. De forma predeterminada, este código está comentado, por lo que se ha habilitado ninguna de los proveedores externos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Debe quitar los comentarios este código para usar al cliente de autenticación externo. Quite la marca de comentario solo los proveedores que desea incluir en el sitio. Para este tutorial, solo permitirá que las credenciales de Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Observe que en el ejemplo anterior que el método incluye las cadenas vacías para los parámetros de registro. Si intenta ejecutar la aplicación ahora, la aplicación produce una excepción de argumento porque no se permiten cadenas vacías para los parámetros. Para proporcionar valores válidos, debe registrar el sitio web con los proveedores externos, como se muestra en la sección siguiente.

## <a name="registering-with-an-external-provider"></a>Registrar con un proveedor externo.

Para autenticar a los usuarios con credenciales de un proveedor externo, debe registrar el sitio web con el proveedor. Al registrar su sitio, recibirá los parámetros (por ejemplo, la clave o el identificador y el secreto) para incluir al registrar al cliente. Debe tener una cuenta con los proveedores que se va a usar.

Este tutorial no muestra todos los pasos que debe seguir para registrar con estos proveedores. Los pasos no suelen ser difíciles. Para registrar correctamente el sitio, siga las instrucciones proporcionadas en esos sitios. Para empezar a registrar su sitio, consulte el sitio para desarrolladores para:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Al registrar el sitio con Facebook, puede proporcionar &quot;localhost&quot; para el dominio del sitio y `&quot;http://localhost/&quot;` para la dirección URL, como se muestra en la imagen siguiente. El uso de localhost funciona con la mayoría de los proveedores, pero actualmente no funcionan con el proveedor de Microsoft. Para el proveedor de Microsoft, debe incluir una dirección URL de sitio web válido.

![registrar el sitio](using-oauth-providers-with-mvc/_static/image4.png)

En la imagen anterior, se han quitado los valores para el identificador de la aplicación, el secreto de la aplicación y el correo electrónico de contacto. Al registrar su sitio realmente, dichos valores estará presentes. Deberá tener en cuenta los valores de identificador de la aplicación y el secreto de la aplicación ya que lo agregará a la aplicación.

## <a name="creating-test-users"></a>Crear usuarios de prueba

Si no cuenta con una cuenta de Facebook existente para probar su sitio, puede omitir esta sección.

Puede crear fácilmente usuarios de prueba para su aplicación en la página de administración de la aplicación de Facebook. Puede utilizar estas cuentas para iniciar sesión en el sitio de prueba. Crear usuarios de prueba, haga clic en el **Roles** vínculo en el panel de navegación izquierdo y hacer clic en el **crear** vínculo.

![crear usuarios de prueba](using-oauth-providers-with-mvc/_static/image5.png)

El sitio de Facebook crea automáticamente el número de cuentas de prueba que se solicita.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Agregando el identificador de aplicación y el secreto del proveedor

Ahora que ha recibido el identificador y el secreto de Facebook, vuelva al archivo AuthConfig y agregarlas como los valores de parámetro. Los valores que se muestra a continuación no son valores reales.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Inicie sesión con credenciales externas

Eso es todo lo que tiene que hacer para habilitar las credenciales externas en el sitio. Ejecute la aplicación y haga clic en el vínculo de inicio de sesión en la esquina superior derecha. La plantilla automáticamente reconoce que ha registrado Facebook como proveedor e incluye un botón para el proveedor. Si registra varios proveedores, un botón para cada una de ellas se incluye automáticamente.

![inicio de sesión externo](using-oauth-providers-with-mvc/_static/image6.png)

Este tutorial no incluye información sobre cómo personalizar lo botones de inicio de sesión para los proveedores externos. Para obtener esa información, consulte [personalizar la interfaz de usuario de inicio de sesión al utilizar OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Haga clic en el botón de Facebook para iniciar sesión con credenciales de Facebook. Cuando selecciona uno de los proveedores externos, se redirige a ese sitio y que el servicio solicita que inicie sesión.

La siguiente imagen muestra la pantalla de inicio de sesión de Facebook. Observa que está usando su cuenta de Facebook para iniciar sesión en un sitio denominado oauthmvcexample.

![autenticación de Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Después de iniciar sesión con credenciales de Facebook, una página informa al usuario que el sitio tendrá acceso a información básica.

![permisos de solicitud](using-oauth-providers-with-mvc/_static/image8.png)

Después de seleccionar **vaya a aplicación**, el usuario debe registrar para el sitio. La siguiente imagen muestra la página de registro después de que un usuario ha iniciado sesión con credenciales de Facebook. El nombre de usuario es normalmente rellenado previamente con un nombre del proveedor.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Haga clic en **registrar** para completar el registro. Cierre el explorador.

Puede ver que se ha agregado la nueva cuenta de la base de datos. En el Explorador de servidores, abra el **DefaultConnection** la base de datos y abra la **tablas** carpeta.

![tablas de bases de datos](using-oauth-providers-with-mvc/_static/image10.png)

Haga clic en el **UserProfile** de tabla y seleccione **mostrar datos de tabla**.

![Mostrar datos](using-oauth-providers-with-mvc/_static/image11.png)

Verá la nueva cuenta agregada. Examine los datos de **página Web\_OAuthMembership** tabla. Verá más datos relacionados con el proveedor externo para la cuenta que acaba de agregar.

Si solo desea habilitar autenticación externa, ya ha terminado. Sin embargo, puede integrar más información del proveedor en el nuevo proceso de registro de usuario, tal como se muestra en las secciones siguientes.

## <a name="create-models-for-additional-user-information"></a>Crear modelos para obtener información adicional del usuario

Como ha observado en las secciones anteriores, no es necesario recuperar cualquier información adicional para que funcione el registro de la cuenta integrada. Sin embargo, proveedores externos más pasan volver más información sobre el usuario. Las secciones siguientes muestran cómo conservar esa información y guardarlo en una base de datos. En concreto, se conservarán los valores para el nombre completo del usuario, el URI de la página web de personal del usuario y un valor que indica si Facebook ha comprobado la cuenta.

Va a usar [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) para agregar una tabla para almacenar información de usuario adicional. Va a agregar la tabla a una base de datos existente, por lo que primero deberá crear una instantánea de la base de datos actual. Al crear una instantánea de la base de datos actual, puede crear más adelante una migración que contiene solo la nueva tabla. Para crear una instantánea de la base de datos actual:

1. Abra el **consola de administrador de paquetes**
2. Ejecute el comando **enable-migrations**
3. Ejecute el comando **migración agregar inicial: IgnoreChanges**
4. Ejecute el comando **Actualizar base de datos**

Ahora, agregará las nuevas propiedades. En la carpeta Models, abra el archivo AccountModels.cs y encontrar la clase RegisterExternalLoginModel. La clase RegisterExternalLoginModel contiene valores que proceden del proveedor de autenticación. Agregue propiedades denominadas FullName y vínculo, como se indica a continuación.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

También en AccountModels.cs, agregue una nueva clase denominada ExtraUserInformation. Esta clase representa la nueva tabla que se creará en la base de datos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

En la clase UsersContext, agregue el código que aparece resaltado a continuación para crear una propiedad DbSet para la nueva clase.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Ahora está listo para crear la nueva tabla. Abra la consola de administrador de paquetes de nuevo y esta vez:

1. Ejecute el comando **AddExtraUserInformation de agregar la migración**
2. Ejecute el comando **Actualizar base de datos**

La nueva tabla ahora existe en la base de datos.

## <a name="retrieve-the-additional-data"></a>Recuperar los datos adicionales

Hay dos maneras de recuperar datos de usuario adicionales. La primera manera consiste en conservar los datos de usuario que se pasan, de forma predeterminada, durante la solicitud de autenticación. La segunda manera es específicamente llamar a la API del proveedor y solicitar más información. Valores de FullName y vínculo automáticamente se pasan por Facebook. Un valor que indica si Facebook ha comprobado la cuenta se recupera mediante una llamada a la API de Facebook. En primer lugar, rellenará los valores de FullName y vínculo y, a continuación, más adelante, se obtendrá el valor comprobado.

Para recuperar los datos de usuario adicionales, abra el **AccountController.cs** un archivo en el **controladores** carpeta.

Este archivo contiene la lógica para el registro, registrar y administrar cuentas. En concreto, tenga en cuenta los métodos llamados **ExternalLoginCallback** y **ExternalLoginConfirmation**. Dentro de estos métodos, puede agregar código para personalizar las operaciones de inicio de sesión externo para la aplicación. La primera línea de la **ExternalLoginCallback** método contiene:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Datos de usuario adicional se pasan en el **ExtraData** propiedad de la **AuthenticationResult** objeto que se devuelve desde el **VerifyAuthentication** método. El cliente de Facebook contiene los siguientes valores en el **ExtraData** propiedad:

- id
- name
- vínculo
- sexo
- accesstoken

Otros proveedores tienen datos similares pero ligeramente distintos en la propiedad ExtraData.

Si el usuario es nuevo en su sitio, algunas recuperar los datos adicionales y pasar datos a la vista de confirmación. El último bloque de código en el método se ejecuta sólo si el usuario es nuevo en su sitio. Reemplace la línea siguiente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Con esta línea:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Este cambio simplemente incluye valores de las propiedades FullName y vínculo.

En el **ExternalLoginConfirmation** método, modifique el código como se indica a continuación para guardar la información adicional del usuario.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajustar la vista

Los datos de usuario adicionales que recuperan desde el proveedor se mostrará en la página de registro.

En el **vistas**/**cuenta** carpeta, abra **ExternalLoginConfirmation.cshtml**. Debajo del campo existente para el nombre de usuario, agregar campos de FullName, vínculo y PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Ya está casi listo para ejecutar la aplicación y registrar un nuevo usuario con la información adicional que se guarda. Debe tener una cuenta que ya no está registrada con el sitio. Puede usar una cuenta de prueba diferentes, o eliminar las filas de la **UserProfile** y **páginas Web\_OAuthMembership** tablas para la cuenta que se va a volver a usar. Si se elimina las filas, se asegurará de que la cuenta se vuelve a registrar.

Ejecute la aplicación y registrar el nuevo usuario. Tenga en cuenta que esta vez la página de confirmación contiene más valores.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Después de completar el registro, cierre el explorador. Buscar en la base de datos debe tener en cuenta los nuevos valores en el **ExtraUserInformation** tabla.

## <a name="install-nuget-package-for-facebook-api"></a>Instalar paquete de NuGet para la API de Facebook

Facebook proporciona una [API](https://developers.facebook.com/docs/reference/apis/) que puede llamar para realizar operaciones. Puede llamar a la API de Facebook dirigiendo enviar solicitudes HTTP, o mediante el uso de instalar un paquete de NuGet que facilita el envío de dichas solicitudes. Con un paquete de NuGet se muestra en este tutorial, pero la instalación de NuGet paquete no es fundamental. Este tutorial muestra cómo utilizar el paquete de Facebook C# SDK. Hay otros paquetes de NuGet que le ayuden con una llamada a la API de Facebook.

Desde el **administrar paquetes de NuGet** windows, seleccione el paquete de Facebook C# SDK.

![Instalar paquete](using-oauth-providers-with-mvc/_static/image13.png)

Utilizará el SDK de Facebook C# para llamar a una operación que requiere el token de acceso para el usuario. La sección siguiente muestra cómo obtener el token de acceso.

## <a name="retrieve-access-token"></a>Recuperar el token de acceso

Vuelva a proveedores externos más pasan un token de acceso una vez comprobadas las credenciales del usuario. Este token de acceso es muy importante porque permite llamar a las operaciones que solo están disponibles para los usuarios autenticados. Por lo tanto, recuperar y almacenar el token de acceso son fundamental cuando desea proporcionar más funcionalidad.

En función del proveedor externo, el token de acceso puede ser válido para solo un período de tiempo limitado. Para asegurarse de que tiene un token de acceso válido, se lo recuperará cada vez que el usuario inicia sesión y almacenar como un valor de sesión en lugar de guardarlo en una base de datos.

En el **ExternalLoginCallback** método, también se pasa el token de acceso en la **ExtraData** propiedad de la **AuthenticationResult** objeto. Agregue el código resaltado a **ExternalLoginCallback** para guardar el token de acceso en la **sesión** objeto. Este código se ejecuta cada vez que el usuario inicia sesión con una cuenta de Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Aunque en este ejemplo se recupera un token de acceso de Facebook, puede recuperar el token de acceso desde cualquier proveedor externo a través de la misma clave denominada &quot;accesstoken&quot;.

## <a name="logging-off"></a>Cerrar sesión

El valor predeterminado **LogOff** método registra el usuario de la aplicación, pero no registra el usuario fuera el proveedor externo. Para iniciar sesión del usuario de Facebook e impedir que el token de persistencia después de que el usuario se desconecte, puede agregar el código que aparece resaltado a la **LogOff** método en el AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

El valor que se proporciona en el `next` parámetro es la dirección URL debe seguir después de que el usuario ha cerrado la sesión Facebook. Al realizar pruebas en el equipo local, proporcionaría el número de puerto correcto para su sitio local. En producción, proporcionaría una página predeterminada, por ejemplo, contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperar la información del usuario que requiere el token de acceso

Ahora que ha almacenado el token de acceso e instalado el paquete de Facebook C# SDK, puede utilizarlos juntos para solicitar información adicional del usuario de Facebook. En el **ExternalLoginConfirmation** método, cree una instancia de la **FacebookClient** clase pasando el valor del token de acceso. Solicite el valor de la **comprobado** propiedad para el usuario autenticado actual. El **comprobado** propiedad indica si Facebook se ha validado la cuenta de otra manera, como enviar un mensaje a un teléfono móvil. Guarde este valor en la base de datos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Nuevo debe eliminar los registros de la base de datos para el usuario, o use otra cuenta de Facebook.

Ejecute la aplicación y registrar el nuevo usuario. Mire el **ExtraUserInformation** tabla para ver el valor de la propiedad comprobado.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado un sitio que se integra con Facebook para la autenticación de usuario y datos de registro. Aprendió a utilizar el comportamiento predeterminado que está configurado para que la aplicación web de MVC 4 y cómo personalizar el comportamiento predeterminado.

## <a name="related-topics"></a>Temas relacionados

- [Crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
