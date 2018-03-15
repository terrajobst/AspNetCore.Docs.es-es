---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Comprender el archivo de proyecto | Documentos de Microsoft
author: jrjlee
description: "Archivos de proyecto de Microsoft Build Engine (MSBuild) se encuentran en el núcleo del proceso de compilación e implementación. Este tema comienza con una visión general de MSBuild..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="understanding-the-project-file"></a>Comprender el archivo de proyecto
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Archivos de proyecto de Microsoft Build Engine (MSBuild) se encuentran en el núcleo del proceso de compilación e implementación. Este tema comienza con una visión general de MSBuild y el archivo de proyecto. Describe los componentes clave que podrá transmitidas cuando trabaja con archivos de proyecto, y funciona a través de un ejemplo de cómo puede usar archivos de proyecto para implementar las aplicaciones reales.
> 
> Lo que aprenderá:
> 
> - Cómo MSBuild utiliza archivos de proyecto de MSBuild para compilar proyectos.
> - Cómo MSBuild se integra con tecnologías de implementación, como la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy).
> - Cómo entender los componentes clave de un archivo de proyecto.
> - ¿Cómo puede usar archivos de proyecto para compilar e implementar aplicaciones complejas.


## <a name="msbuild-and-the-project-file"></a>MSBuild y el archivo de proyecto

Al crear y compilar soluciones en Visual Studio, Visual Studio utiliza MSBuild para compilar cada proyecto de la solución. Cada proyecto de Visual Studio incluye un archivo de proyecto de MSBuild, con una extensión de archivo que refleje el tipo de proyecto & #x 2014; por ejemplo, un proyecto de C# (.csproj), un proyecto de Visual Basic.NET (.vbproj) o un proyecto de base de datos (.dbproj). Para compilar un proyecto, MSBuild debe procesar el archivo de proyecto asociado al proyecto. El archivo de proyecto es un documento XML que contiene toda la información y las instrucciones que MSBuild necesita para compilar el proyecto, como el contenido que se va a incluir, los requisitos de la plataforma, información de versiones, servidor web o configuración de servidor de base de datos y la tareas que deben realizarse.

Archivos de proyecto de MSBuild se basan en el [esquema XML de MSBuild](https://msdn.microsoft.com/library/5dy88c2e.aspx), y así el proceso de compilación es completamente abierto y transparente. Además, no es necesario instalar Visual Studio para poder usar el motor de MSBuild & #x 2014; el ejecutable de MSBuild.exe es parte de .NET Framework y se puede ejecutar desde un símbolo del sistema. Como desarrollador, puede crear sus propios archivos de proyecto de MSBuild, con el esquema XML de MSBuild, imponer control sofisticado y específico sobre cómo se generan e implementan los proyectos. Estos archivos de proyecto personalizadas funcionan en exactamente del mismo modo que los archivos de proyecto que Visual Studio genera automáticamente.

> [!NOTE]
> También puede usar archivos de proyecto de MSBuild con el servicio Team Build en Team Foundation Server (TFS). Por ejemplo, puede usar archivos de proyecto en escenarios de integración continua (CI) para automatizar la implementación en un entorno de prueba al proteger el código nuevo. Para obtener más información, consulte [configurar Team Foundation Server para automatizar la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Convenciones de nomenclatura de archivo de proyecto

Al crear sus propios archivos de proyecto, puede usar cualquier extensión de archivo que desee. Sin embargo, para facilitar las soluciones de otras personas entiendan, debe usar estas convenciones comunes:

- Usar la extensión proj cuando se crea un archivo de proyecto que compila proyectos.
- Usar la extensión .targets cuando se crea un archivo de proyecto reutilizables para importar en otros archivos del proyecto. Archivos con la extensión .targets normalmente no compilan nada por sí mismos, simplemente contienen instrucciones que puede importar los archivos proj.

### <a name="integration-with-deployment-technologies"></a>Integración con tecnologías de implementación

Si ya ha trabajado con proyectos de aplicación web en Visual Studio 2010, como las aplicaciones web ASP.NET y aplicaciones web de ASP.NET MVC, sabrá que estos proyectos incluyen compatibilidad integrada para empaquetar e implementar la aplicación web en un entorno de destino. El **propiedades** páginas con estos proyectos incluyen **Empaquetar/Publicar Web** y **Empaquetar/publicar SQL** pestañas que puede usar para configurar cómo los componentes de su aplicaciones se empaquetan e implementan. Esto muestra la **Empaquetar/Publicar Web** ficha:

![](understanding-the-project-file/_static/image1.png)

La tecnología que subyace estas capacidades se conoce como la canalización de publicación de Web (WPP). El WPP básicamente pone MSBuild y [Web Deploy](https://go.microsoft.com/?linkid=9805122) conjuntamente para proporcionar un proceso de compilación, implementación de paquete y completo para las aplicaciones web.

Lo bueno es que puede aprovechar las ventajas de los puntos de integración que la WPP proporciona cuando se crean archivos de proyecto personalizado para proyectos web. Puede incluir instrucciones de implementación en el archivo de proyecto, lo que permite generar los proyectos, crear paquetes de implementación web e instale estos paquetes en servidores remotos a través de un único archivo de proyecto y una sola llamada a MSBuild. También puede llamar a cualquier otros ejecutables como parte del proceso de compilación. Por ejemplo, puede ejecutar la herramienta de línea de comandos VSDBCMD.exe para implementar una base de datos desde un archivo de esquema. En el transcurso de este tema, podrá ver cómo puede aprovechar estas capacidades para cumplir los requisitos de los escenarios de implementación de empresa.

> [!NOTE]
> Para obtener más información sobre cómo funciona el proceso de implementación de aplicación web, consulte [introducción de implementación de proyecto de aplicación de ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>La Anatomía de un archivo de proyecto

Antes de examinar el proceso de compilación con más detalle, merece la pena realizar unos instantes para familiarizarse con la estructura básica de un archivo de proyecto de MSBuild. Esta sección proporciona información general de los elementos más comunes que encontrará cuando revise, editar o crear un archivo de proyecto. En concreto, aprenderá a:

- Cómo usar *propiedades* para administrar las variables para el proceso de compilación.
- Cómo usar *elementos* para identificar las entradas para el proceso de compilación, como archivos de código.
- Cómo usar *destinos* y *tareas* para proporcionar instrucciones de ejecución a MSBuild, con *propiedades* y *elementos* definida en otra parte el archivo de proyecto.

Esto muestra la relación entre los elementos clave de un archivo de proyecto de MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>El elemento de proyecto

El [proyecto](https://msdn.microsoft.com/library/bcxfsh87.aspx) elemento es el elemento raíz de cada archivo de proyecto. Además de identificar el esquema XML para el archivo de proyecto, el **proyecto** elemento puede incluir atributos para especificar los puntos de entrada para el proceso de compilación. Por ejemplo, en la [solución de ejemplo de Contact Manager](the-contact-manager-solution.md), el *Publish.proj* archivo Especifica que la compilación debe iniciarse mediante una llamada del destino denominado **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Propiedades y condiciones

Un archivo de proyecto, normalmente, debe proporcionar una gran cantidad de diferentes partes de información con el fin de generar e implementar los proyectos correctamente. Estos fragmentos de información pudieron incluir nombres de servidor, cadenas de conexión, las credenciales, las configuraciones de compilación, rutas de acceso de archivo de origen y de destino y cualquier otra información que van a incluir para admitir la personalización. En un archivo de proyecto, las propiedades deben estar definidas dentro de un [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elemento. Propiedades de MSBuild constan de pares de clave y valor. En el **PropertyGroup** elemento, el nombre del elemento define la clave de propiedad y el contenido del elemento define el valor de propiedad. Por ejemplo, podría definir propiedades denominadas **ServerName** y **ConnectionString** para almacenar una cadena de conexión y nombre de servidor estático.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Para recuperar un valor de propiedad, utilice el formato **$(***PropertyName***) ***.* Por ejemplo, para recuperar el valor de la **ServerName** propiedad, escriba:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Verá ejemplos de cómo y cuándo se debe utilizar valores de propiedad más adelante en este tema.


Incrustar información como propiedades estáticas en un archivo de proyecto no es siempre el enfoque ideal para administrar el proceso de compilación. En muchos escenarios, desea obtener la información de otros orígenes o permitir al usuario que proporcione la información de la línea de comandos. MSBuild permite especificar cualquier valor de propiedad como un parámetro de línea de comandos. Por ejemplo, el usuario podría proporcionar un valor para **ServerName** al que se ejecuta MSBuild.exe desde la línea de comandos.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Para obtener más información sobre los argumentos y los modificadores que puede utilizar con MSBuild.exe, vea [referencia de línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


Puede usar la misma sintaxis de propiedad para obtener los valores de variables de entorno y las propiedades de proyecto integrados. Una gran cantidad de propiedades utilizadas con frecuencia se definen automáticamente y puede usar los archivos de proyecto al incluir el nombre del parámetro correspondiente. Por ejemplo, para recuperar la plataforma del proyecto actual & #x 2014; por ejemplo, **x86** o **AnyCpu**& #x 2014; puede incluir la **$(Platform)** referencia de propiedad en el archivo de proyecto. Para obtener más información, consulte [Macros para propiedades y comandos de generación](https://msdn.microsoft.com/library/c02as0cs.aspx), [propiedades comunes de proyectos de MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), y [propiedades reservadas](https://msdn.microsoft.com/library/ms164309.aspx).

Propiedades se utilizan a menudo junto con *condiciones*. La mayoría de los elementos de MSBuild admite el **condición** atributo, que le permite especificar los criterios que MSBuild debe evaluar el elemento. Por ejemplo, considere la posibilidad de esta definición de propiedad:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Cuando MSBuild procesa esta definición de propiedad, en primer lugar comprueba para ver si un **$(OutputRoot)** valor de la propiedad está disponible. Si el valor de propiedad está en blanco & #x 2014; en otras palabras, el usuario no ha proporcionado un valor para esta propiedad & #x 2014; la condición se evalúa como **true** y el valor de propiedad se establece en **... \Publish\Out**. Si el usuario ha proporcionado un valor para esta propiedad, la condición se evalúa como **false** y no se utiliza el valor de propiedad estática.

Para obtener más información sobre las distintas maneras en que puede especificar las condiciones, consulte [condiciones de MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elementos y grupos de elementos

Uno de los roles importantes del archivo del proyecto es definir las entradas para el proceso de compilación. Normalmente, estas entradas son archivos & #x 2014; archivos de código, archivos de configuración, archivos de comandos y otros archivos que necesita para procesar o copiar como parte del proceso de compilación. En el esquema de proyecto de MSBuild, estas entradas se representan mediante [elementos](https://msdn.microsoft.com/library/ms164283.aspx) elementos. En un archivo de proyecto, se deben definir elementos dentro de un [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elemento. Al igual que **propiedad** elementos, puede asignar un **elemento** elemento sin embargo al igual que. Sin embargo, debe especificar un **Include** atributo para identificar el archivo o comodín que representa el elemento.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Si especifica varios **elementos** elementos con el mismo nombre, en realidad está creando una lista de recursos con nombre. Una buena forma de ver esto en acción es echar un vistazo dentro de uno de los archivos de proyecto creados por Visual Studio. Por ejemplo, el *ContactManager.Mvc.csproj* archivo en la solución de ejemplo incluye una gran cantidad de grupos de elementos, cada uno con varios exactamente el mismo nombre **elementos** elementos.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


De este modo, el archivo de proyecto se indica a MSBuild para crear listas de los archivos que deben procesarse en la misma manera & #x 2014; la **referencia** lista incluye los ensamblados que deben estar en su lugar para una compilación correcta, el **Compilar** lista incluye los archivos de código que se deben compilar, y el **contenido** lista incluye recursos que deben copiarse sin modificaciones. Analizaremos cómo el proceso de compilación hace referencia y utiliza estos elementos más adelante en este tema.

También pueden incluir elementos [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) los elementos secundarios. Estos son pares de clave-valor definidos por el usuario y básicamente representan propiedades que son específicas para ese elemento. Por ejemplo, muchos de los **compilar** incluyen elementos en el archivo de proyecto **DependentUpon** los elementos secundarios.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Además de metadatos de elementos creados por el usuario, todos los elementos se asignan varios metadatos comunes durante la creación. Para obtener más información, consulte [Metadatos de elementos conocidos](https://msdn.microsoft.com/library/ms164313.aspx).


Puede crear **ItemGroup** elementos en el nivel raíz **proyecto** elemento o dentro de específicos de **destino** elementos. **ItemGroup** también admiten elementos **condición** atributos, que le permite personalizar las entradas para el proceso de compilación según condiciones como la configuración del proyecto o la plataforma.

### <a name="targets-and-tasks"></a>Destinos y tareas

En el esquema de MSBuild, un [tarea](https://msdn.microsoft.com/library/77f2hx1s.aspx) elemento representa una instrucción de compilación individual (o tareas). MSBuild incluye un gran número de tareas predefinidas. Por ejemplo:

- El **copia** tarea copia archivos a una nueva ubicación.
- El **Csc** tarea, invoca el compilador de Visual C#.
- El **Vbc** tarea, invoca el compilador de Visual Basic.
- El **Exec** tarea ejecuta un programa especificado.
- El **mensaje** tarea escribe un mensaje en un registrador.

> [!NOTE]
> Para obtener detalles completos de las tareas que están disponibles desde el principio, consulte [referencia de tareas de MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Para obtener más información sobre las tareas, incluido cómo crear sus propias tareas personalizadas, consulte [tareas de MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Las tareas siempre deben estar contenidas en [destino](https://msdn.microsoft.com/library/t50z2hka.aspx) elementos. A **destino** elemento es un conjunto de uno o más tareas que se ejecutan secuencialmente y un archivo de proyecto puede contener varios destinos. Si desea ejecutar una tarea o un conjunto de tareas, se invoca el destino que los contiene. Por ejemplo, suponga que tiene un archivo de proyecto simple registra un mensaje.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Puede invocar el destino de la línea de comandos mediante la **/t** conmutador para especificar el destino.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Como alternativa, puede agregar un **DefaultTargets** atribuir a la **proyecto** elemento para especificar los destinos que se desea invocar.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


En este caso, no es necesario especificar el destino de la línea de comandos. Puede especificar simplemente el archivo de proyecto y MSBuild invocará la **FullPublish** destino automáticamente.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Destinos y tareas pueden incluir **condición** atributos. Por lo tanto, puede optar por omitir todos destinos o tareas individuales si se cumplen determinadas condiciones.

Por lo general, al crear tareas útiles y destinos, debe hacer referencia a las propiedades y los elementos que han definido en otro lugar en el archivo de proyecto:

- Para usar un valor de propiedad, escriba **$(***PropertyName***)**, donde *PropertyName* es el nombre de la **propiedad** elemento o el nombre de la parámetro.
- Para usar un elemento, escriba **@(***ItemName***)**, donde *ItemName* es el nombre de la **elemento** elemento.

> [!NOTE]
> Recuerde que si crea varios elementos con el mismo nombre, está creando una lista. En cambio, si crea varias propiedades con el mismo nombre, el último valor de propiedad que proporcione sobrescribirá cualquier propiedad anterior con el mismo nombre & #x 2014; una propiedad solo puede contener un valor único.


Por ejemplo, en la *Publish.proj* de archivos en la solución de ejemplo, eche un vistazo a la **BuildProjects** destino.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


En este ejemplo, se pueden observar estos puntos clave:

- Si el **BuildingInTeamBuild** parámetro está especificado y tiene un valor de **true**, se ejecutará ninguna de las tareas dentro de este destino.
- El destino contiene una sola instancia de la [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) tarea. Esta tarea le permite crear otros proyectos de MSBuild.
- El **ProjectsToBuild** elemento se pasa a la tarea. Este elemento puede representar una lista de archivos de proyecto o solución, todos los definidos por **ProjectsToBuild** elementos dentro de un grupo de elementos de elemento. En este caso, el **ProjectsToBuild** elemento hace referencia a un solo archivo de solución.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Los valores de propiedad pasados a la **MSBuild** tarea incluir parámetros denominados **OutputRoot** y **configuración**. Estos se establecen en valores de parámetro si se proporcionan o valores de propiedad estática si no lo son.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

También puede ver que el **MSBuild** tarea invoca un destino denominado **generar**. Este es uno de varios destinos integrados que se usan ampliamente en archivos de proyecto de Visual Studio y están disponible en los archivos de proyecto personalizados, como **generar**, **limpiar**, **volver a generar**, y **publicar**. Aprenderá más acerca del uso de destinos y tareas para controlar el proceso de compilación y sobre la **MSBuild** en particular, la tarea más adelante en este tema.

> [!NOTE]
> Para obtener más información sobre los destinos, consulte [destinos de MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Dividir archivos de proyecto para admitir varios entornos

Imagine que desea poder implementar una solución para varios entornos, como servidores de prueba, ensayo plataformas y entornos de producción. La configuración puede variar considerablemente entre estos entornos & #x 2014; no solo en cuanto a los nombres de servidor, cadenas de conexión y así sucesivamente, sino también potencialmente en cuanto a las credenciales, la configuración de seguridad y una gran cantidad de otros factores. Si tiene que hacer esto con regularidad, no resulta muy conveniente modificar varias propiedades en el archivo de proyecto cada vez que se cambia el entorno de destino. Ni es una solución ideal para requerir una lista de valores de propiedad que se proporcionan para el proceso de compilación sin fin.

Afortunadamente, hay una alternativa. MSBuild permite dividir la configuración de compilación a través de varios archivos de proyecto. Para ver cómo funciona esto, en la solución de ejemplo, observe que hay dos archivos de proyecto personalizado:

- *Publish.proj*, que contiene propiedades, elementos, y destinos que son comunes a todos los entornos.
- *Env Dev.proj*, que contiene propiedades que son específicas de un entorno de desarrollo.

Ahora, observe que la *Publish.proj* archivo incluye una [importación](https://msdn.microsoft.com/library/92x05xfs.aspx) elemento inmediatamente debajo de la apertura **proyecto** etiqueta.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


El **importar** elemento se usa para importar el contenido de otro archivo de proyecto de MSBuild en el archivo de proyecto de MSBuild actual. En este caso, el **TargetEnvPropsFile** parámetro proporciona el nombre de archivo del archivo del proyecto que desea importar. Puede proporcionar un valor para este parámetro al ejecutar MSBuild.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Esto combina eficazmente el contenido de los dos archivos en un único archivo de proyecto. Con este enfoque, puede crear un archivo de proyecto que contiene la configuración de compilación universal y varios archivos de proyecto adicionales que contiene propiedades específicas del entorno. Como resultado, basta con ejecutar un comando con un valor de parámetro diferente le permite implementar la solución en un entorno diferente.

![](understanding-the-project-file/_static/image3.png)

Dividir los archivos de proyecto de esta manera es una buena práctica para seguir. Permite a los desarrolladores implementar varios entornos mediante la ejecución de un único comando, al evitar la duplicación de propiedades de compilación universal en varios archivos de proyecto.

> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicas del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Conclusión

En este tema se proporciona una introducción general a los archivos de proyecto de MSBuild y se explica cómo puede crear sus propios archivos de proyecto personalizadas para controlar el proceso de compilación. También introdujo el concepto de dividir los archivos de proyecto en las instrucciones de compilación universal y propiedades de compilación específica del entorno, para que sea fácil de compilar e implementar proyectos en varios destinos.

El siguiente tema, [descripción del proceso de compilación](understanding-the-build-process.md), proporciona más información sobre cómo puede usar archivos de proyecto de implementación y compilación de control, le guía a través de la implementación de una solución con un nivel de complejidad realista.

## <a name="further-reading"></a>Información adicional

Para obtener una introducción más detallada a los archivos de proyecto y el WPP, consulte [dentro de la Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Anterior](setting-up-the-contact-manager-solution.md)
[Siguiente](understanding-the-build-process.md)
