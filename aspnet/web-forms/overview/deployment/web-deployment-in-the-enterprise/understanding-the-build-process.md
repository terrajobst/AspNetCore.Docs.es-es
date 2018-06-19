---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Descripción del proceso de compilación | Documentos de Microsoft
author: jrjlee
description: En este tema se ofrece un tutorial de un proceso de compilación e implementación de escala empresarial. El enfoque descrito en este tema usa Engin personalizados de compilación de Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 4544a5e6212ea9b1247062dc35edc135ff7ca354
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888005"
---
<a name="understanding-the-build-process"></a>Descripción del proceso de compilación
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se ofrece un tutorial de un proceso de compilación e implementación de escala empresarial. El enfoque descrito en este tema usa archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) para proporcionar un control minucioso sobre todos los aspectos del proceso. Dentro de los archivos de proyecto, destinos de MSBuild personalizados se utilizan para ejecutar la utilidad de implementación de base de datos VSDBCMD.exe y utilidades de implementación como la herramienta de implementación Web de Internet Information Services (IIS) (MSDeploy.exe).
> 
> > [!NOTE]
> > El tema anterior, [comprender el archivo de proyecto](understanding-the-project-file.md), se describen los componentes clave de un archivo de proyecto de MSBuild e introdujo el concepto de división de archivos de proyecto para admitir la implementación en varios entornos de destino. Si no ya está familiarizado con estos conceptos, debe revisar [comprender el archivo de proyecto](understanding-the-project-file.md) antes de que funcione a través de este tema.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="build-and-deployment-overview"></a>Información general de implementación y compilación

En el [póngase en contacto con el administrador solución](the-contact-manager-solution.md), tres archivos controlan el proceso de compilación e implementación:

- A *archivo de proyecto universal* (*Publish.proj*). Contiene instrucciones de compilación e implementación que no cambian entre los entornos de destino.
- Un *archivo de proyecto específicas del entorno* (*Dev.proj Env*). Esto contiene la configuración de compilación e implementación que es específicas de un entorno de destino concreto. Por ejemplo, podría usar la *Env Dev.proj* archivo para proporcionar la configuración de un entorno de prueba o desarrollador y cree un archivo alternativo denominado *Stage.proj Env* para proporcionar la configuración para un almacenamiento provisional entorno.
- A *archivo de comandos* (*Dev.cmd publicar*). Contiene un comando que especifica el proyecto que los archivos desea ejecutar de MSBuild.exe. Puede crear un archivo de comandos para cada entorno de destino, donde cada archivo contiene un comando de MSBuild.exe que especifica un archivo de proyecto específicos de un entorno diferente. Esto permite al desarrollador implementar en distintos entornos solo con ejecutar el archivo de comandos adecuados.

En la solución de ejemplo, puede encontrar estos tres archivos en la carpeta de solución de publicación.

![](understanding-the-build-process/_static/image1.png)

Antes de examinar estos archivos con más detalle, echemos un vistazo a cómo el proceso de compilación general funciona cuando se emplea este método. En un nivel alto, el proceso de compilación e implementación tiene este aspecto:

![](understanding-the-build-process/_static/image2.png)

Lo primero que ocurre es que los dos archivos de proyecto&#x2014;uno que contiene instrucciones de compilación e implementación universales y otro que contiene configuración específica del entorno&#x2014;se combinan en un único archivo de proyecto. MSBuild, a continuación, funciona a través de las instrucciones que aparecen en el archivo de proyecto. Se crea cada uno de los proyectos de la solución, utilizando el archivo de proyecto para cada proyecto. A continuación, llama a otras herramientas, como Web Deploy (MSDeploy.exe) y la utilidad VSDBCMD para implementar el contenido web y bases de datos en el entorno de destino.

De principio a fin, el proceso de compilación e implementación realiza estas tareas:

1. Elimina el contenido del directorio de salida, como preparación para una nueva compilación.
2. Crea cada proyecto de la solución:

    1. Para los proyectos web&#x2014;en este caso, una aplicación web de MVC de ASP.NET y WCF servicio web&#x2014;el proceso de compilación crea un paquete de implementación web para cada proyecto.
    2. Para los proyectos de base de datos, el proceso de compilación crea un manifiesto de implementación (archivo .deploymanifest) para cada proyecto.
3. Usa la utilidad VSDBCMD.exe para implementar cada proyecto de base de datos en la solución, con distintas propiedades de los archivos de proyecto&#x2014;una cadena de conexión de destino y un nombre de base de datos&#x2014;junto con el archivo .deploymanifest.
4. Usa la utilidad MSDeploy.exe para implementar cada proyecto web en la solución, con distintas propiedades de los archivos de proyecto para controlar el proceso de implementación.

Puede usar la solución de ejemplo para realizar el seguimiento de este proceso con más detalle.

> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicas del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Invocar la compilación y el proceso de implementación

Para implementar la solución póngase en contacto con el administrador en un entorno de prueba para desarrolladores, se ejecuta el desarrollador el *Dev.cmd publicar* archivo de comandos. Esto invoca MSBuild.exe, especificar *Publish.proj* como el archivo de proyecto para ejecutar y *Dev.proj Env* como valor de parámetro.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> El **/fl** cambiar (abreviatura de **/fileLogger**) registra el resultado de la compilación en un archivo denominado *msbuild.log* en el directorio actual. Para obtener más información, consulte el [referencia de línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


En este momento, MSBuild empieza a ejecutarse, carga el *Publish.proj* archivo y comenzará a procesar las instrucciones dentro de él. La primera instrucción indica a MSBuild que importa el proyecto de archivos que la **TargetEnvPropsFile** parámetro especifica.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


El **TargetEnvPropsFile** parámetro especifica la *Env Dev.proj* de archivos, por lo que MSBuild combina el contenido de la *Env Dev.proj* el archivo en el  *Publish.proj* archivo.

Los siguientes elementos que MSBuild se encuentra en el archivo de proyecto combinados son grupos de propiedades. Propiedades se procesan en el orden en que aparecen en el archivo. MSBuild crea un par de clave y valor para cada propiedad, proporcionando que se cumplen las condiciones especificadas. Propiedades definidas más adelante en el archivo sobrescriban todas las propiedades con el mismo nombre que definió anteriormente en el archivo. Por ejemplo, considere la **OutputRoot** propiedades.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Cuando MSBuild procesa la primera **OutputRoot** elemento, proporcionando un parámetro con el mismo nombre no se ha proporcionado, Establece el valor de la **OutputRoot** propiedad **... \Publish\Out**. Cuando encuentra el segundo **OutputRoot** elemento, si la condición se evalúa como **true**, sobrescribirá el valor de la **OutputRoot** propiedad con el valor de la **OutDir** parámetro.

El siguiente elemento que se encuentra en MSBuild es un grupo de elementos único, que contiene un elemento denominado **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild procesa esta instrucción mediante la creación de una lista de elementos con el nombre **ProjectsToBuild**. En este caso, la lista de elementos contiene un único valor&#x2014;la ruta de acceso y el nombre del archivo de solución.

En este momento, los elementos restantes son objetivos. Destinos se procesan de forma diferente de propiedades y los elementos&#x2014;destinos en esencia, no se procesan a menos que están ya sea explícitamente especificados por el usuario o invocados por otra construcción dentro del archivo de proyecto. Recuerde que la apertura **proyecto** etiqueta incluye un **DefaultTargets** atributo.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Esto indica a MSBuild que invoque el **FullPublish** destino, si los destinos no están especificados cuando se invoque MSBuild.exe. El **FullPublish** destino no contiene ninguna tarea; en su lugar, simplemente especifica una lista de dependencias.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Esta dependencia indica a MSBuild que en orden para ejecutar el **FullPublish** destino, debe invocar esta lista de destinos en el orden indicado:

1. Se debe invocar el **limpiar** destino.
2. Se debe invocar el **BuildProjects** destino.
3. Se debe invocar el **GatherPackagesForPublishing** destino.
4. Se debe invocar el **PublishDbPackages** destino.
5. Se debe invocar el **PublishWebPackages** destino.

### <a name="the-clean-target"></a>El destino Clean

El **limpiar** destino básicamente elimina el directorio de salida y todo su contenido, como preparación para una nueva compilación.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Tenga en cuenta que el destino incluye un **ItemGroup** elemento. Al definir propiedades o elementos de un **destino** elemento, va a crear *dinámica* propiedades y elementos. En otras palabras, las propiedades o elementos no se procesan hasta que se ejecuta el destino. El directorio de salida podría no existe o contener los archivos hasta que comience el proceso de compilación, lo que no se puede compilar la  **\_FilesToDelete** aparece como un elemento estático; tendrá que esperar hasta la ejecución en curso. Por lo tanto, se crea la lista como un elemento dinámico dentro del destino.

> [!NOTE]
> En este caso, dado que la **limpiar** destino es la primera que se ejecuta, no es necesario usar un grupo de elementos dinámicos real. Sin embargo, es recomendable utilizar propiedades dinámicas y los elementos de este tipo de escenario, tal y como se desea ejecutar destinos en un orden diferente en algún momento.  
> También debe tener como objetivo para evitar declarar elementos que no se utilizará nunca. Si tiene elementos que solo se usará en un destino concreto, considere la posibilidad de colocarlas en el destino para quitar cualquier sobrecarga innecesaria en el proceso de compilación.


Dinámica de elementos, el **limpiar** destino es bastante sencilla y hace uso de los integrados **mensaje**, **eliminar**, y **RemoveDir**tareas para:

1. Enviar un mensaje al registrador.
2. Generar una lista de archivos que se va a eliminar.
3. Elimine los archivos.
4. Quite el directorio de salida.

### <a name="the-buildprojects-target"></a>El destino de BuildProjects

El **BuildProjects** destino básicamente genera todos los proyectos de la solución de ejemplo.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Este destino se describe con más detalle en el tema anterior, [comprender el archivo de proyecto](understanding-the-project-file.md), para ilustrar cómo tareas y los destinos hacer referencia a propiedades y elementos. En este momento, le interesa principalmente la **MSBuild** tarea. Puede utilizar esta tarea para compilar varios proyectos. La tarea no crea una nueva instancia de MSBuild.exe; utiliza la instancia de ejecución actual para cada proyecto de compilación. Los puntos clave de interés en este ejemplo se enumeran las propiedades de implementación:

- El **DeployOnBuild** propiedad indica a MSBuild que ejecute las instrucciones de implementación en la configuración del proyecto una vez completada la compilación de cada proyecto.
- El **DeployTarget** propiedad identifica el destino que se desea invocar después de que se compila el proyecto. En este caso, el **paquete** destino genera el resultado del proyecto en un paquete de web que se pueden implementar.

> [!NOTE]
> El **paquete** destino llama a la Web publicación canalización (WPP), que proporciona la integración entre MSBuild y Web Deploy. Si desea Eche un vistazo a los destinos integrados que proporciona el WPP, revise la *Microsoft.Web.Publishing.targets* archivo en la carpeta de %\MSBuild\Microsoft\VisualStudio\v10.0\Web % PROGRAMFILES (x 86).


### <a name="the-gatherpackagesforpublishing-target"></a>El destino de GatherPackagesForPublishing

Si analiza el **GatherPackagesForPublishing** destino, observará que realmente no contiene todas las tareas. En su lugar, contiene un grupo de elementos único que define tres elementos dinámicos.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Estos elementos hacen referencia a los paquetes de implementación que se crearon cuando la **BuildProjects** se ha ejecutado el destino. No se pudo definir estos elementos de estáticamente en el archivo de proyecto, dado que no existen los archivos al que hacen referencia los elementos hasta que el **BuildProjects** se ejecuta el destino. En su lugar, los elementos deben definirse dinámicamente dentro de un destino que no se invoca hasta después de la **BuildProjects** se ejecuta el destino.

No se usan los elementos dentro de este destino&#x2014;este destino simplemente genera los elementos y los metadatos asociados a cada valor de elemento. Una vez que se procesan estos elementos, el **PublishPackages** elemento contendrá dos valores, la ruta de acceso a la *ContactManager.Mvc.deploy.cmd* archivo y la ruta de acceso a la  *ContactManager.Service.deploy.cmd* archivo. Web Deploy crea estos archivos como parte del paquete para cada proyecto web, y éstos son los archivos que se deben invocar en el servidor de destino con el fin de implementar los paquetes. Si abre uno de estos archivos, básicamente verá un comando MSDeploy.exe con distintos valores de parámetros específicos de la compilación.

El **DbPublishPackages** elemento contendrá un valor único, la ruta de acceso a la *ContactManager.Database.deploymanifest* archivo.

> [!NOTE]
> Un archivo .deploymanifest se genera cuando se compila un proyecto de base de datos y utiliza el mismo esquema como un archivo de proyecto de MSBuild. Contiene toda la información necesaria para implementar una base de datos, incluidos los detalles de los scripts anteriores y posteriores a la implementación y la ubicación del esquema de base de datos (.dbschema). Para obtener más información, consulte [An Overview of Database Build e implementación](https://msdn.microsoft.com/library/aa833165.aspx).


Aprenderá más acerca de cómo se crean y usan en paquetes de implementación y manifiestos de implementación de base de datos [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md) y [implementar proyectos de base de datos](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>El destino de PublishDbPackages

Brevemente general, la **PublishDbPackages** destino llama a la utilidad VSDBCMD para implementar la **ContactManager** base de datos en un entorno de destino. La configuración de implementación de base de datos implica una gran cantidad de decisiones y matices y aprenderá más acerca de esto en [implementar proyectos de base de datos](deploying-database-projects.md) y [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). En este tema, nos centraremos en cómo funciona realmente este destino.

En primer lugar, tenga en cuenta que la etiqueta de apertura incluye un **salidas** atributo.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Este es un ejemplo de *el procesamiento por lotes de destino*. En los archivos de proyecto de MSBuild, el procesamiento por lotes es una técnica para recorrer en iteración colecciones. El valor de la **salidas** atributo, **"% (DbPublishPackages.Identity)"**, hace referencia a la **identidad** propiedad de metadatos de la **DbPublishPackages**  lista de elementos. Esta notación **Outputs=%***(ItemList.ItemMetadataName)*, se traduce como:

- Dividir los elementos de **DbPublishPackages** en lotes de elementos que contienen el mismo **identidad** valor de metadatos.
- Ejecute el destino una vez por lote.

> [!NOTE]
> **Identidad** es uno de los [valores de metadatos integradas](https://msdn.microsoft.com/library/ms164313.aspx) que se asigna a cada elemento durante la creación. Hace referencia al valor de la **Include** de atributo en el **elemento** elemento&#x2014;en otras palabras, la ruta de acceso y el nombre del elemento.


En este caso, dado que nunca debería haber más de un elemento con la misma ruta de acceso y nombre de archivo, básicamente estamos trabajando con tamaños de lote de uno. El destino se ejecuta una vez por cada paquete de la base de datos.

Puede ver una notación similar en el  **\_Cmd** propiedad, que genera un comando VSDBCMD con los modificadores correspondientes.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


En este caso, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, y **%(DbPublishPackages.FullPath)** todos hacen referencia a los valores de metadatos de la **DbPublishPackages** colección de elementos. El  **\_Cmd** propiedad se usa en la **Exec** tarea, que invoca el comando.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Como resultado de esta notación, el **Exec** tarea creará lotes basándose en combinaciones únicas de la **DatabaseConnectionString**, **TargetDatabase**y **FullPath** valores de metadatos y la tarea se ejecutarán una vez para cada lote. Este es un ejemplo de *tarea de procesamiento por lotes*. Sin embargo, dado que el nivel de destino de procesamiento por lotes, la colección de elementos en lotes con un único elemento, ya ha dividido el **Exec** tarea ejecutará una vez y solo una vez para cada iteración del destino. En otras palabras, esta tarea invoca la utilidad VSDBCMD una vez para cada paquete de la base de datos en la solución.

> [!NOTE]
> Para obtener más información sobre el procesamiento por lotes de tareas y de destino, vea MSBuild [procesamiento por lotes](https://msdn.microsoft.com/library/ms171473.aspx), [metadatos de elementos en procesamiento por lotes de destino](https://msdn.microsoft.com/library/ms228229.aspx), y [metadatos de elementos en procesamiento por lotes de tareas](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>El destino de PublishWebPackages

Hasta este momento, se han invocado el **BuildProjects** destino, lo cual genera un paquete de implementación web para cada proyecto en la solución de ejemplo. Que acompaña a cada paquete es un *deploy.cmd* archivo, que contiene los comandos MSDeploy.exe necesarios para implementar el paquete en el entorno de destino, y un *SetParameters.xml* archivo, que especifica el detalles necesarios del entorno de destino. También se han invocado el **GatherPackagesForPublishing** destino, lo cual genera una colección de elementos que contiene el *deploy.cmd* archivos que le interesa. En esencia, el **PublishWebPackages** destino realiza las siguientes funciones:

- Manipula la *SetParameters.xml* archivo para cada paquete para incluir los detalles correcta para el entorno de destino, mediante la **XmlPoke** tarea.
- Invoca el *deploy.cmd* archivo para cada paquete, mediante los modificadores correspondientes.

Al igual que el **PublishDbPackages** destino, el **PublishWebPackages** destino usa el procesamiento por lotes de destino para asegurarse de que el destino se ejecuta una vez para cada paquete de web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Dentro del destino, el **Exec** tarea se usa para ejecutar el *deploy.cmd* archivo para cada paquete de web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Para obtener más información acerca de cómo configurar la implementación de paquetes de web, consulte [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona un tutorial sobre cómo se utilizan los archivos de proyecto de división para controlar el proceso de compilación e implementación de principio a fin para la solución de ejemplo póngase en contacto con el administrador. Con este enfoque permite ejecutar complejo, las implementaciones de escala empresarial en un único paso repetible, simplemente mediante la ejecución de un archivo de comandos específicos del entorno.

## <a name="further-reading"></a>Información adicional

Para obtener una introducción más detallada a los archivos de proyecto y el WPP, consulte [dentro de la Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-project-file.md)
> [Siguiente](building-and-packaging-web-application-projects.md)
