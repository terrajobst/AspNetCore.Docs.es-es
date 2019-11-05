---
title: Configurar la autenticación de certificados en ASP.NET Core
author: blowdart
description: Obtenga información acerca de cómo configurar la autenticación de certificados en ASP.NET Core para IIS y HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: 1e646aabb4e384e6906575e7beaa680e91f968a0
ms.sourcegitcommit: e5d4768aaf85703effb4557a520d681af8284e26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616578"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a>Configurar la autenticación de certificados en ASP.NET Core

`Microsoft.AspNetCore.Authentication.Certificate` contiene una implementación similar a la [autenticación de certificado](https://tools.ietf.org/html/rfc5246#section-7.4.4) para ASP.net Core. La autenticación de certificados se produce en el nivel de TLS, mucho antes de que llegue a ASP.NET Core. Más concretamente, se trata de un controlador de autenticación que valida el certificado y, a continuación, le proporciona un evento en el que puede resolver ese certificado en una `ClaimsPrincipal`. 

[Configure el host para la](#configure-your-host-to-require-certificates) autenticación de certificados, ya sea IIS, Kestrel, Azure Web Apps o cualquier otra cosa que esté usando.

## <a name="proxy-and-load-balancer-scenarios"></a>Escenarios de proxy y equilibrador de carga

La autenticación de certificados es un escenario con estado que se usa principalmente en el que un proxy o un equilibrador de carga no controla el tráfico entre clientes y servidores. Si se usa un proxy o un equilibrador de carga, la autenticación de certificado solo funciona si el proxy o el equilibrador de carga:

* Controla la autenticación.
* Pasa la información de autenticación del usuario a la aplicación (por ejemplo, en un encabezado de solicitud), que actúa en la información de autenticación.

Una alternativa a la autenticación de certificados en entornos en los que se usan servidores proxy y equilibradores de carga es Active Directory Federated Services (ADFS) con OpenID Connect (OIDC).

## <a name="get-started"></a>Primeros pasos

Adquiera un certificado HTTPS, aplíquelo y [Configure el host](#configure-your-host-to-require-certificates) para que requiera certificados.

En la aplicación Web, agregue una referencia al paquete de `Microsoft.AspNetCore.Authentication.Certificate`. Después, en el método `Startup.ConfigureServices`, llame a `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` con sus opciones, proporcionando un delegado para que `OnCertificateValidated` realice cualquier validación complementaria en el certificado de cliente enviado con las solicitudes. Convierta esa información en un `ClaimsPrincipal` y establézcala en la propiedad `context.Principal`.

Si se produce un error en la autenticación, este controlador devuelve una respuesta `403 (Forbidden)` en lugar de un `401 (Unauthorized)`, como cabría esperar. La razón es que la autenticación debe realizarse durante la conexión TLS inicial. En el momento en que llega al controlador, es demasiado tarde. No hay ninguna manera de actualizar la conexión de una conexión anónima a una con un certificado.

Agregue también `app.UseAuthentication();` en el método `Startup.Configure`. De lo contrario, HttpContext. user no se establecerá en `ClaimsPrincipal` crea a partir del certificado. Por ejemplo:

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

En el ejemplo anterior se muestra la manera predeterminada de agregar la autenticación de certificado. El controlador crea una entidad de seguridad de usuario mediante las propiedades comunes del certificado.

## <a name="configure-certificate-validation"></a>Configurar la validación de certificados

El controlador de `CertificateAuthenticationOptions` tiene algunas validaciones integradas que son las validaciones mínimas que se deben realizar en un certificado. Cada una de estas opciones está habilitada de forma predeterminada.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = encadenado, SelfSigned o todos (encadenado | SelfSigned

Esta comprobación valida que solo se permite el tipo de certificado adecuado.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Esta comprobación valida que el certificado presentado por el cliente tiene el uso mejorado de clave (EKU) de autenticación del cliente o no tiene ningún EKU. Como indican las especificaciones, si no se especifica ningún EKU, se considera que todos los EKU son válidos.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Esta comprobación valida que el certificado está dentro de su período de validez. En cada solicitud, el controlador garantiza que un certificado que era válido cuando se presentó no ha expirado durante su sesión actual.

### <a name="revocationflag"></a>RevocationFlag

Marca que especifica en qué certificados de la cadena se comprueba la revocación.

Las comprobaciones de revocación solo se realizan cuando el certificado está encadenado a un certificado raíz.

### <a name="revocationmode"></a>RevocationMode

Marca que especifica cómo se realizan las comprobaciones de revocación.

Si se especifica una comprobación en línea, se puede producir un retraso largo mientras se Contacta con la entidad de certificación.

Las comprobaciones de revocación solo se realizan cuando el certificado está encadenado a un certificado raíz.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>¿Puedo configurar mi aplicación para requerir un certificado solo en determinadas rutas?

Esto no es posible. Recuerde que el intercambio de certificados se realiza al inicio de la conversación HTTPS, se realiza en el servidor antes de que se reciba la primera solicitud en esa conexión, por lo que no es posible el ámbito en función de los campos de solicitud.

## <a name="handler-events"></a>Eventos de controlador

El controlador tiene dos eventos:

* `OnAuthenticationFailed` &ndash; llama si se produce una excepción durante la autenticación y le permite reaccionar.
* `OnCertificateValidated` &ndash; llama después de validar el certificado, se ha aprobado la validación y se ha creado una entidad de seguridad predeterminada. Este evento le permite realizar su propia validación y aumentar o reemplazar la entidad de seguridad. Por ejemplo, se incluyen:
  * Determinar si los servicios conocen el certificado.
  * Construir su propia entidad de seguridad. Considere el ejemplo siguiente de `Startup.ConfigureServices`:

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

Si encuentra que el certificado de entrada no cumple su validación adicional, llame a `context.Fail("failure reason")` con un motivo del error.

En el caso de una funcionalidad real, es probable que quiera llamar a un servicio registrado en la inserción de dependencias que se conecta a una base de datos u otro tipo de almacén de usuario. Obtenga acceso a su servicio mediante el contexto que se pasa al delegado. Considere el ejemplo siguiente de `Startup.ConfigureServices`:

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

Conceptualmente, la validación del certificado es un problema de autorización. La adición de una comprobación, por ejemplo, de un emisor o huella digital en una directiva de autorización, en lugar de en `OnCertificateValidated`, es absolutamente aceptable.

## <a name="configure-your-host-to-require-certificates"></a>Configuración del host para requerir certificados

### <a name="kestrel"></a>Kestrel

En *Program.CS*, configure Kestrel como se indica a continuación:

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

Complete los pasos siguientes en el administrador de IIS:

1. Seleccione su sitio en la pestaña **conexiones** .
1. Haga doble clic en la opción **configuración de SSL** en la ventana **ver características** .
1. Active la casilla **requerir SSL** y seleccione el botón de radio **requerir** en la sección **certificados de cliente** .

![Configuración de certificados de cliente en IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure y proxies web personalizados

Consulte la [documentación de host e implementación](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) para obtener información sobre cómo configurar el middleware de reenvío de certificados.
