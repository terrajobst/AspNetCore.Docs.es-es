---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Detección de clase de inicio OWIN | Microsoft Docs
author: Praburaj
description: Este tutorial muestra cómo configurar qué clase de inicio OWIN se carga. Para obtener más información sobre OWIN, vea una información general del proyecto Katana. En este tutorial era...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 4e753187f1caae646402712c2abc28856ae71a79
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910712"
---
<a name="owin-startup-class-detection"></a>Detección de clase de inicio OWIN
====================
por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial muestra cómo configurar qué clase de inicio OWIN se carga. Para obtener más información sobre OWIN, consulte [una visión general del proyecto Katana](an-overview-of-project-katana.md). En este tutorial se escribió por Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan y Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Requisitos previos
>
> [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)


## <a name="owin-startup-class-detection"></a>Detección de clase de inicio OWIN

 Todas las aplicaciones OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de aplicación. Hay diferentes maneras de conectarse la clase de inicio con el tiempo de ejecución, según el modelo de hospedaje que elija (OwinHost, IIS y IIS Express). La clase de inicio que se muestra en este tutorial se puede usar en cada aplicación de hospedaje. Conecte la clase de inicio con el tiempo de ejecución hospedaje mediante uno de estos enfoques:

1. **Convención de nomenclatura**: Katana busca una clase denominada `Startup` en el espacio de nombres que coincide con el nombre del ensamblado o el espacio de nombres global.
2. **Atributo OwinStartup**: este es el enfoque que tardará la mayoría de los desarrolladores para especificar la clase de inicio. El atributo siguiente establecerá la clase de inicio el `TestStartup` clase en el `StartupDemo` espacio de nombres.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   El `OwinStartup` atributo invalida la convención de nomenclatura. También puede especificar un nombre descriptivo con este atributo, sin embargo, con un nombre descriptivo, deberá usar también el `appSetting` elemento en el archivo de configuración.
3. **El elemento appSetting del archivo de configuración**: el `appSetting` elemento invalida la `OwinStartup` atributo y la convención de nomenclatura. Puede tener varias clases de inicio (cada mediante un `OwinStartup` atributo) y configurar qué clase de inicio se cargan en un archivo de configuración mediante marcado similar al siguiente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   También se puede usar la clave siguiente, que se especifica explícitamente la clase de inicio y el ensamblado:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   El siguiente código XML en el archivo de configuración especifica un nombre de clase de inicio descriptivo `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   El marcado anterior se debe usar con la siguiente `OwinStartup` atributo que especifica un nombre descriptivo y hace que el `ProductionStartup2` clase para ejecutar.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Para deshabilitar la detección de inicio OWIN agregar el `appSetting owin:AutomaticAppStartup` con un valor de `"false"` en el archivo web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Crear una aplicación Web de ASP.NET mediante el inicio de OWIN

1. Crear una aplicación web de Asp.Net vacía y asígnele el nombre **StartupDemo**. -Instalar `Microsoft.Owin.Host.SystemWeb` con el Administrador de paquetes de NuGet. Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**y, a continuación, **Package Manager Console**. Escriba el comando siguiente:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Agregue una clase de inicio OWIN. En Visual Studio 2013 a la derecha, haga clic en el proyecto y seleccione **Agregar clase**.: en el **Agregar nuevo elemento** diálogo cuadro, escriba *OWIN* en el campo de búsqueda y cambie el nombre a Startup.cs, y, a continuación, haga clic en **agregar**.

     ![](owin-startup-class-detection/_static/image1.png)

   La próxima vez que desee agregar un *clase Owin Startup*, estará en disponible desde el **agregar** menú.

     ![](owin-startup-class-detection/_static/image2.png)

   Como alternativa, puede a la derecha, haga clic en el proyecto y seleccione **agregar**, a continuación, seleccione **nuevo elemento**y, a continuación, seleccione el **clase Owin Startup**.

     ![](owin-startup-class-detection/_static/image3.png)

- Reemplace el código generado en el *Startup.cs* archivo por lo siguiente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  El `app.Use` expresión lambda se usa para registrar el componente de middleware especificado a la canalización OWIN. En este caso estamos configurando el registro de solicitudes entrantes antes de responder a la solicitud entrante. El `next` parámetro es el delegado ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [tarea](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) al componente siguiente en la canalización. El `app.Run` expresión lambda se enlaza la canalización de solicitudes entrantes y proporciona el mecanismo de respuesta.
     > [!NOTE]
     > En el código anterior, hemos comentado la `OwinStartup` atributo y nos estamos depender de la convención de la ejecución de la clase denominada `Startup` .-presione ***F5*** para ejecutar la aplicación. Actualice la vista varias veces.

    ![](owin-startup-class-detection/_static/image4.png) Nota: El número que aparece en las imágenes en este tutorial no coincidirá con el número que aparece. La cadena de milisegundo se usa para mostrar una nueva respuesta cuando se actualice la página.
  Puede ver la información de seguimiento en el **salida** ventana.

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Agregar más clases de inicio

En esta sección vamos a agregar otra clase de inicio. Puede agregar varias clases de inicio OWIN para la aplicación. Por ejemplo, puede crear clases de inicio para el desarrollo, pruebas y producción.

1. Cree una nueva clase de inicio OWIN y asígnele el nombre `ProductionStartup`.
2. Reemplace el código generado con el siguiente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Presione F5 de Control para ejecutar la aplicación. El `OwinStartup` atributo especifica que se ejecuta la clase de inicio de producción.

    ![](owin-startup-class-detection/_static/image6.png)
4. Cree otra clase de inicio OWIN y asígnele el nombre `TestStartup`.
5. Reemplace el código generado con el siguiente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   El `OwinStartup` sobrecarga de atributo anterior especifica `TestingConfiguration` como el *descriptivo* nombre de la clase de inicio.
6. Abra el *web.config* archivo y agregue la clave de inicio de la aplicación OWIN que especifica el nombre descriptivo de la clase de inicio:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Presione F5 de Control para ejecutar la aplicación. El elemento de configuración de la aplicación toma precedentes y la prueba se ejecuta la configuración.

    ![](owin-startup-class-detection/_static/image7.png)
8. Quitar el *descriptivo* nombre desde el `OwinStartup` atributo el `TestStartup` clase.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Reemplace la clave de inicio de la aplicación OWIN en la *web.config* archivo por lo siguiente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Revertir la `OwinStartup` atributo en cada clase en el código de atributo predeterminado generado por Visual Studio:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Cada una de las claves de inicio de la aplicación OWIN siguientes hará que la clase de producción ejecutar.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    La clave del último inicio especifica el método de configuración de inicio. La siguiente clave de inicio de OWIN aplicación le permite cambiar el nombre de la clase de configuración para `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Uso de Owinhost.exe

1. Reemplace el archivo Web.config con el marcado siguiente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   La clave del último gana, así que en este caso `TestStartup` se especifica.
2. Instalar Owinhost desde la PMC:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Navegue hasta la carpeta de aplicación (la carpeta que contiene el *Web.config* archivo) y en un símbolo del sistema y escriba:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Se mostrará la ventana de comandos:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Inicie un explorador con la dirección URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost respeta las convenciones de inicio mencionadas anteriormente.
5. En la ventana de comandos, presione ENTRAR para salir OwinHost.
6. En el `ProductionStartup` de clases, agregue el siguiente atributo OwinStartup que especifica un nombre descriptivo de *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. En el símbolo del sistema y escriba:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Se carga la clase de inicio de producción.
    ![](owin-startup-class-detection/_static/image9.png)
   Nuestra aplicación tiene varias clases de inicio y, en este ejemplo se ha aplazado qué clase de inicio para cargar hasta el tiempo de ejecución.
8. Pruebe las siguientes opciones de inicio en tiempo de ejecución:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
