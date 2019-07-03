---
title: Configurar la autenticación de certificado en ASP.NET Core
author: blowdart
description: Obtenga información sobre cómo configurar la autenticación de certificado en ASP.NET Core para IIS y HTTP.sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 8609c58265340da1d618135795915d6c49e750a3
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538719"
---
# <a name="overview"></a>Información general

`Microsoft.AspNetCore.Authentication.Certificate` contiene una implementación similar a [autenticación de certificados](https://tools.ietf.org/html/rfc5246#section-7.4.4) para ASP.NET Core. Autenticación de certificado se produce en el nivel TLS, mucho antes de que alguna vez se llegue a ASP.NET Core. Más concretamente, esto es un controlador de autenticación que valida el certificado y, a continuación, le ofrece un evento que puede resolver dicho certificado para un `ClaimsPrincipal`. 

[Configurar el host](#configure-your-host-to-require-certificates) para la autenticación de certificado, ya sea IIS, Kestrel, Azure Web Apps, o cualquier otra cosa que esté usando.

## <a name="get-started"></a>Primeros pasos

Adquirir un certificado HTTPS, aplicarla, y [configurar el host](#configure-your-host-to-require-certificates) para requerir certificados.

En la aplicación web, agregue una referencia a la `Microsoft.AspNetCore.Authentication.Certificate` paquete. A continuación, en el `Startup.Configure` método, llame a `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` con sus opciones, que proporciona un delegado para `OnCertificateValidated` para realizar cualquier validación adicional en el certificado de cliente enviado con las solicitudes. Convertir esa información en un `ClaimsPrincipal` y establézcalo en la `context.Principal` propiedad.

Si se produce un error de autenticación, este controlador devuelve un `403 (Forbidden)` respuesta en su lugar un `401 (Unauthorized)`, como cabría esperar. El razonamiento es que la autenticación debe ocurrir durante la conexión inicial de TLS. Cuando llega el controlador, es demasiado tarde. No hay ninguna manera para actualizar la conexión de una conexión anónima a uno con un certificado.

También agregar `app.UseAuthentication();` en el `Startup.Configure` método. En caso contrario, no se establecerán HttpContext.User `ClaimsPrincipal` creado a partir del certificado. Por ejemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

El ejemplo anterior muestra la manera predeterminada para agregar la autenticación de certificado. El controlador crea a una entidad de usuario con las propiedades comunes del certificado.

## <a name="configure-certificate-validation"></a>Configurar la validación del certificado

El `CertificateAuthenticationOptions` controlador tiene algunas validaciones integradas que están las validaciones mínimas que debe realizar en un certificado. Cada una de estas opciones está habilitada de forma predeterminada.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = encadenadas, SelfSigned o todos (encadenados | SelfSigned)

Esta comprobación se valida que se permite solo el tipo de certificado adecuado.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Esta comprobación se valida que el certificado presentado por el cliente tiene la autenticación del cliente amplía el uso de claves (EKU), o ninguna EKU en absoluto. Según las especificaciones por ejemplo, si no se especifica ningún EKU, todos los EKU se consideran válidos.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Esta comprobación se valida que el certificado está dentro de su período de validez. En cada solicitud, el controlador garantiza que no ha expirado un certificado que tenía cuando se presentó durante su sesión actual.

### <a name="revocationflag"></a>RevocationFlag

Una marca que especifica qué certificados de la cadena se comprueba la revocación.

Solo se realizan comprobaciones de revocación cuando el certificado está enlazado a un certificado raíz.

### <a name="revocationmode"></a>RevocationMode

Una marca que especifica cómo se realizan las comprobaciones de revocación.

Especificar una comprobación en línea puede provocar un retraso largo mientras se contacta con la entidad emisora de certificados.

Solo se realizan comprobaciones de revocación cuando el certificado está enlazado a un certificado raíz.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>¿Configurar mi aplicación para solicitar un certificado solo en determinadas rutas de acceso?

Esto no es posible. Recuerde que se realiza el intercambio de certificados que el inicio de la conversación de HTTPS, ya está listo el servidor antes de que se recibe la primera solicitud en esa conexión, por lo que no es posible con ámbito basado en los campos de la solicitud.

## <a name="handler-events"></a>Eventos

El controlador tiene dos eventos:

* `OnAuthenticationFailed` &ndash; Se llama si se produce durante la autenticación de una excepción y le permite reaccionar.
* `OnCertificateValidated` &ndash; Se llama después de que se validó el certificado pasó la validación y se ha creado una entidad de seguridad predeterminada. Este evento permite realizar su propia validación y aumentar o reemplazar la entidad de seguridad. Para obtener ejemplos incluyen:
  * Determinar si el certificado se conoce a los servicios.
  * Creación de su propia entidad de seguridad. Considere el ejemplo siguiente de `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

Si encuentra el certificado de entrada no cumple su validación adicional, llame a `context.Fail("failure reason")` con un motivo del error.

Funcionalidad para el real, probablemente quiera llamar a un servicio registrado en la inserción de dependencias que se conecta a una base de datos u otro tipo de almacén de usuario. Acceso al servicio utilizando el contexto que se pasa el delegado. Considere el ejemplo siguiente de `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

Conceptualmente, la validación del certificado es una preocupación de autorización. Agregar una comprobación, por ejemplo, un emisor o huella digital en una directiva de autorización, en lugar de inside `OnCertificateValidated`, es perfectamente aceptable.

## <a name="configure-your-host-to-require-certificates"></a>Configurar el host para requerir certificados

### <a name="kestrel"></a>Kestrel

En *Program.cs*, configure Kestrel como sigue:

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a>IIS

Complete los pasos siguientes en el Administrador de IIS:

1. Seleccione el sitio desde el **conexiones** ficha.
1. Haga doble clic en el **configuración SSL** opción el **vista características** ventana.
1. Compruebe el **requerir SSL** casilla de verificación y seleccione el **requieren** botón de radio en el **certificados de cliente** sección.

![Configuración del certificado de cliente en IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure y servidores proxy web personalizada

Consulte la [hospedaje e implementación documentación](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) acerca de cómo configurar el certificado de middleware de reenvío.
