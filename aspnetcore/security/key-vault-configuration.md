---
title: Proveedor de configuración de almacén de claves de Azure en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar el proveedor de configuración de Azure Key Vault para configurar una aplicación mediante pares de nombre-valor que se cargan en tiempo de ejecución.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 8e40c8308a692731e71fb8ebebfc64e606874290
ms.sourcegitcommit: 98e9c7187772d4ddefe6d8e85d0d206749dbd2ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55737660"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="59f93-103">Proveedor de configuración de almacén de claves de Azure en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59f93-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="59f93-104">Por [Halter](https://github.com/guardrex) y [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="59f93-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="59f93-105">Este documento explica cómo usar el [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) proveedor de configuración para cargar los valores de configuración de aplicación de secretos de Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="59f93-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="59f93-106">Azure Key Vault es un servicio basado en la nube que ayuda a proteger claves criptográficas y secretos usados por aplicaciones y servicios.</span><span class="sxs-lookup"><span data-stu-id="59f93-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="59f93-107">Escenarios comunes para usar Azure Key Vault con aplicaciones ASP.NET Core son:</span><span class="sxs-lookup"><span data-stu-id="59f93-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="59f93-108">Controlar el acceso a datos confidenciales de la configuración.</span><span class="sxs-lookup"><span data-stu-id="59f93-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="59f93-109">Cumple el requisito de FIPS 140-2 nivel 2 había validado módulos de seguridad de Hardware (HSM) al almacenar datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="59f93-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="59f93-110">Este escenario está disponible para las aplicaciones que tienen como destino ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="59f93-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="59f93-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59f93-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="59f93-112">Paquetes</span><span class="sxs-lookup"><span data-stu-id="59f93-112">Packages</span></span>

<span data-ttu-id="59f93-113">Para usar el proveedor de configuración de Azure Key Vault, agregue una referencia de paquete para el [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paquete.</span><span class="sxs-lookup"><span data-stu-id="59f93-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="59f93-114">Para adoptar el escenario de Azure Managed Service Identity, agregue una referencia de paquete a la [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paquete.</span><span class="sxs-lookup"><span data-stu-id="59f93-114">To adopt the Azure Managed Service Identity scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="59f93-115">En el momento de escribir la última versión estable de `Microsoft.Azure.Services.AppAuthentication`, versión `1.0.3`, proporciona compatibilidad para [asignado por el sistema administra identidades](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="59f93-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="59f93-116">Compatibilidad con *asignada por el usuario administra las identidades* está disponible en el `1.0.2-preview` paquete.</span><span class="sxs-lookup"><span data-stu-id="59f93-116">Support for *user-assigned managed identities* is available in the `1.0.2-preview` package.</span></span> <span data-ttu-id="59f93-117">En este tema se muestra el uso de identidades administradas por el sistema y la aplicación de ejemplo proporcionada usa la versión `1.0.3` de la `Microsoft.Azure.Services.AppAuthentication` paquete.</span><span class="sxs-lookup"><span data-stu-id="59f93-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="59f93-118">Aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="59f93-118">Sample app</span></span>

<span data-ttu-id="59f93-119">La aplicación de ejemplo se ejecuta en cualquiera de los dos modos según la `#define` instrucción en la parte superior de la *Program.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="59f93-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="59f93-120">`Basic` &ndash; Muestra el uso de un identificador de la aplicación de Azure Key Vault y una contraseña (secreto de cliente) para tener acceso a los secretos almacenados en el almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="59f93-120">`Basic` &ndash; Demonstrates the use of an Azure Key Vault Application ID and Password (Client Secret) to access secrets stored in the key vault.</span></span> <span data-ttu-id="59f93-121">Implementar el `Basic` versión del ejemplo en cualquier host capaz de ofrecer servicio a una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59f93-121">Deploy the `Basic` version of the sample to any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="59f93-122">`Managed` &ndash; Muestra cómo usar Azure [Managed Service Identity (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) para autenticar la aplicación a Azure Key Vault con la autenticación de Azure AD sin credenciales almacenadas en el código o configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-122">`Managed` &ndash; Demonstrates how to use Azure's [Managed Service Identity (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="59f93-123">Cuando se usa MSI para autenticarse, un identificador de aplicación de Azure AD y una contraseña (secreto de cliente) no se requieren.</span><span class="sxs-lookup"><span data-stu-id="59f93-123">When using MSI to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="59f93-124">El `Managed` versión del ejemplo debe implementarse en Azure.</span><span class="sxs-lookup"><span data-stu-id="59f93-124">The `Managed` version of the sample must be deployed to Azure.</span></span>

<span data-ttu-id="59f93-125">Para obtener más información sobre cómo configurar una aplicación de ejemplo con las directivas de preprocesador (`#define`), consulte <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="59f93-125">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="59f93-126">Almacenamiento de secreto en el entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="59f93-126">Secret storage in the Development environment</span></span>

<span data-ttu-id="59f93-127">Establecer secretos localmente mediante el [herramienta Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="59f93-127">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="59f93-128">Cuando se ejecuta la aplicación de ejemplo en el equipo local en el entorno de desarrollo, se cargan los secretos del almacén local del Administrador de secretos.</span><span class="sxs-lookup"><span data-stu-id="59f93-128">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="59f93-129">La herramienta Secret Manager requiere un `<UserSecretsId>` propiedad en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-129">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="59f93-130">Establezca el valor de propiedad (`{GUID}`) a cualquier GUID único:</span><span class="sxs-lookup"><span data-stu-id="59f93-130">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="59f93-131">Los secretos se crean como pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="59f93-131">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="59f93-132">Usan valores jerárquicos (las secciones de configuración) un `:` (dos puntos) como separador en [configuración de ASP.NET Core](xref:fundamentals/configuration/index) los nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="59f93-132">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="59f93-133">Se utiliza el Administrador de secretos desde un shell de comandos que se abrió a raíz del contenido del proyecto, donde `{SECRET NAME}` es el nombre y `{SECRET VALUE}` es el valor:</span><span class="sxs-lookup"><span data-stu-id="59f93-133">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="59f93-134">Ejecute los siguientes comandos en un shell de comandos desde la raíz del contenido del proyecto para establecer los secretos de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59f93-134">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="59f93-135">Cuando estos secretos se almacenan en Azure Key Vault en el [almacenamiento secreto en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección, el `_dev` sufijo se cambia a `_prod`.</span><span class="sxs-lookup"><span data-stu-id="59f93-135">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="59f93-136">El sufijo proporciona una indicación visual en la salida de la aplicación que indica el origen de los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="59f93-136">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="59f93-137">Almacenamiento de secreto en el entorno de producción con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="59f93-137">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="59f93-138">Las instrucciones proporcionadas por el [inicio rápido: Establecer y recuperar un secreto de Azure Key Vault mediante CLI de Azure](/azure/key-vault/quick-create-cli) tema se resumen a continuación para crear una instancia de Azure Key Vault y almacenar secretos usados por la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="59f93-138">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="59f93-139">Consulte el tema para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="59f93-139">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="59f93-140">Abrir shell en la nube de Azure mediante cualquiera de los métodos siguientes en el [portal Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="59f93-140">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="59f93-141">Seleccione **Pruébelo** en la esquina superior derecha de un bloque de código.</span><span class="sxs-lookup"><span data-stu-id="59f93-141">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="59f93-142">Utilice la cadena de búsqueda "CLI de Azure" en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="59f93-142">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="59f93-143">Abra Cloud Shell en el explorador con el **iniciar Cloud Shell** botón.</span><span class="sxs-lookup"><span data-stu-id="59f93-143">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="59f93-144">Seleccione el **Cloud Shell** botón en el menú de la esquina superior derecha de Azure portal.</span><span class="sxs-lookup"><span data-stu-id="59f93-144">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="59f93-145">Para obtener más información, consulte [interfaz de línea de comandos (CLI) de Azure](/cli/azure/) y [información general de Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="59f93-145">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="59f93-146">Si no están ya se ha autenticado, inicie sesión con la `az login` comando.</span><span class="sxs-lookup"><span data-stu-id="59f93-146">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="59f93-147">Cree un grupo de recursos con el comando siguiente, donde `{RESOURCE GROUP NAME}` es el nombre del grupo de recursos para el nuevo grupo de recursos y `{LOCATION}` es la región de Azure (centro de datos):</span><span class="sxs-lookup"><span data-stu-id="59f93-147">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="59f93-148">Crear un almacén de claves en el grupo de recursos con el comando siguiente, donde `{KEY VAULT NAME}` es el nombre para el nuevo almacén de claves y `{LOCATION}` es la región de Azure (centro de datos):</span><span class="sxs-lookup"><span data-stu-id="59f93-148">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="59f93-149">Cree los secretos del almacén de claves como pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="59f93-149">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="59f93-150">Nombres de secretos de Azure Key Vault se limitan a caracteres alfanuméricos y guiones.</span><span class="sxs-lookup"><span data-stu-id="59f93-150">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="59f93-151">Usan valores jerárquicos (las secciones de configuración) `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="59f93-151">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="59f93-152">Dos puntos, que normalmente se utilizan para delimitar una sección de una subclave en [configuración de ASP.NET Core](xref:fundamentals/configuration/index), no se permiten en los nombres de secretos del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="59f93-152">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="59f93-153">Por lo tanto, son usa dos guiones y se intercambian en dos puntos cuando se cargan los secretos en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-153">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="59f93-154">Los secretos siguientes son para su uso con la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="59f93-154">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="59f93-155">Los valores incluyen un `_prod` sufijo para distinguirlos de los `_dev` sufijo valores cargados en el entorno de desarrollo de secretos del usuario.</span><span class="sxs-lookup"><span data-stu-id="59f93-155">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="59f93-156">Reemplace `{KEY VAULT NAME}` con el nombre del almacén de claves que creó en el paso anterior:</span><span class="sxs-lookup"><span data-stu-id="59f93-156">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret"></a><span data-ttu-id="59f93-157">Use el identificador de la aplicación y secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="59f93-157">Use Application ID and Client Secret</span></span>

<span data-ttu-id="59f93-158">Configurar Azure AD, Azure Key Vault y la aplicación para usar un identificador de la aplicación y una contraseña (secreto de cliente) para autenticarse en un almacén de claves cuando la aplicación se hospeda fuera de Azure.</span><span class="sxs-lookup"><span data-stu-id="59f93-158">Configure Azure AD, Azure Key Vault, and the app to use an Application ID and Password (Client Secret) to authenticate to a key vault when the app is hosted outside of Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="59f93-159">Aunque se admite el uso de un identificador de la aplicación y una contraseña (secreto de cliente) para las aplicaciones hospedadas en Azure, se recomienda usar la [Managed Service Identity (MSI) proveedor](#use-the-managed-service-identity-msi-provider) al hospedar una aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="59f93-159">Although using an Application ID and Password (Client Secret) is supported for apps hosted in Azure, we recommend using the [Managed Service Identity (MSI) Provider](#use-the-managed-service-identity-msi-provider) when hosting an app in Azure.</span></span> <span data-ttu-id="59f93-160">MSI no requiere almacenar las credenciales de la aplicación o su configuración, por lo que se considera como un enfoque suele ser más seguro.</span><span class="sxs-lookup"><span data-stu-id="59f93-160">MSI doesn't require storing credentials in the app or its configuration, so it's regarded as a generally safer approach.</span></span>

<span data-ttu-id="59f93-161">La aplicación de ejemplo usa un identificador de la aplicación y una contraseña (secreto de cliente) cuando el `#define` instrucción en la parte superior de la *Program.cs* archivo se establece en `Basic`.</span><span class="sxs-lookup"><span data-stu-id="59f93-161">The sample app uses an Application ID and Password (Client Secret) when the `#define` statement at the top of the *Program.cs* file is set to `Basic`.</span></span>

1. <span data-ttu-id="59f93-162">Registrar la aplicación con Azure AD y establecer una contraseña (secreto de cliente) para la identidad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-162">Register the app with Azure AD and establish a Password (Client Secret) for the app's identity.</span></span>
1. <span data-ttu-id="59f93-163">Store el nombre del almacén de claves, Id. de aplicación y secreto de cliente/contraseña en la aplicación *appsettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="59f93-163">Store the key vault name, Application ID, and Password/Client Secret in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="59f93-164">Vaya a **los almacenes de claves** en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="59f93-164">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="59f93-165">Seleccione el almacén de claves que creó en el [almacenamiento secreto en el entorno de producción con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sección.</span><span class="sxs-lookup"><span data-stu-id="59f93-165">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="59f93-166">Seleccione **las directivas de acceso**.</span><span class="sxs-lookup"><span data-stu-id="59f93-166">Select **Access policies**.</span></span>
1. <span data-ttu-id="59f93-167">Seleccione **Agregar nuevo**.</span><span class="sxs-lookup"><span data-stu-id="59f93-167">Select **Add new**.</span></span>
1. <span data-ttu-id="59f93-168">Seleccione **seleccione entidad** y seleccione la aplicación registrada por nombre.</span><span class="sxs-lookup"><span data-stu-id="59f93-168">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="59f93-169">Seleccione el **seleccione** botón.</span><span class="sxs-lookup"><span data-stu-id="59f93-169">Select the **Select** button.</span></span>
1. <span data-ttu-id="59f93-170">Abra **permisos de secretos** y proporcione la aplicación con **obtener** y **lista** permisos.</span><span class="sxs-lookup"><span data-stu-id="59f93-170">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="59f93-171">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="59f93-171">Select **OK**.</span></span>
1. <span data-ttu-id="59f93-172">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="59f93-172">Select **Save**.</span></span>
1. <span data-ttu-id="59f93-173">Implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-173">Deploy the app.</span></span>

<span data-ttu-id="59f93-174">El `Basic` aplicación de ejemplo obtiene sus valores de configuración de `IConfigurationRoot` con el mismo nombre que el nombre del secreto:</span><span class="sxs-lookup"><span data-stu-id="59f93-174">The `Basic` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="59f93-175">Valores que no son jerárquicos: El valor de `SecretName` se obtiene con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="59f93-175">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="59f93-176">Valores jerárquicos (secciones): Use `:` notación (dos puntos) o el `GetSection` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="59f93-176">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="59f93-177">Para obtener el valor de configuración, use cualquiera de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="59f93-177">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="59f93-178">Las llamadas de aplicación `AddAzureKeyVault` con los valores proporcionados por el *appsettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="59f93-178">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

<span data-ttu-id="59f93-179">Valores de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59f93-179">Example values:</span></span>

* <span data-ttu-id="59f93-180">Nombre de almacén de claves: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="59f93-180">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="59f93-181">Id. de aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="59f93-181">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="59f93-182">Contraseña: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="59f93-182">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="59f93-183">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="59f93-183">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="59f93-184">Al ejecutar la aplicación, una página Web muestra los valores de secreto cargados.</span><span class="sxs-lookup"><span data-stu-id="59f93-184">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="59f93-185">En el entorno de desarrollo, los valores de secreto de carga con el `_dev` sufijo.</span><span class="sxs-lookup"><span data-stu-id="59f93-185">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="59f93-186">En el entorno de producción, los valores de carga con el `_prod` sufijo.</span><span class="sxs-lookup"><span data-stu-id="59f93-186">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-the-managed-service-identity-msi-provider"></a><span data-ttu-id="59f93-187">Usar el proveedor de identidades (MSI) de servicio administrada</span><span class="sxs-lookup"><span data-stu-id="59f93-187">Use the Managed Service Identity (MSI) Provider</span></span>

<span data-ttu-id="59f93-188">Una aplicación implementada en Azure puede aprovechar las ventajas de Managed Service Identity (MSI), lo que permite que la aplicación para autenticarse con Azure Key Vault mediante la autenticación de Azure AD sin credenciales (identificador de la aplicación y secreto de cliente/contraseña) almacenados en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-188">An app deployed to Azure can take advantage of Managed Service Identity (MSI), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="59f93-189">La aplicación de ejemplo usa MSI cuando la `#define` instrucción en la parte superior de la *Program.cs* archivo se establece en `Managed`.</span><span class="sxs-lookup"><span data-stu-id="59f93-189">The sample app uses MSI when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="59f93-190">Escriba el nombre del almacén en la aplicación *appsettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="59f93-190">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="59f93-191">La aplicación de ejemplo no requiere un identificador de la aplicación y una contraseña (secreto de cliente) cuando se establece en el `Managed` versión, por lo que puede pasar por alto esas entradas de configuración.</span><span class="sxs-lookup"><span data-stu-id="59f93-191">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="59f93-192">La aplicación se implementa en Azure y Azure autentica la aplicación para acceder a Azure Key Vault usando solo el nombre del almacén se almacena en el *appsettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="59f93-192">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="59f93-193">Implemente la aplicación de ejemplo en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="59f93-193">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="59f93-194">Una aplicación implementada en Azure App Service se registra automáticamente con Azure AD cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="59f93-194">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="59f93-195">Obtener el identificador de objeto de la implementación para su uso en el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="59f93-195">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="59f93-196">El identificador de objeto se muestra en el portal de Azure en el **identidad** panel de App Service.</span><span class="sxs-lookup"><span data-stu-id="59f93-196">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="59f93-197">Uso de CLI de Azure y el Id. de objeto de la aplicación, proporcione la aplicación con `list` y `get` permisos para tener acceso al almacén de claves:</span><span class="sxs-lookup"><span data-stu-id="59f93-197">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="59f93-198">**Reinicie la aplicación** mediante Azure portal, PowerShell o CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="59f93-198">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="59f93-199">La aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59f93-199">The sample app:</span></span>

* <span data-ttu-id="59f93-200">Crea una instancia de la `AzureServiceTokenProvider` clase sin una cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="59f93-200">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="59f93-201">Cuando no se proporciona una cadena de conexión, el proveedor intenta obtener un token de acceso de MSI.</span><span class="sxs-lookup"><span data-stu-id="59f93-201">When a connection string isn't provided, the provider attempts to obtain an access token from MSI.</span></span>
* <span data-ttu-id="59f93-202">Un nuevo `KeyVaultClient` se crea con el `AzureServiceTokenProvider` devolución de llamada de token de instancia.</span><span class="sxs-lookup"><span data-stu-id="59f93-202">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="59f93-203">El `KeyVaultClient` instancia se utiliza con una implementación predeterminada de `IKeyVaultSecretManager` que carga todos los valores de secreto y reemplaza dos guiones (`--`) con dos puntos (`:`) en los nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="59f93-203">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="59f93-204">Al ejecutar la aplicación, una página Web muestra los valores de secreto cargados.</span><span class="sxs-lookup"><span data-stu-id="59f93-204">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="59f93-205">En el entorno de desarrollo, los valores de secreto tienen el `_dev` sufijo porque le proporciona los secretos del usuario.</span><span class="sxs-lookup"><span data-stu-id="59f93-205">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="59f93-206">En el entorno de producción, los valores de carga con el `_prod` sufijo porque le proporciona Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="59f93-206">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="59f93-207">Si recibe un `Access denied` error, confirme que la aplicación está registrada con Azure AD y proporciona acceso al almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="59f93-207">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="59f93-208">Confirme que ha reiniciado el servicio en Azure.</span><span class="sxs-lookup"><span data-stu-id="59f93-208">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="59f93-209">Utilizar un prefijo de nombre de clave</span><span class="sxs-lookup"><span data-stu-id="59f93-209">Use a key name prefix</span></span>

<span data-ttu-id="59f93-210">`AddAzureKeyVault` Proporciona una sobrecarga que acepta una implementación de `IKeyVaultSecretManager`, que le permite controlar los secretos del almacén clave se convierten en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="59f93-210">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="59f93-211">Por ejemplo, puede implementar la interfaz para cargar los valores de secreto según un valor de prefijo que se proporcione al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-211">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="59f93-212">Esto permite, por ejemplo, para cargar los secretos en función de la versión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-212">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="59f93-213">No utilizar prefijos en secretos del almacén de claves para colocar los secretos de varias aplicaciones en el mismo almacén de claves o colocar los secretos del entorno (por ejemplo, *desarrollo* frente a *producción* secretos) en la misma almacén.</span><span class="sxs-lookup"><span data-stu-id="59f93-213">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="59f93-214">Se recomienda que diferentes aplicaciones y entornos de desarrollo y producción usan almacenes de claves independientes para aislar los entornos de aplicación para el nivel más alto de seguridad.</span><span class="sxs-lookup"><span data-stu-id="59f93-214">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="59f93-215">En el ejemplo siguiente, se establece un secreto en la clave de almacén (y utilizar la herramienta Secret Manager para el entorno de desarrollo) para `5000-AppSecret` (no se permiten los períodos en los nombres de secretos del almacén de claves).</span><span class="sxs-lookup"><span data-stu-id="59f93-215">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="59f93-216">Este secreto representa un secreto de la aplicación para la versión 5.0.0.0 de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-216">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="59f93-217">Por otra versión de la aplicación, 5.1.0.0, se agrega un secreto a la clave de almacén (y utilizar la herramienta Secret Manager) para `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="59f93-217">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="59f93-218">Cada versión de la aplicación carga su valor secreto con control de versiones en su configuración como `AppSecret`, extracción, la versión cuando se cargue el secreto.</span><span class="sxs-lookup"><span data-stu-id="59f93-218">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="59f93-219">`AddAzureKeyVault` se llama con una personalizada `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="59f93-219">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="59f93-220">Proporcionan los valores de nombre de almacén de claves, el identificador de aplicación y contraseña (secreto de cliente) la *appsettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="59f93-220">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="59f93-221">Valores de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59f93-221">Example values:</span></span>

* <span data-ttu-id="59f93-222">Nombre de almacén de claves: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="59f93-222">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="59f93-223">Id. de aplicación: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="59f93-223">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="59f93-224">Contraseña: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="59f93-224">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="59f93-225">El `IKeyVaultSecretManager` reacciona ante los prefijos de la versión de secretos para cargar el secreto adecuado en la configuración de implementación:</span><span class="sxs-lookup"><span data-stu-id="59f93-225">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="59f93-226">El `Load` se llama al método mediante un algoritmo de proveedor que recorre en iteración los secretos del almacén para buscar las que tienen el prefijo de la versión.</span><span class="sxs-lookup"><span data-stu-id="59f93-226">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="59f93-227">Cuando se encuentra un prefijo de la versión con `Load`, el algoritmo utiliza el `GetKey` método para devolver el nombre de la configuración del nombre del secreto.</span><span class="sxs-lookup"><span data-stu-id="59f93-227">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="59f93-228">Quita el prefijo de la versión del nombre del secreto y devuelve el resto del nombre del secreto para la carga en la configuración de la aplicación de pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="59f93-228">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="59f93-229">Cuando se implementa este enfoque:</span><span class="sxs-lookup"><span data-stu-id="59f93-229">When this approach is implemented:</span></span>

1. <span data-ttu-id="59f93-230">Versión de la aplicación especificada en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-230">The app's version specified in the app's project file.</span></span> <span data-ttu-id="59f93-231">En el ejemplo siguiente, se establece la versión de la aplicación en `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="59f93-231">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="59f93-232">Confirme que un `<UserSecretsId>` propiedad está presente en el archivo de proyecto de la aplicación, donde `{GUID}` es un GUID proporcionado por el usuario:</span><span class="sxs-lookup"><span data-stu-id="59f93-232">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="59f93-233">Guardar los secretos siguientes localmente con el [herramienta Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="59f93-233">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="59f93-234">Los secretos se guardan en Azure Key Vault mediante los siguientes comandos de CLI de Azure:</span><span class="sxs-lookup"><span data-stu-id="59f93-234">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="59f93-235">Cuando se ejecuta la aplicación, se cargan los secretos del almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="59f93-235">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="59f93-236">El secreto de la cadena de `5000-AppSecret` se asocia a la versión de la aplicación especificada en el archivo de proyecto de la aplicación (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="59f93-236">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="59f93-237">La versión, `5000` (con el guión), se elimina del nombre de clave.</span><span class="sxs-lookup"><span data-stu-id="59f93-237">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="59f93-238">A lo largo de la aplicación, lee la configuración con la clave `AppSecret` carga el valor del secreto.</span><span class="sxs-lookup"><span data-stu-id="59f93-238">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="59f93-239">Si se cambia la versión de la aplicación en el archivo de proyecto `5.1.0.0` y se vuelve a ejecutar la aplicación, el valor secreto devuelto es `5.1.0.0_secret_value_dev` en el entorno de desarrollo y `5.1.0.0_secret_value_prod` en producción.</span><span class="sxs-lookup"><span data-stu-id="59f93-239">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="59f93-240">También puede proporcionar su propia `KeyVaultClient` implementación `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="59f93-240">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="59f93-241">Un cliente personalizado permite compartir una única instancia del cliente a través de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59f93-241">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a><span data-ttu-id="59f93-242">Autenticarse en Azure Key Vault con un certificado X.509</span><span class="sxs-lookup"><span data-stu-id="59f93-242">Authenticate to Azure Key Vault with an X.509 certificate</span></span>

<span data-ttu-id="59f93-243">Al desarrollar una aplicación de .NET Framework en un entorno que admite los certificados, puede autenticarse en Azure Key Vault con un certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="59f93-243">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="59f93-244">Clave privada del certificado X.509 es administrada por el sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="59f93-244">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="59f93-245">Para obtener más información, consulte [autenticar con un certificado en lugar de un secreto de cliente](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="59f93-245">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="59f93-246">Use la `AddAzureKeyVault` sobrecarga que acepta un `X509Certificate2` (`_env` en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="59f93-246">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="59f93-247">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="59f93-247">Bind an array to a class</span></span>

<span data-ttu-id="59f93-248">El proveedor es capaz de leer los valores de configuración en una matriz para el enlace a una matriz POCO.</span><span class="sxs-lookup"><span data-stu-id="59f93-248">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="59f93-249">Al leer desde un origen de configuración que permite que las claves que contiene dos puntos (`:`) separadores, un segmento clave numérico se usa para distinguir las claves que constituyen una matriz (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="59f93-249">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="59f93-250">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="59f93-250">`:{n}:`).</span></span> <span data-ttu-id="59f93-251">Para obtener más información, consulte [configuración: Enlazar una matriz a una clase](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="59f93-251">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="59f93-252">Claves de Azure Key Vault no pueden usar un coma como separador.</span><span class="sxs-lookup"><span data-stu-id="59f93-252">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="59f93-253">El enfoque descrito en este tema usa los guiones dobles (`--`) como separador para valores jerárquicos (secciones).</span><span class="sxs-lookup"><span data-stu-id="59f93-253">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="59f93-254">Las claves de matriz se almacenan en Azure Key Vault con doble guiones y segmentos de clave numéricas (`--0--`, `--1--`,...</span><span class="sxs-lookup"><span data-stu-id="59f93-254">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="59f93-255">`--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="59f93-255">`--{n}--`).</span></span>

<span data-ttu-id="59f93-256">Examine el siguiente [Serilog](https://serilog.net/) proporcionado por un archivo JSON de configuración de proveedor de registro.</span><span class="sxs-lookup"><span data-stu-id="59f93-256">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="59f93-257">Hay dos objetos literales definidos en el `WriteTo` matriz que se reflejan dos Serilog *receptores*, que describen los destinos de salida del registro:</span><span class="sxs-lookup"><span data-stu-id="59f93-257">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="59f93-258">La configuración se muestra en el archivo JSON anterior se almacena en Azure Key Vault con guión doble (`--`) numérica y notación de segmentos:</span><span class="sxs-lookup"><span data-stu-id="59f93-258">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="59f93-259">Key</span><span class="sxs-lookup"><span data-stu-id="59f93-259">Key</span></span> | <span data-ttu-id="59f93-260">Valor</span><span class="sxs-lookup"><span data-stu-id="59f93-260">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="59f93-261">Volver a cargar los secretos</span><span class="sxs-lookup"><span data-stu-id="59f93-261">Reload secrets</span></span>

<span data-ttu-id="59f93-262">Los secretos se almacenan en caché hasta que `IConfigurationRoot.Reload()` se llama.</span><span class="sxs-lookup"><span data-stu-id="59f93-262">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="59f93-263">Ha expirado, deshabilitado, y actualizado los secretos del almacén de claves no se respeten la aplicación hasta que `Reload` se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="59f93-263">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="59f93-264">Secretos deshabilitados y expirados</span><span class="sxs-lookup"><span data-stu-id="59f93-264">Disabled and expired secrets</span></span>

<span data-ttu-id="59f93-265">Generar secretos expirados y deshabilitados una `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="59f93-265">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="59f93-266">Para evitar que la aplicación genere, reemplace la aplicación o actualizar el secreto deshabilitado o expirado.</span><span class="sxs-lookup"><span data-stu-id="59f93-266">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="59f93-267">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="59f93-267">Troubleshoot</span></span>

<span data-ttu-id="59f93-268">Cuando no se puede cargar la configuración mediante el proveedor de la aplicación, se escribe un mensaje de error en la [infraestructura de registro de ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="59f93-268">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="59f93-269">Configuración de carga evitará que las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="59f93-269">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="59f93-270">La aplicación no está configurada correctamente en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="59f93-270">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="59f93-271">El almacén de claves no existe en Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="59f93-271">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="59f93-272">La aplicación no está autorizada para acceder al almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="59f93-272">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="59f93-273">La directiva de acceso no incluye `Get` y `List` permisos.</span><span class="sxs-lookup"><span data-stu-id="59f93-273">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="59f93-274">En el almacén de claves, los datos de configuración (par nombre-valor) denominados incorrectamente, falta, deshabilitado o expirado.</span><span class="sxs-lookup"><span data-stu-id="59f93-274">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="59f93-275">La aplicación tiene el nombre de almacén de claves incorrecto (`Vault`), Id. de aplicación de Azure AD (`ClientId`), o la clave de Azure AD (`ClientSecret`).</span><span class="sxs-lookup"><span data-stu-id="59f93-275">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="59f93-276">La clave de Azure AD (`ClientSecret`) ha expirado.</span><span class="sxs-lookup"><span data-stu-id="59f93-276">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="59f93-277">La clave de configuración (nombre) es incorrecta en la aplicación para el valor que está intentando cargar.</span><span class="sxs-lookup"><span data-stu-id="59f93-277">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59f93-278">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="59f93-278">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="59f93-279">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="59f93-279">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="59f93-280">Microsoft Azure: Documentación de Key Vault</span><span class="sxs-lookup"><span data-stu-id="59f93-280">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="59f93-281">Para generar y transferir protegidas con HSM de claves para Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="59f93-281">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="59f93-282">Clase KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="59f93-282">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
