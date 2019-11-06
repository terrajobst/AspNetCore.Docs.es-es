---
title: Configurar la autenticación de certificados en ASP.NET Core
author: blowdart
description: Obtenga información acerca de cómo configurar la autenticación de certificados en ASP.NET Core para IIS y HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 11/05/2019
uid: security/authentication/certauth
ms.openlocfilehash: 081935e6e6248b5fe9b7bf4cd966dc73761d2ec1
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634048"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="02cfa-103">Configurar la autenticación de certificados en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02cfa-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="02cfa-104">`Microsoft.AspNetCore.Authentication.Certificate` contiene una implementación similar a la [autenticación de certificado](https://tools.ietf.org/html/rfc5246#section-7.4.4) para ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="02cfa-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="02cfa-105">La autenticación de certificados se produce en el nivel de TLS, mucho antes de que llegue a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02cfa-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="02cfa-106">Más concretamente, se trata de un controlador de autenticación que valida el certificado y, a continuación, le proporciona un evento en el que puede resolver ese certificado en una `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="02cfa-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="02cfa-107">[Configure el host para la](#configure-your-host-to-require-certificates) autenticación de certificados, ya sea IIS, Kestrel, Azure Web Apps o cualquier otra cosa que esté usando.</span><span class="sxs-lookup"><span data-stu-id="02cfa-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="02cfa-108">Escenarios de proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="02cfa-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="02cfa-109">La autenticación de certificados es un escenario con estado que se usa principalmente en el que un proxy o un equilibrador de carga no controla el tráfico entre clientes y servidores.</span><span class="sxs-lookup"><span data-stu-id="02cfa-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="02cfa-110">Si se usa un proxy o un equilibrador de carga, la autenticación de certificado solo funciona si el proxy o el equilibrador de carga:</span><span class="sxs-lookup"><span data-stu-id="02cfa-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="02cfa-111">Controla la autenticación.</span><span class="sxs-lookup"><span data-stu-id="02cfa-111">Handles the authentication.</span></span>
* <span data-ttu-id="02cfa-112">Pasa la información de autenticación del usuario a la aplicación (por ejemplo, en un encabezado de solicitud), que actúa en la información de autenticación.</span><span class="sxs-lookup"><span data-stu-id="02cfa-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="02cfa-113">Una alternativa a la autenticación de certificados en entornos en los que se usan servidores proxy y equilibradores de carga es Active Directory Federated Services (ADFS) con OpenID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="02cfa-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="02cfa-114">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="02cfa-114">Get started</span></span>

<span data-ttu-id="02cfa-115">Adquiera un certificado HTTPS, aplíquelo y [Configure el host](#configure-your-host-to-require-certificates) para que requiera certificados.</span><span class="sxs-lookup"><span data-stu-id="02cfa-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="02cfa-116">En la aplicación Web, agregue una referencia al paquete de `Microsoft.AspNetCore.Authentication.Certificate`.</span><span class="sxs-lookup"><span data-stu-id="02cfa-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="02cfa-117">Después, en el método `Startup.ConfigureServices`, llame a `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` con sus opciones, proporcionando un delegado para que `OnCertificateValidated` realice cualquier validación complementaria en el certificado de cliente enviado con las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="02cfa-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="02cfa-118">Convierta esa información en un `ClaimsPrincipal` y establézcala en la propiedad `context.Principal`.</span><span class="sxs-lookup"><span data-stu-id="02cfa-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="02cfa-119">Si se produce un error en la autenticación, este controlador devuelve una respuesta `403 (Forbidden)` en lugar de un `401 (Unauthorized)`, como cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="02cfa-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="02cfa-120">La razón es que la autenticación debe realizarse durante la conexión TLS inicial.</span><span class="sxs-lookup"><span data-stu-id="02cfa-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="02cfa-121">En el momento en que llega al controlador, es demasiado tarde.</span><span class="sxs-lookup"><span data-stu-id="02cfa-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="02cfa-122">No hay ninguna manera de actualizar la conexión de una conexión anónima a una con un certificado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="02cfa-123">Agregue también `app.UseAuthentication();` en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="02cfa-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="02cfa-124">De lo contrario, HttpContext. user no se establecerá en `ClaimsPrincipal` crea a partir del certificado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="02cfa-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="02cfa-125">For example:</span></span>

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

<span data-ttu-id="02cfa-126">En el ejemplo anterior se muestra la manera predeterminada de agregar la autenticación de certificado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="02cfa-127">El controlador crea una entidad de seguridad de usuario mediante las propiedades comunes del certificado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="02cfa-128">Configurar la validación de certificados</span><span class="sxs-lookup"><span data-stu-id="02cfa-128">Configure certificate validation</span></span>

<span data-ttu-id="02cfa-129">El controlador de `CertificateAuthenticationOptions` tiene algunas validaciones integradas que son las validaciones mínimas que se deben realizar en un certificado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="02cfa-130">Cada una de estas opciones está habilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="02cfa-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="02cfa-131">AllowedCertificateTypes = encadenado, SelfSigned o todos (encadenado | SelfSigned</span><span class="sxs-lookup"><span data-stu-id="02cfa-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="02cfa-132">Esta comprobación valida que solo se permite el tipo de certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="02cfa-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="02cfa-133">ValidateCertificateUse</span></span>

<span data-ttu-id="02cfa-134">Esta comprobación valida que el certificado presentado por el cliente tiene el uso mejorado de clave (EKU) de autenticación del cliente o no tiene ningún EKU.</span><span class="sxs-lookup"><span data-stu-id="02cfa-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="02cfa-135">Como indican las especificaciones, si no se especifica ningún EKU, se considera que todos los EKU son válidos.</span><span class="sxs-lookup"><span data-stu-id="02cfa-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="02cfa-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="02cfa-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="02cfa-137">Esta comprobación valida que el certificado está dentro de su período de validez.</span><span class="sxs-lookup"><span data-stu-id="02cfa-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="02cfa-138">En cada solicitud, el controlador garantiza que un certificado que era válido cuando se presentó no ha expirado durante su sesión actual.</span><span class="sxs-lookup"><span data-stu-id="02cfa-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="02cfa-139">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="02cfa-139">RevocationFlag</span></span>

<span data-ttu-id="02cfa-140">Marca que especifica en qué certificados de la cadena se comprueba la revocación.</span><span class="sxs-lookup"><span data-stu-id="02cfa-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="02cfa-141">Las comprobaciones de revocación solo se realizan cuando el certificado está encadenado a un certificado raíz.</span><span class="sxs-lookup"><span data-stu-id="02cfa-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="02cfa-142">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="02cfa-142">RevocationMode</span></span>

<span data-ttu-id="02cfa-143">Marca que especifica cómo se realizan las comprobaciones de revocación.</span><span class="sxs-lookup"><span data-stu-id="02cfa-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="02cfa-144">Si se especifica una comprobación en línea, se puede producir un retraso largo mientras se Contacta con la entidad de certificación.</span><span class="sxs-lookup"><span data-stu-id="02cfa-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="02cfa-145">Las comprobaciones de revocación solo se realizan cuando el certificado está encadenado a un certificado raíz.</span><span class="sxs-lookup"><span data-stu-id="02cfa-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="02cfa-146">¿Puedo configurar mi aplicación para requerir un certificado solo en determinadas rutas?</span><span class="sxs-lookup"><span data-stu-id="02cfa-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="02cfa-147">Esto no es posible.</span><span class="sxs-lookup"><span data-stu-id="02cfa-147">This isn't possible.</span></span> <span data-ttu-id="02cfa-148">Recuerde que el intercambio de certificados se realiza al inicio de la conversación HTTPS, se realiza en el servidor antes de que se reciba la primera solicitud en esa conexión, por lo que no es posible el ámbito en función de los campos de solicitud.</span><span class="sxs-lookup"><span data-stu-id="02cfa-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="02cfa-149">Eventos de controlador</span><span class="sxs-lookup"><span data-stu-id="02cfa-149">Handler events</span></span>

<span data-ttu-id="02cfa-150">El controlador tiene dos eventos:</span><span class="sxs-lookup"><span data-stu-id="02cfa-150">The handler has two events:</span></span>

* <span data-ttu-id="02cfa-151">`OnAuthenticationFailed` &ndash; llama si se produce una excepción durante la autenticación y le permite reaccionar.</span><span class="sxs-lookup"><span data-stu-id="02cfa-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="02cfa-152">`OnCertificateValidated` &ndash; llama después de validar el certificado, se ha aprobado la validación y se ha creado una entidad de seguridad predeterminada.</span><span class="sxs-lookup"><span data-stu-id="02cfa-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="02cfa-153">Este evento le permite realizar su propia validación y aumentar o reemplazar la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="02cfa-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="02cfa-154">Por ejemplo, se incluyen:</span><span class="sxs-lookup"><span data-stu-id="02cfa-154">For examples include:</span></span>
  * <span data-ttu-id="02cfa-155">Determinar si los servicios conocen el certificado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="02cfa-156">Construir su propia entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="02cfa-156">Constructing your own principal.</span></span> <span data-ttu-id="02cfa-157">Considere el ejemplo siguiente de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="02cfa-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="02cfa-158">Si encuentra que el certificado de entrada no cumple su validación adicional, llame a `context.Fail("failure reason")` con un motivo del error.</span><span class="sxs-lookup"><span data-stu-id="02cfa-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="02cfa-159">En el caso de una funcionalidad real, es probable que quiera llamar a un servicio registrado en la inserción de dependencias que se conecta a una base de datos u otro tipo de almacén de usuario.</span><span class="sxs-lookup"><span data-stu-id="02cfa-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="02cfa-160">Obtenga acceso a su servicio mediante el contexto que se pasa al delegado.</span><span class="sxs-lookup"><span data-stu-id="02cfa-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="02cfa-161">Considere el ejemplo siguiente de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="02cfa-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="02cfa-162">Conceptualmente, la validación del certificado es un problema de autorización.</span><span class="sxs-lookup"><span data-stu-id="02cfa-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="02cfa-163">La adición de una comprobación, por ejemplo, de un emisor o huella digital en una directiva de autorización, en lugar de en `OnCertificateValidated`, es absolutamente aceptable.</span><span class="sxs-lookup"><span data-stu-id="02cfa-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="02cfa-164">Configuración del host para requerir certificados</span><span class="sxs-lookup"><span data-stu-id="02cfa-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="02cfa-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="02cfa-165">Kestrel</span></span>

<span data-ttu-id="02cfa-166">En *Program.CS*, configure Kestrel como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="02cfa-166">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp

public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}

public static IHostBuilder CreateHostBuilder(string[] args)
{
    return Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                    webBuilder.ConfigureKestrel(o =>
                    {
                        o.ConfigureHttpsDefaults(o => o.ClientCertificateMode = ClientCertificateMode.RequireCertificate);
                    });
                });
}
```

### <a name="iis"></a><span data-ttu-id="02cfa-167">IIS</span><span class="sxs-lookup"><span data-stu-id="02cfa-167">IIS</span></span>

<span data-ttu-id="02cfa-168">Complete los pasos siguientes en el administrador de IIS:</span><span class="sxs-lookup"><span data-stu-id="02cfa-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="02cfa-169">Seleccione su sitio en la pestaña **conexiones** .</span><span class="sxs-lookup"><span data-stu-id="02cfa-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="02cfa-170">Haga doble clic en la opción **configuración de SSL** en la ventana **ver características** .</span><span class="sxs-lookup"><span data-stu-id="02cfa-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="02cfa-171">Active la casilla **requerir SSL** y seleccione el botón de radio **requerir** en la sección **certificados de cliente** .</span><span class="sxs-lookup"><span data-stu-id="02cfa-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Configuración de certificados de cliente en IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="02cfa-173">Azure y proxies web personalizados</span><span class="sxs-lookup"><span data-stu-id="02cfa-173">Azure and custom web proxies</span></span>

<span data-ttu-id="02cfa-174">Consulte la [documentación de host e implementación](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) para obtener información sobre cómo configurar el middleware de reenvío de certificados.</span><span class="sxs-lookup"><span data-stu-id="02cfa-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
