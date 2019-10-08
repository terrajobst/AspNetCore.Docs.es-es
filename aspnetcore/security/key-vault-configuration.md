---
title: Azure Key Vault proveedor de configuración en ASP.NET Core
author: guardrex
description: Aprenda a usar el proveedor de configuración de Azure Key Vault para configurar una aplicación mediante pares de nombre y valor cargados en tiempo de ejecución.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/key-vault-configuration
ms.openlocfilehash: cc3894df4df169d941f54ef3dfad5d3e6f798aad
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007408"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="be158-103">Azure Key Vault proveedor de configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be158-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="be158-104">Por [Luke Latham](https://github.com/guardrex) y [Andrew Stanton-enfermera](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="be158-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="be158-105">En este documento se explica cómo usar el proveedor de configuración de [key Vault de Microsoft Azure](https://azure.microsoft.com/services/key-vault/) para cargar los valores de configuración de la aplicación de Azure Key Vault secretos.</span><span class="sxs-lookup"><span data-stu-id="be158-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="be158-106">Azure Key Vault es un servicio basado en la nube que ayuda a proteger las claves criptográficas y los secretos que usan las aplicaciones y los servicios.</span><span class="sxs-lookup"><span data-stu-id="be158-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="be158-107">Entre los escenarios comunes para usar Azure Key Vault con ASP.NET Core aplicaciones se incluyen:</span><span class="sxs-lookup"><span data-stu-id="be158-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="be158-108">Controlar el acceso a los datos de configuración confidenciales.</span><span class="sxs-lookup"><span data-stu-id="be158-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="be158-109">Cumplir los requisitos de los módulos de seguridad de hardware (HSM) de FIPS 140-2 nivel 2 validados al almacenar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="be158-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="be158-110">Este escenario está disponible para las aplicaciones que tienen como destino ASP.NET Core 2,1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="be158-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="be158-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="be158-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="be158-112">Paquetes</span><span class="sxs-lookup"><span data-stu-id="be158-112">Packages</span></span>

<span data-ttu-id="be158-113">Para usar el proveedor de configuración de Azure Key Vault, agregue una referencia de paquete al paquete [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="be158-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="be158-114">Para adoptar el escenario [identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) , agregue una referencia de paquete al paquete [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .</span><span class="sxs-lookup"><span data-stu-id="be158-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="be158-115">En el momento de escribir este documento, la versión estable más reciente de `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, proporciona compatibilidad con [identidades administradas asignadas por el sistema](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span><span class="sxs-lookup"><span data-stu-id="be158-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="be158-116">La compatibilidad con *identidades administradas asignadas* por el usuario está disponible en el paquete `1.2.0-preview2`.</span><span class="sxs-lookup"><span data-stu-id="be158-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="be158-117">En este tema se muestra el uso de identidades administradas por el sistema y la aplicación de ejemplo proporcionada usa la versión `1.0.3` del paquete `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="be158-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="be158-118">Aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="be158-118">Sample app</span></span>

<span data-ttu-id="be158-119">La aplicación de ejemplo se ejecuta en uno de los dos modos determinados por la instrucción `#define` en la parte superior del archivo *Program.CS* :</span><span class="sxs-lookup"><span data-stu-id="be158-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="be158-120">`Certificate` &ndash; muestra el uso de un identificador de cliente Azure Key Vault y un certificado X. 509 para tener acceso a los secretos almacenados en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="be158-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="be158-121">Esta versión del ejemplo se puede ejecutar desde cualquier ubicación, implementada en Azure App Service o cualquier host capaz de servir a una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="be158-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="be158-122">`Managed` &ndash; muestra cómo usar [identidades administradas para los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview) para autenticar la aplicación en Azure Key Vault con Azure ad autenticación sin credenciales almacenadas en el código o la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="be158-123">Al usar identidades administradas para autenticar, no se requieren un identificador de aplicación Azure AD y una contraseña (secreto de cliente).</span><span class="sxs-lookup"><span data-stu-id="be158-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="be158-124">La versión `Managed` del ejemplo debe implementarse en Azure.</span><span class="sxs-lookup"><span data-stu-id="be158-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="be158-125">Siga las instrucciones de la sección [uso de las identidades administradas para los recursos de Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="be158-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="be158-126">Para obtener más información sobre cómo configurar una aplicación de ejemplo mediante las directivas de preprocesador (`#define`), consulte <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="be158-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="be158-127">Almacenamiento de secretos en el entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="be158-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="be158-128">Establezca secretos localmente mediante la [herramienta de administrador de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="be158-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="be158-129">Cuando la aplicación de ejemplo se ejecuta en el equipo local en el entorno de desarrollo, los secretos se cargan desde el almacén del administrador secreto local.</span><span class="sxs-lookup"><span data-stu-id="be158-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="be158-130">La herramienta Administrador de secretos requiere una propiedad `<UserSecretsId>` en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="be158-131">Establezca el valor de propiedad (`{GUID}`) en cualquier GUID único:</span><span class="sxs-lookup"><span data-stu-id="be158-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="be158-132">Los secretos se crean como pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="be158-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="be158-133">Los valores jerárquicos (secciones de configuración) usan un `:` (dos puntos) como separador en ASP.NET Core nombres de clave de [configuración](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="be158-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="be158-134">El administrador de secretos se usa desde un shell de comandos abierto a la [raíz del contenido](xref:fundamentals/index#content-root)del proyecto, donde `{SECRET NAME}` es el nombre y `{SECRET VALUE}` es el valor:</span><span class="sxs-lookup"><span data-stu-id="be158-134">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="be158-135">Ejecute los siguientes comandos en un shell de comandos desde la [raíz del contenido](xref:fundamentals/index#content-root) del proyecto para establecer los secretos de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="be158-135">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="be158-136">Cuando estos secretos se almacenan en Azure Key Vault en el [almacenamiento Secret en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección, el sufijo `_dev` se cambia a `_prod`.</span><span class="sxs-lookup"><span data-stu-id="be158-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="be158-137">El sufijo proporciona una indicación visual en la salida de la aplicación que indica el origen de los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="be158-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="be158-138">Almacenamiento secreto en el entorno de producción con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="be158-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="be158-139">Las instrucciones proporcionadas por el @no__t 0Quickstart: Establecer y recuperar un secreto de Azure Key Vault mediante el tema CLI de Azure @ no__t-0 se resumen aquí para crear un Azure Key Vault y almacenar secretos usados por la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="be158-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="be158-140">Consulte el tema para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="be158-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="be158-141">Abra Azure Cloud Shell con cualquiera de los métodos siguientes en el [Azure portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="be158-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="be158-142">Seleccione **pruébelo** en la esquina superior derecha de un bloque de código.</span><span class="sxs-lookup"><span data-stu-id="be158-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="be158-143">Use la cadena de búsqueda "CLI de Azure" en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="be158-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="be158-144">Abra Cloud Shell en el explorador con el botón **iniciar Cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="be158-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="be158-145">Seleccione el botón **Cloud Shell** en el menú de la esquina superior derecha del Azure portal.</span><span class="sxs-lookup"><span data-stu-id="be158-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="be158-146">Para obtener más información, consulte [interfaz de la línea de comandos de Azure (CLI)](/cli/azure/) e [información general de Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="be158-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="be158-147">Si aún no está autenticado, inicie sesión con el comando `az login`.</span><span class="sxs-lookup"><span data-stu-id="be158-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="be158-148">Cree un grupo de recursos con el siguiente comando, donde `{RESOURCE GROUP NAME}` es el nombre del grupo de recursos para el nuevo grupo de recursos y `{LOCATION}` es la región de Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="be158-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="be158-149">Cree un almacén de claves en el grupo de recursos con el siguiente comando, donde `{KEY VAULT NAME}` es el nombre del nuevo almacén de claves y `{LOCATION}` es la región de Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="be158-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="be158-150">Cree secretos en el almacén de claves como pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="be158-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="be158-151">Azure Key Vault los nombres de los secretos se limitan a caracteres alfanuméricos y guiones.</span><span class="sxs-lookup"><span data-stu-id="be158-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="be158-152">Los valores jerárquicos (secciones de configuración) usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="be158-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="be158-153">Los dos puntos, que se suelen usar para delimitar una sección de una subclave en [ASP.net Core configuración](xref:fundamentals/configuration/index), no se permiten en los nombres de secreto del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="be158-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="be158-154">Por lo tanto, se usan dos guiones y se intercambian por dos puntos cuando se cargan los secretos en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="be158-155">Los siguientes secretos son para su uso con la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="be158-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="be158-156">Los valores incluyen un sufijo `_prod` para distinguirlos de los valores de sufijo `_dev` cargados en el entorno de desarrollo de los secretos de usuario.</span><span class="sxs-lookup"><span data-stu-id="be158-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="be158-157">Reemplace `{KEY VAULT NAME}` por el nombre del almacén de claves que creó en el paso anterior:</span><span class="sxs-lookup"><span data-stu-id="be158-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="be158-158">Usar el identificador de aplicación y el certificado X. 509 para aplicaciones no hospedadas en Azure</span><span class="sxs-lookup"><span data-stu-id="be158-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="be158-159">Configure Azure AD, Azure Key Vault y la aplicación para que use un identificador de aplicación Azure Active Directory y un certificado X. 509 para autenticarse en un almacén de claves **cuando la aplicación se hospeda fuera de Azure**.</span><span class="sxs-lookup"><span data-stu-id="be158-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="be158-160">Para obtener más información, vea [acerca de las claves, los secretos y los certificados](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="be158-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="be158-161">Aunque el uso de un identificador de aplicación y un certificado X. 509 es compatible con las aplicaciones hospedadas en Azure, se recomienda usar [identidades administradas para los recursos de Azure](#use-managed-identities-for-azure-resources) al hospedar una aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="be158-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="be158-162">Las identidades administradas no requieren el almacenamiento de un certificado en la aplicación o en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="be158-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="be158-163">La aplicación de ejemplo usa un identificador de aplicación y un certificado X. 509 cuando la instrucción `#define` en la parte superior del archivo *Program.CS* se establece en `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="be158-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="be158-164">Cree un certificado de archivo PKCS # 12 ( *. pfx*).</span><span class="sxs-lookup"><span data-stu-id="be158-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="be158-165">Las opciones para crear certificados incluyen [Makecert en Windows](/windows/desktop/seccrypto/makecert) y [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="be158-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="be158-166">Instale el certificado en el almacén de certificados personales del usuario actual.</span><span class="sxs-lookup"><span data-stu-id="be158-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="be158-167">Marcar la clave como exportable es opcional.</span><span class="sxs-lookup"><span data-stu-id="be158-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="be158-168">Anote la huella digital del certificado, que se usa más adelante en este proceso.</span><span class="sxs-lookup"><span data-stu-id="be158-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="be158-169">Exporte el certificado de archivo PKCS # 12 ( *. pfx*) como un certificado codificado en der ( *. cer*).</span><span class="sxs-lookup"><span data-stu-id="be158-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="be158-170">Registre la aplicación con Azure AD (**registros de aplicaciones**).</span><span class="sxs-lookup"><span data-stu-id="be158-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="be158-171">Cargue el certificado codificado en DER ( *. cer*) en Azure ad:</span><span class="sxs-lookup"><span data-stu-id="be158-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="be158-172">Seleccione la aplicación en Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be158-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="be158-173">Vaya a **certificados & Secrets**.</span><span class="sxs-lookup"><span data-stu-id="be158-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="be158-174">Seleccione **upload Certificate (cargar certificado** ) para cargar el certificado, que contiene la clave pública.</span><span class="sxs-lookup"><span data-stu-id="be158-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="be158-175">Un certificado *. cer*, *. pem*o *. CRT* es aceptable.</span><span class="sxs-lookup"><span data-stu-id="be158-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="be158-176">Almacene el nombre del almacén de claves, el identificador de la aplicación y la huella digital del certificado en el archivo *appSettings. JSON* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="be158-177">Vaya a **almacenes de claves** en el Azure portal.</span><span class="sxs-lookup"><span data-stu-id="be158-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="be158-178">Seleccione el almacén de claves que creó en el [almacenamiento de secretos en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección.</span><span class="sxs-lookup"><span data-stu-id="be158-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="be158-179">Seleccione **directivas de acceso**.</span><span class="sxs-lookup"><span data-stu-id="be158-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="be158-180">Seleccione **Agregar nuevo**.</span><span class="sxs-lookup"><span data-stu-id="be158-180">Select **Add new**.</span></span>
1. <span data-ttu-id="be158-181">Seleccione **seleccionar entidad** de seguridad y seleccione la aplicación registrada por nombre.</span><span class="sxs-lookup"><span data-stu-id="be158-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="be158-182">Seleccione el botón **seleccionar** .</span><span class="sxs-lookup"><span data-stu-id="be158-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="be158-183">Abra los **permisos de secreto** y proporcione la aplicación con los permisos **Get** y **List** .</span><span class="sxs-lookup"><span data-stu-id="be158-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="be158-184">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="be158-184">Select **OK**.</span></span>
1. <span data-ttu-id="be158-185">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="be158-185">Select **Save**.</span></span>
1. <span data-ttu-id="be158-186">Implemente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-186">Deploy the app.</span></span>

<span data-ttu-id="be158-187">La aplicación de ejemplo `Certificate` obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el secreto:</span><span class="sxs-lookup"><span data-stu-id="be158-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="be158-188">Valores no jerárquicos: El valor de `SecretName` se obtiene con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="be158-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="be158-189">Valores jerárquicos (secciones): Use la notación `:` (dos puntos) o el método de extensión `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="be158-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="be158-190">Use cualquiera de estos métodos para obtener el valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="be158-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="be158-191">El certificado X. 509 se administra mediante el sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="be158-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="be158-192">La aplicación llama a `AddAzureKeyVault` con los valores proporcionados por el archivo *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="be158-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="be158-193">Valores de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="be158-193">Example values:</span></span>

* <span data-ttu-id="be158-194">Nombre del almacén de claves: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="be158-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="be158-195">IDENTIFICADOR de la aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="be158-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="be158-196">Huella digital del certificado: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="be158-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="be158-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="be158-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="be158-198">Al ejecutar la aplicación, se muestran los valores de secreto cargados en una página web.</span><span class="sxs-lookup"><span data-stu-id="be158-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="be158-199">En el entorno de desarrollo, los valores secretos se cargan con el sufijo `_dev`.</span><span class="sxs-lookup"><span data-stu-id="be158-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="be158-200">En el entorno de producción, los valores se cargan con el sufijo `_prod`.</span><span class="sxs-lookup"><span data-stu-id="be158-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="be158-201">Uso de identidades administradas para los recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="be158-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="be158-202">**Una aplicación implementada en Azure** puede aprovechar las [identidades administradas de los recursos de Azure](/azure/active-directory/managed-identities-azure-resources/overview), lo que permite que la aplicación se autentique con Azure Key Vault mediante la autenticación de Azure ad sin credenciales (identificador de aplicación y contraseña/secreto de cliente). almacenado en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="be158-203">La aplicación de ejemplo usa identidades administradas para los recursos de Azure cuando la instrucción `#define` en la parte superior del archivo *Program.CS* se establece en `Managed`.</span><span class="sxs-lookup"><span data-stu-id="be158-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="be158-204">Escriba el nombre del almacén en el archivo *appSettings. JSON* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="be158-205">La aplicación de ejemplo no requiere un identificador de aplicación y una contraseña (secreto de cliente) cuando se establece en la versión `Managed`, por lo que puede omitir esas entradas de configuración.</span><span class="sxs-lookup"><span data-stu-id="be158-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="be158-206">La aplicación se implementa en Azure y Azure autentica la aplicación para acceder a Azure Key Vault solo mediante el nombre del almacén almacenado en el archivo *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="be158-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="be158-207">Implemente la aplicación de ejemplo en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="be158-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="be158-208">Una aplicación implementada en Azure App Service se registra automáticamente con Azure AD cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="be158-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="be158-209">Obtenga el identificador de objeto de la implementación para usarlo en el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="be158-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="be158-210">El identificador de objeto se muestra en el Azure Portal en el panel **identidad** del App Service.</span><span class="sxs-lookup"><span data-stu-id="be158-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="be158-211">Con CLI de Azure y el identificador de objeto de la aplicación, proporcione la aplicación con los permisos `list` y `get` para acceder al almacén de claves:</span><span class="sxs-lookup"><span data-stu-id="be158-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="be158-212">**Reinicie la aplicación** con CLI de Azure, PowerShell o el Azure portal.</span><span class="sxs-lookup"><span data-stu-id="be158-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="be158-213">La aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="be158-213">The sample app:</span></span>

* <span data-ttu-id="be158-214">Crea una instancia de la clase `AzureServiceTokenProvider` sin una cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="be158-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="be158-215">Cuando no se proporciona una cadena de conexión, el proveedor intenta obtener un token de acceso de identidades administradas para los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="be158-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="be158-216">Se crea un nuevo `KeyVaultClient` con la devolución de llamada del token de instancia `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="be158-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="be158-217">La instancia de `KeyVaultClient` se utiliza con una implementación predeterminada de `IKeyVaultSecretManager` que carga todos los valores secretos y reemplaza los dobles guiones (`--`) por dos puntos (`:`) en los nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="be158-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="be158-218">Al ejecutar la aplicación, se muestran los valores de secreto cargados en una página web.</span><span class="sxs-lookup"><span data-stu-id="be158-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="be158-219">En el entorno de desarrollo, los valores secretos tienen el sufijo `_dev` porque son proporcionados por los secretos del usuario.</span><span class="sxs-lookup"><span data-stu-id="be158-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="be158-220">En el entorno de producción, los valores se cargan con el sufijo `_prod` porque los proporciona Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="be158-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="be158-221">Si recibe un error `Access denied`, confirme que la aplicación está registrada con Azure AD y que proporciona acceso al almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="be158-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="be158-222">Confirme que ha reiniciado el servicio en Azure.</span><span class="sxs-lookup"><span data-stu-id="be158-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="be158-223">Usar un prefijo de nombre de clave</span><span class="sxs-lookup"><span data-stu-id="be158-223">Use a key name prefix</span></span>

<span data-ttu-id="be158-224">`AddAzureKeyVault` proporciona una sobrecarga que acepta una implementación de `IKeyVaultSecretManager`, lo que permite controlar cómo se convierten los secretos del almacén de claves en claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="be158-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="be158-225">Por ejemplo, puede implementar la interfaz para cargar valores de secreto basados en un valor de prefijo que se proporciona al inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="be158-226">Esto le permite, por ejemplo, cargar secretos en función de la versión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="be158-227">No use prefijos en secretos del almacén de claves para colocar secretos para varias aplicaciones en el mismo almacén de claves o para colocar secretos de entorno (por ejemplo, *desarrollo* frente a secretos de *producción* ) en el mismo almacén.</span><span class="sxs-lookup"><span data-stu-id="be158-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="be158-228">Se recomienda que las distintas aplicaciones y entornos de desarrollo y producción usen almacenes de claves independientes para aislar los entornos de aplicación para obtener el máximo nivel de seguridad.</span><span class="sxs-lookup"><span data-stu-id="be158-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="be158-229">En el siguiente ejemplo, se establece un secreto en el almacén de claves (y el uso de la herramienta de administrador de secretos para el entorno de desarrollo) para `5000-AppSecret` (no se permiten puntos en los nombres de secreto del almacén de claves).</span><span class="sxs-lookup"><span data-stu-id="be158-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="be158-230">Este secreto representa un secreto de aplicación para la versión 5.0.0.0 de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="be158-231">Para otra versión de la aplicación, 5.1.0.0, se agrega un secreto al almacén de claves (y con la herramienta de administración de secretos) para `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="be158-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="be158-232">Cada versión de la aplicación carga su valor de secreto con versión en su configuración como `AppSecret`, eliminando la versión mientras carga el secreto.</span><span class="sxs-lookup"><span data-stu-id="be158-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="be158-233">se llama a `AddAzureKeyVault` con un @no__t personalizado-1:</span><span class="sxs-lookup"><span data-stu-id="be158-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="be158-234">La implementación de `IKeyVaultSecretManager` reacciona a los prefijos de versión de los secretos para cargar el secreto adecuado en la configuración:</span><span class="sxs-lookup"><span data-stu-id="be158-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="be158-235">Un algoritmo de proveedor llama al método `Load`, que recorre en iteración los secretos del almacén para encontrar los que tienen el prefijo de versión.</span><span class="sxs-lookup"><span data-stu-id="be158-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="be158-236">Cuando se encuentra un prefijo de versión con `Load`, el algoritmo utiliza el método `GetKey` para devolver el nombre de la configuración del nombre de secreto.</span><span class="sxs-lookup"><span data-stu-id="be158-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="be158-237">Elimina el prefijo de versión del nombre del secreto y devuelve el resto del nombre de secreto para cargarlo en los pares de nombre y valor de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="be158-238">Cuando se implementa este enfoque:</span><span class="sxs-lookup"><span data-stu-id="be158-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="be158-239">La versión de la aplicación especificada en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="be158-240">En el ejemplo siguiente, la versión de la aplicación se establece en `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="be158-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="be158-241">Confirme que hay una propiedad `<UserSecretsId>` en el archivo de proyecto de la aplicación, donde `{GUID}` es un GUID proporcionado por el usuario:</span><span class="sxs-lookup"><span data-stu-id="be158-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="be158-242">Guarde los siguientes secretos localmente con la [herramienta de administración de secretos](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="be158-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="be158-243">Los secretos se guardan en Azure Key Vault mediante los siguientes comandos de CLI de Azure:</span><span class="sxs-lookup"><span data-stu-id="be158-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="be158-244">Cuando se ejecuta la aplicación, se cargan los secretos del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="be158-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="be158-245">El secreto de cadena para `5000-AppSecret` coincide con la versión de la aplicación especificada en el archivo de proyecto de la aplicación (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="be158-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="be158-246">La versión, `5000` (con el guión), se elimina del nombre de la clave.</span><span class="sxs-lookup"><span data-stu-id="be158-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="be158-247">En toda la aplicación, la lectura de la configuración con la clave `AppSecret` carga el valor del secreto.</span><span class="sxs-lookup"><span data-stu-id="be158-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="be158-248">Si se cambia la versión de la aplicación en el archivo de proyecto a `5.1.0.0` y se vuelve a ejecutar la aplicación, el valor de secreto devuelto es `5.1.0.0_secret_value_dev` en el entorno de desarrollo y @no__t 2 en producción.</span><span class="sxs-lookup"><span data-stu-id="be158-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="be158-249">También puede proporcionar su propia implementación `KeyVaultClient` a `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="be158-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="be158-250">Un cliente personalizado permite compartir una única instancia del cliente a través de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="be158-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="be158-251">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="be158-251">Bind an array to a class</span></span>

<span data-ttu-id="be158-252">El proveedor es capaz de leer valores de configuración en una matriz para enlazar a una matriz POCO.</span><span class="sxs-lookup"><span data-stu-id="be158-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="be158-253">Al leer desde un origen de configuración que permite que las claves contengan separadores de dos puntos (`:`), se usa un segmento de clave numérica para distinguir las claves que componen una matriz (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="be158-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="be158-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="be158-254">`:{n}:`).</span></span> <span data-ttu-id="be158-255">Para obtener más información, vea [Configuration: Enlazar una matriz a una clase @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="be158-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="be158-256">Azure Key Vault teclas no pueden usar un signo de dos puntos como separador.</span><span class="sxs-lookup"><span data-stu-id="be158-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="be158-257">El enfoque que se describe en este tema utiliza los guiones dobles (`--`) como separador de valores jerárquicos (secciones).</span><span class="sxs-lookup"><span data-stu-id="be158-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="be158-258">Las claves de matriz se almacenan en Azure Key Vault con dobles guiones y segmentos de clave numérica (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="be158-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="be158-259">Examine la siguiente configuración de proveedor de registro de [Serilog](https://serilog.net/) proporcionada por un archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="be158-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="be158-260">Hay dos literales de objeto definidos en la matriz `WriteTo` que reflejan dos *receptores*Serilog, que describen los destinos para el registro de la salida:</span><span class="sxs-lookup"><span data-stu-id="be158-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="be158-261">La configuración que se muestra en el archivo JSON anterior se almacena en Azure Key Vault mediante la notación de doble guión (`--`) y los segmentos numéricos:</span><span class="sxs-lookup"><span data-stu-id="be158-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="be158-262">Key</span><span class="sxs-lookup"><span data-stu-id="be158-262">Key</span></span> | <span data-ttu-id="be158-263">Valor</span><span class="sxs-lookup"><span data-stu-id="be158-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="be158-264">Volver a cargar los secretos</span><span class="sxs-lookup"><span data-stu-id="be158-264">Reload secrets</span></span>

<span data-ttu-id="be158-265">Los secretos se almacenan en caché hasta que se llama a `IConfigurationRoot.Reload()`.</span><span class="sxs-lookup"><span data-stu-id="be158-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="be158-266">La aplicación no respeta los secretos expirados, deshabilitados y actualizados en el almacén de claves hasta que se ejecute `Reload`.</span><span class="sxs-lookup"><span data-stu-id="be158-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="be158-267">Secretos deshabilitados y expirados</span><span class="sxs-lookup"><span data-stu-id="be158-267">Disabled and expired secrets</span></span>

<span data-ttu-id="be158-268">Los secretos deshabilitados y expirados inician una `KeyVaultClientException` en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="be158-268">Disabled and expired secrets throw a `KeyVaultClientException` at runtime.</span></span> <span data-ttu-id="be158-269">Para evitar que se inicie la aplicación, proporcione la configuración mediante un proveedor de configuración diferente o actualice el secreto deshabilitado o expirado.</span><span class="sxs-lookup"><span data-stu-id="be158-269">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="be158-270">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="be158-270">Troubleshoot</span></span>

<span data-ttu-id="be158-271">Cuando la aplicación no carga la configuración mediante el proveedor, se escribe un mensaje de error en la [infraestructura de registro de ASP.net Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="be158-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="be158-272">Las siguientes condiciones impedirán que se cargue la configuración:</span><span class="sxs-lookup"><span data-stu-id="be158-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="be158-273">La aplicación o el certificado no están configurados correctamente en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="be158-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="be158-274">El almacén de claves no existe en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="be158-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="be158-275">La aplicación no está autorizada para acceder al almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="be158-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="be158-276">La Directiva de acceso no incluye los permisos `Get` y `List`.</span><span class="sxs-lookup"><span data-stu-id="be158-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="be158-277">En el almacén de claves, los datos de configuración (par nombre-valor) tienen un nombre incorrecto, faltan, están deshabilitados o han expirado.</span><span class="sxs-lookup"><span data-stu-id="be158-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="be158-278">La aplicación tiene un nombre de almacén de claves incorrecto (`KeyVaultName`), Azure AD ID. de aplicación (`AzureADApplicationId`) o Azure AD huella digital del certificado (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="be158-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="be158-279">La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.</span><span class="sxs-lookup"><span data-stu-id="be158-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be158-280">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="be158-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <span data-ttu-id="be158-281">[Microsoft Azure: Key Vault @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="be158-281">[Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)</span></span>
* <span data-ttu-id="be158-282">[Microsoft Azure: Key Vault documentación @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="be158-282">[Microsoft Azure: Key Vault Documentation](/azure/key-vault/)</span></span>
* [<span data-ttu-id="be158-283">Cómo generar y transferir claves protegidas con HSM para Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="be158-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="be158-284">Clase KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="be158-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* <span data-ttu-id="be158-285">[Inicio rápido: Establecimiento y recuperación de un secreto desde Azure Key Vault mediante una aplicación Web de .NET @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="be158-285">[Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app](/azure/key-vault/quick-create-net)</span></span>
* <span data-ttu-id="be158-286">[Tutorial: Cómo usar Azure Key Vault con máquinas virtuales Windows de Azure en. NET @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="be158-286">[Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET](/azure/key-vault/tutorial-net-windows-virtual-machine)</span></span>
