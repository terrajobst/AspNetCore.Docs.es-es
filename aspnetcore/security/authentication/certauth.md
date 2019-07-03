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
# <a name="overview"></a><span data-ttu-id="384ec-103">Información general</span><span class="sxs-lookup"><span data-stu-id="384ec-103">Overview</span></span>

<span data-ttu-id="384ec-104">`Microsoft.AspNetCore.Authentication.Certificate` contiene una implementación similar a [autenticación de certificados](https://tools.ietf.org/html/rfc5246#section-7.4.4) para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="384ec-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="384ec-105">Autenticación de certificado se produce en el nivel TLS, mucho antes de que alguna vez se llegue a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="384ec-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="384ec-106">Más concretamente, esto es un controlador de autenticación que valida el certificado y, a continuación, le ofrece un evento que puede resolver dicho certificado para un `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="384ec-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="384ec-107">[Configurar el host](#configure-your-host-to-require-certificates) para la autenticación de certificado, ya sea IIS, Kestrel, Azure Web Apps, o cualquier otra cosa que esté usando.</span><span class="sxs-lookup"><span data-stu-id="384ec-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="384ec-108">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="384ec-108">Get started</span></span>

<span data-ttu-id="384ec-109">Adquirir un certificado HTTPS, aplicarla, y [configurar el host](#configure-your-host-to-require-certificates) para requerir certificados.</span><span class="sxs-lookup"><span data-stu-id="384ec-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="384ec-110">En la aplicación web, agregue una referencia a la `Microsoft.AspNetCore.Authentication.Certificate` paquete.</span><span class="sxs-lookup"><span data-stu-id="384ec-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="384ec-111">A continuación, en el `Startup.Configure` método, llame a `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` con sus opciones, que proporciona un delegado para `OnCertificateValidated` para realizar cualquier validación adicional en el certificado de cliente enviado con las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="384ec-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="384ec-112">Convertir esa información en un `ClaimsPrincipal` y establézcalo en la `context.Principal` propiedad.</span><span class="sxs-lookup"><span data-stu-id="384ec-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="384ec-113">Si se produce un error de autenticación, este controlador devuelve un `403 (Forbidden)` respuesta en su lugar un `401 (Unauthorized)`, como cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="384ec-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="384ec-114">El razonamiento es que la autenticación debe ocurrir durante la conexión inicial de TLS.</span><span class="sxs-lookup"><span data-stu-id="384ec-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="384ec-115">Cuando llega el controlador, es demasiado tarde.</span><span class="sxs-lookup"><span data-stu-id="384ec-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="384ec-116">No hay ninguna manera para actualizar la conexión de una conexión anónima a uno con un certificado.</span><span class="sxs-lookup"><span data-stu-id="384ec-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="384ec-117">También agregar `app.UseAuthentication();` en el `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="384ec-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="384ec-118">En caso contrario, no se establecerán HttpContext.User `ClaimsPrincipal` creado a partir del certificado.</span><span class="sxs-lookup"><span data-stu-id="384ec-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="384ec-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="384ec-119">For example:</span></span>

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

<span data-ttu-id="384ec-120">El ejemplo anterior muestra la manera predeterminada para agregar la autenticación de certificado.</span><span class="sxs-lookup"><span data-stu-id="384ec-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="384ec-121">El controlador crea a una entidad de usuario con las propiedades comunes del certificado.</span><span class="sxs-lookup"><span data-stu-id="384ec-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="384ec-122">Configurar la validación del certificado</span><span class="sxs-lookup"><span data-stu-id="384ec-122">Configure certificate validation</span></span>

<span data-ttu-id="384ec-123">El `CertificateAuthenticationOptions` controlador tiene algunas validaciones integradas que están las validaciones mínimas que debe realizar en un certificado.</span><span class="sxs-lookup"><span data-stu-id="384ec-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="384ec-124">Cada una de estas opciones está habilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="384ec-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="384ec-125">AllowedCertificateTypes = encadenadas, SelfSigned o todos (encadenados | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="384ec-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="384ec-126">Esta comprobación se valida que se permite solo el tipo de certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="384ec-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="384ec-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="384ec-127">ValidateCertificateUse</span></span>

<span data-ttu-id="384ec-128">Esta comprobación se valida que el certificado presentado por el cliente tiene la autenticación del cliente amplía el uso de claves (EKU), o ninguna EKU en absoluto.</span><span class="sxs-lookup"><span data-stu-id="384ec-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="384ec-129">Según las especificaciones por ejemplo, si no se especifica ningún EKU, todos los EKU se consideran válidos.</span><span class="sxs-lookup"><span data-stu-id="384ec-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="384ec-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="384ec-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="384ec-131">Esta comprobación se valida que el certificado está dentro de su período de validez.</span><span class="sxs-lookup"><span data-stu-id="384ec-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="384ec-132">En cada solicitud, el controlador garantiza que no ha expirado un certificado que tenía cuando se presentó durante su sesión actual.</span><span class="sxs-lookup"><span data-stu-id="384ec-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="384ec-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="384ec-133">RevocationFlag</span></span>

<span data-ttu-id="384ec-134">Una marca que especifica qué certificados de la cadena se comprueba la revocación.</span><span class="sxs-lookup"><span data-stu-id="384ec-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="384ec-135">Solo se realizan comprobaciones de revocación cuando el certificado está enlazado a un certificado raíz.</span><span class="sxs-lookup"><span data-stu-id="384ec-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="384ec-136">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="384ec-136">RevocationMode</span></span>

<span data-ttu-id="384ec-137">Una marca que especifica cómo se realizan las comprobaciones de revocación.</span><span class="sxs-lookup"><span data-stu-id="384ec-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="384ec-138">Especificar una comprobación en línea puede provocar un retraso largo mientras se contacta con la entidad emisora de certificados.</span><span class="sxs-lookup"><span data-stu-id="384ec-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="384ec-139">Solo se realizan comprobaciones de revocación cuando el certificado está enlazado a un certificado raíz.</span><span class="sxs-lookup"><span data-stu-id="384ec-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="384ec-140">¿Configurar mi aplicación para solicitar un certificado solo en determinadas rutas de acceso?</span><span class="sxs-lookup"><span data-stu-id="384ec-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="384ec-141">Esto no es posible.</span><span class="sxs-lookup"><span data-stu-id="384ec-141">This isn't possible.</span></span> <span data-ttu-id="384ec-142">Recuerde que se realiza el intercambio de certificados que el inicio de la conversación de HTTPS, ya está listo el servidor antes de que se recibe la primera solicitud en esa conexión, por lo que no es posible con ámbito basado en los campos de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="384ec-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="384ec-143">Eventos</span><span class="sxs-lookup"><span data-stu-id="384ec-143">Handler events</span></span>

<span data-ttu-id="384ec-144">El controlador tiene dos eventos:</span><span class="sxs-lookup"><span data-stu-id="384ec-144">The handler has two events:</span></span>

* <span data-ttu-id="384ec-145">`OnAuthenticationFailed` &ndash; Se llama si se produce durante la autenticación de una excepción y le permite reaccionar.</span><span class="sxs-lookup"><span data-stu-id="384ec-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="384ec-146">`OnCertificateValidated` &ndash; Se llama después de que se validó el certificado pasó la validación y se ha creado una entidad de seguridad predeterminada.</span><span class="sxs-lookup"><span data-stu-id="384ec-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="384ec-147">Este evento permite realizar su propia validación y aumentar o reemplazar la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="384ec-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="384ec-148">Para obtener ejemplos incluyen:</span><span class="sxs-lookup"><span data-stu-id="384ec-148">For examples include:</span></span>
  * <span data-ttu-id="384ec-149">Determinar si el certificado se conoce a los servicios.</span><span class="sxs-lookup"><span data-stu-id="384ec-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="384ec-150">Creación de su propia entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="384ec-150">Constructing your own principal.</span></span> <span data-ttu-id="384ec-151">Considere el ejemplo siguiente de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="384ec-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="384ec-152">Si encuentra el certificado de entrada no cumple su validación adicional, llame a `context.Fail("failure reason")` con un motivo del error.</span><span class="sxs-lookup"><span data-stu-id="384ec-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="384ec-153">Funcionalidad para el real, probablemente quiera llamar a un servicio registrado en la inserción de dependencias que se conecta a una base de datos u otro tipo de almacén de usuario.</span><span class="sxs-lookup"><span data-stu-id="384ec-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="384ec-154">Acceso al servicio utilizando el contexto que se pasa el delegado.</span><span class="sxs-lookup"><span data-stu-id="384ec-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="384ec-155">Considere el ejemplo siguiente de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="384ec-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="384ec-156">Conceptualmente, la validación del certificado es una preocupación de autorización.</span><span class="sxs-lookup"><span data-stu-id="384ec-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="384ec-157">Agregar una comprobación, por ejemplo, un emisor o huella digital en una directiva de autorización, en lugar de inside `OnCertificateValidated`, es perfectamente aceptable.</span><span class="sxs-lookup"><span data-stu-id="384ec-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="384ec-158">Configurar el host para requerir certificados</span><span class="sxs-lookup"><span data-stu-id="384ec-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="384ec-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="384ec-159">Kestrel</span></span>

<span data-ttu-id="384ec-160">En *Program.cs*, configure Kestrel como sigue:</span><span class="sxs-lookup"><span data-stu-id="384ec-160">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="384ec-161">IIS</span><span class="sxs-lookup"><span data-stu-id="384ec-161">IIS</span></span>

<span data-ttu-id="384ec-162">Complete los pasos siguientes en el Administrador de IIS:</span><span class="sxs-lookup"><span data-stu-id="384ec-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="384ec-163">Seleccione el sitio desde el **conexiones** ficha.</span><span class="sxs-lookup"><span data-stu-id="384ec-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="384ec-164">Haga doble clic en el **configuración SSL** opción el **vista características** ventana.</span><span class="sxs-lookup"><span data-stu-id="384ec-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="384ec-165">Compruebe el **requerir SSL** casilla de verificación y seleccione el **requieren** botón de radio en el **certificados de cliente** sección.</span><span class="sxs-lookup"><span data-stu-id="384ec-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Configuración del certificado de cliente en IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="384ec-167">Azure y servidores proxy web personalizada</span><span class="sxs-lookup"><span data-stu-id="384ec-167">Azure and custom web proxies</span></span>

<span data-ttu-id="384ec-168">Consulte la [hospedaje e implementación documentación](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) acerca de cómo configurar el certificado de middleware de reenvío.</span><span class="sxs-lookup"><span data-stu-id="384ec-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
