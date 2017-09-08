---
title: Configurar HTTPS para el desarrollo de ASP.NET Core
author: Rick-Anderson
description: "Muestra cómo configurar HTTPS para el desarrollo de ASP.NET Core 2.0."
keywords: "Núcleo de ASP.NET, SSL, HTTPS"
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: e06f4194d496b5b11aa867e66563bec317e735ff
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="10aa9-104">Configurar HTTPS para el desarrollo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10aa9-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="10aa9-105">En este tema se aplica a ASP.NET Core 2.0 Preview 1</span><span class="sxs-lookup"><span data-stu-id="10aa9-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="10aa9-106">Puede configurar la aplicación para que use HTTPS durante el desarrollo para simular HTTPS en el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="10aa9-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="10aa9-107">Habilitar HTTPS posible que deba habilitar la integración con varios proveedores de identidad (como [Azure AD](https://azure.microsoft.com/services/active-directory) y [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).</span><span class="sxs-lookup"><span data-stu-id="10aa9-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="10aa9-108">En Windows, si no ha instalado Visual Studio o IIS Express, el certificado de desarrollo de IIS Express en el almacén de certificados LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="10aa9-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="10aa9-109">Puede actualizar las propiedades del proyecto en Visual Studio para usar este certificado cuando se ejecuta en segundo plano de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="10aa9-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="10aa9-110">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="10aa9-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="10aa9-111">En el panel izquierdo, seleccione **depurar**.</span><span class="sxs-lookup"><span data-stu-id="10aa9-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="10aa9-112">Comprobar **habilitar SSL**</span><span class="sxs-lookup"><span data-stu-id="10aa9-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="10aa9-113">Copie la dirección URL de SSL y péguelo en el **URL de la aplicación**</span><span class="sxs-lookup"><span data-stu-id="10aa9-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![Pestaña de propiedades de la aplicación web de depuración](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="10aa9-115">Para el desarrollo puede utilizar el certificado de desarrollo de IIS Express, si está disponible, o crear un certificado nuevo para fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="10aa9-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="10aa9-116">El certificado de desarrollo debe estar configurado en el `appsettings.Development.json` de archivos para que no se utiliza en producción:</span><span class="sxs-lookup"><span data-stu-id="10aa9-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

<span data-ttu-id="10aa9-117">Una aplicación con esta configuración de la ejecución en producción iniciará una excepción que indica que "Ningún certificado denominado 'HTTPS' encontrada en la configuración para el entorno actual (producción)".</span><span class="sxs-lookup"><span data-stu-id="10aa9-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="10aa9-118">Para cambiar la [entorno](xref:fundamentals/environments) a `Development`, establezca el `ASPNETCORE_ENVIRONMENT` variable de entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="10aa9-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="10aa9-119">Si no tiene el certificado de IIS Express desarrollo instalado, puede crear un certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="10aa9-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="10aa9-120">En Windows puede crear un certificado de desarrollo y agregarlo al almacén raíz de confianza para el usuario actual mediante la ejecución de los siguientes comandos de PowerShell en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="10aa9-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="10aa9-121">Kestrel en Mac OS y Linux</span><span class="sxs-lookup"><span data-stu-id="10aa9-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="10aa9-122">Puede configurar Kestrel para escuchar a través de HTTPS mediante la configuración de un punto de conexión con el deseado dirección IP, puerto y certificado.</span><span class="sxs-lookup"><span data-stu-id="10aa9-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="10aa9-123">El certificado puede estar configurado de forma alineada, o en el nivel superior `Certificates` sección y, a continuación, hace referencia por nombre:</span><span class="sxs-lookup"><span data-stu-id="10aa9-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

<span data-ttu-id="10aa9-124">En Mac OS y Linux puede crear un certificado autofirmado para el uso de HTTPS [OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="10aa9-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="10aa9-125">Una vez el `certificate.pfx` ha generado el archivo, configure el certificado HTTPS en el `appsettings.Development.json` archivo:</span><span class="sxs-lookup"><span data-stu-id="10aa9-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

<span data-ttu-id="10aa9-126">También necesitará especificar la frase de contraseña para el certificado mediante el establecimiento de la propiedad de configuración de "Certificados: HTTPS:Password".</span><span class="sxs-lookup"><span data-stu-id="10aa9-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="10aa9-127">Las contraseñas no deberían almacenarse en texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="10aa9-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="10aa9-128">Vea [seguro para almacenamiento de secretos durante el desarrollo de aplicaciones](app-secrets.md) para el control adecuado de la frase de contraseña de certificado.</span><span class="sxs-lookup"><span data-stu-id="10aa9-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="10aa9-129">En Mac OS puede [agregar el certificado a la cadena de claves](https://support.apple.com/kb/PH20129?locale=en_US) y [cambiar su configuración de confianza](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) para que sea de confianza para HTTPS durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="10aa9-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="10aa9-130">Para agregar el certificado a la cadena de claves (el equivalente de la `CurrentUser/My` almacenar en Windows) ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="10aa9-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="10aa9-131">Y, a continuación, debe confiar en el certificado:</span><span class="sxs-lookup"><span data-stu-id="10aa9-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="10aa9-132">A continuación, puede configurar la aplicación para usar este certificado en el desarrollo similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="10aa9-132">You can then configure your app to use this certificate in development like this:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
