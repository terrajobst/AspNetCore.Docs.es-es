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
ms.openlocfilehash: 7c366ffbac71152c2f29901ff12bac2962e83e3e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>Configurar HTTPS para el desarrollo de ASP.NET Core

> [!NOTE] 
> En este tema se aplica a ASP.NET Core 2.0 Preview 1

Puede configurar la aplicación para que use HTTPS durante el desarrollo para simular HTTPS en el entorno de producción. Habilitar HTTPS posible que deba habilitar la integración con varios proveedores de identidad (como [Azure AD](https://azure.microsoft.com/services/active-directory) y [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).

<a name="iisxpress"></a>

En Windows, si no ha instalado Visual Studio o IIS Express, el certificado de desarrollo de IIS Express en el almacén de certificados LocalMachine. Puede actualizar las propiedades del proyecto en Visual Studio para usar este certificado cuando se ejecuta en segundo plano de IIS Express.

   * En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.
   * En el panel izquierdo, seleccione **depurar**.
   * Comprobar **habilitar SSL**
   * Copie la dirección URL de SSL y péguelo en el **URL de la aplicación**

![Pestaña de propiedades de la aplicación web de depuración](enforcing-ssl/_static/ssl.png)

Para el desarrollo puede utilizar el certificado de desarrollo de IIS Express, si está disponible, o crear un certificado nuevo para fines de desarrollo. El certificado de desarrollo debe estar configurado en el `appsettings.Development.json` de archivos para que no se utiliza en producción:

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

Una aplicación con esta configuración de la ejecución en producción iniciará una excepción que indica que "Ningún certificado denominado 'HTTPS' encontrada en la configuración para el entorno actual (producción)". Para cambiar la [entorno](xref:fundamentals/environments) a `Development`, establezca el `ASPNETCORE_ENVIRONMENT` variable de entorno `Development`.

Si no tiene el certificado de IIS Express desarrollo instalado, puede crear un certificado de desarrollo. En Windows puede crear un certificado de desarrollo y agregarlo al almacén raíz de confianza para el usuario actual mediante la ejecución de los siguientes comandos de PowerShell en un símbolo del sistema con privilegios elevados:

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>Kestrel en Mac OS y Linux

Puede configurar Kestrel para escuchar a través de HTTPS mediante la configuración de un punto de conexión con el deseado dirección IP, puerto y certificado. El certificado puede estar configurado de forma alineada, o en el nivel superior `Certificates` sección y, a continuación, hace referencia por nombre:

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

En Mac OS y Linux puede crear un certificado autofirmado para el uso de HTTPS [OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

Una vez el `certificate.pfx` ha generado el archivo, configure el certificado HTTPS en el `appsettings.Development.json` archivo:

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

También necesitará especificar la frase de contraseña para el certificado mediante el establecimiento de la propiedad de configuración de "Certificados: HTTPS:Password". Las contraseñas no deberían almacenarse en texto sin formato. Vea [seguro para almacenamiento de secretos durante el desarrollo de aplicaciones](app-secrets.md) para el control adecuado de la frase de contraseña de certificado.

En Mac OS puede [agregar el certificado a la cadena de claves](https://support.apple.com/kb/PH20129?locale=en_US) y [cambiar su configuración de confianza](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) para que sea de confianza para HTTPS durante el desarrollo. Para agregar el certificado a la cadena de claves (el equivalente de la `CurrentUser/My` almacenar en Windows) ejecute el siguiente comando:

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

Y, a continuación, debe confiar en el certificado:

```bash
security add-trusted-cert localhost.cer
```

A continuación, puede configurar la aplicación para usar este certificado en el desarrollo similar al siguiente:

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
