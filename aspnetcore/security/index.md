---
title: Seguridad
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a><span data-ttu-id="29f85-103">Seguridad</span><span class="sxs-lookup"><span data-stu-id="29f85-103">Security</span></span>

*   [<span data-ttu-id="29f85-104">Autenticación</span><span class="sxs-lookup"><span data-stu-id="29f85-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="29f85-105">Introducción a Identity</span><span class="sxs-lookup"><span data-stu-id="29f85-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="29f85-106">Habilitación de la autenticación con Facebook, Google y otros proveedores externos</span><span class="sxs-lookup"><span data-stu-id="29f85-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="29f85-107">Configuración de la autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="29f85-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="29f85-108">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="29f85-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="29f85-109">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="29f85-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="29f85-110">Uso de la autenticación de cookies sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="29f85-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="29f85-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="29f85-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="29f85-112">Integración de Azure AD en una aplicación web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29f85-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="29f85-113">Llamada a una API Web de ASP.NET Core desde una aplicación de WPF con Azure AD</span><span class="sxs-lookup"><span data-stu-id="29f85-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="29f85-114">Llamada a una API Web en una aplicación web de ASP.NET Core con Azure AD</span><span class="sxs-lookup"><span data-stu-id="29f85-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="29f85-115">Una aplicación web de ASP.NET Core con Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="29f85-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="29f85-116">Protección de aplicaciones de ASP.NET Core con IdentityServer4</span><span class="sxs-lookup"><span data-stu-id="29f85-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="29f85-117">Autorización</span><span class="sxs-lookup"><span data-stu-id="29f85-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="29f85-118">Introducción</span><span class="sxs-lookup"><span data-stu-id="29f85-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="29f85-119">Creación de una aplicación con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="29f85-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="29f85-120">Autorización simple</span><span class="sxs-lookup"><span data-stu-id="29f85-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="29f85-121">Autorización basada en roles</span><span class="sxs-lookup"><span data-stu-id="29f85-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="29f85-122">Autorización basada en notificaciones</span><span class="sxs-lookup"><span data-stu-id="29f85-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="29f85-123">Autorización personalizada basada en directivas</span><span class="sxs-lookup"><span data-stu-id="29f85-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="29f85-124">Inserción de dependencias en controladores de requisitos</span><span class="sxs-lookup"><span data-stu-id="29f85-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="29f85-125">Autorización basada en recursos</span><span class="sxs-lookup"><span data-stu-id="29f85-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="29f85-126">Autorización basada en vistas</span><span class="sxs-lookup"><span data-stu-id="29f85-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="29f85-127">Limitación de identidad por esquema</span><span class="sxs-lookup"><span data-stu-id="29f85-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="29f85-128">Protección de datos</span><span class="sxs-lookup"><span data-stu-id="29f85-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="29f85-129">Introducción a la protección de datos</span><span class="sxs-lookup"><span data-stu-id="29f85-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="29f85-130">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="29f85-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="29f85-131">API de consumidor</span><span class="sxs-lookup"><span data-stu-id="29f85-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="29f85-132">Información general sobre las API de consumidor</span><span class="sxs-lookup"><span data-stu-id="29f85-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="29f85-133">Cadenas de propósito</span><span class="sxs-lookup"><span data-stu-id="29f85-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="29f85-134">Jerarquía de propósito y configuración multiempresa</span><span class="sxs-lookup"><span data-stu-id="29f85-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="29f85-135">Hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="29f85-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="29f85-136">Limitación de la duración de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="29f85-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="29f85-137">Desprotección de cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="29f85-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="29f85-138">Configuración</span><span class="sxs-lookup"><span data-stu-id="29f85-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="29f85-139">Configuración de la protección de datos</span><span class="sxs-lookup"><span data-stu-id="29f85-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="29f85-140">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="29f85-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="29f85-141">Directiva para toda la máquina</span><span class="sxs-lookup"><span data-stu-id="29f85-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="29f85-142">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="29f85-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="29f85-143">API de extensibilidad</span><span class="sxs-lookup"><span data-stu-id="29f85-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="29f85-144">Extensibilidad de criptografía de núcleo</span><span class="sxs-lookup"><span data-stu-id="29f85-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="29f85-145">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="29f85-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="29f85-146">Otras API</span><span class="sxs-lookup"><span data-stu-id="29f85-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="29f85-147">Implementación</span><span class="sxs-lookup"><span data-stu-id="29f85-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="29f85-148">Detalles de cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="29f85-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="29f85-149">Derivación de subclave y cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="29f85-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="29f85-150">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="29f85-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="29f85-151">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="29f85-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="29f85-152">Proveedores de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="29f85-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="29f85-153">Cifrado de claves en reposo</span><span class="sxs-lookup"><span data-stu-id="29f85-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="29f85-154">Inmutabilidad de claves y cambio de configuración</span><span class="sxs-lookup"><span data-stu-id="29f85-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="29f85-155">Formato de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="29f85-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="29f85-156">Proveedores de protección de datos efímeros</span><span class="sxs-lookup"><span data-stu-id="29f85-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="29f85-157">Compatibilidad</span><span class="sxs-lookup"><span data-stu-id="29f85-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="29f85-158">Uso compartido de cookies entre aplicaciones</span><span class="sxs-lookup"><span data-stu-id="29f85-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="29f85-159">Reemplazo de <machineKey> en ASP.NET</span><span class="sxs-lookup"><span data-stu-id="29f85-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="29f85-160">Creación de una aplicación con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="29f85-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="29f85-161">Almacenamiento seguro de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="29f85-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="29f85-162">Proveedor de configuración de Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="29f85-162">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="29f85-163">Aplicación de SSL</span><span class="sxs-lookup"><span data-stu-id="29f85-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="29f85-164">Configuración de HTTPS para el desarrollo</span><span class="sxs-lookup"><span data-stu-id="29f85-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="29f85-165">Prevención de ataques de falsificación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="29f85-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="29f85-166">Prevención de ataques de redireccionamiento abierto</span><span class="sxs-lookup"><span data-stu-id="29f85-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="29f85-167">Prevención de scripting entre sitios</span><span class="sxs-lookup"><span data-stu-id="29f85-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="29f85-168">Habilitación de solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="29f85-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
