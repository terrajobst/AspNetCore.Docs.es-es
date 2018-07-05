---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Implementación Web de ASP.NET con Visual Studio: implementación en producción | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 5bc39162bc53dc27230f8ccc55266212a51b6380
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369650"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Implementación Web de ASP.NET con Visual Studio: implementación en producción
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

En este tutorial, configure una cuenta de Microsoft Azure, cree entornos de ensayo y producción e implementar la aplicación web ASP.NET para el almacenamiento provisional y característica de publicación de los entornos de producción mediante el uso de Visual Studio con un solo clic.

Si lo prefiere, puede implementar en un proveedor de hospedaje de terceros. La mayoría de los procedimientos descritos en este tutorial es los mismos para un proveedor de hospedaje o de Azure, excepto que cada proveedor tiene su propia interfaz de usuario para la administración de cuenta y el sitio web. Puede encontrar un proveedor de hospedaje en el [Galería de proveedores](https://www.microsoft.com/web/hosting) en el sitio web Microsoft.com.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, asegúrese de comprobar la página de solución de problemas en esta serie de tutoriales.

## <a name="get-a-microsoft-azure-account"></a>Obtener una cuenta de Microsoft Azure

Si aún no tiene una cuenta de Azure, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Crear un entorno de ensayo

> [!NOTE]
> Desde que se escribió este tutorial, Azure App Service agrega una nueva característica para automatizar muchos de los procesos para crear entornos de ensayo y producción. Consulte [establecer entornos de ensayo para aplicaciones web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Como se explica en el [implementar en el tutorial sobre entornos de prueba](deploying-to-iis.md), más el entorno de prueba confiable es un sitio web en el proveedor de hospedaje que simplemente como el sitio web de producción. En muchos de los proveedores de hospedaje tendría que sopesar las ventajas de esto frente al costo adicional significativo, pero en Azure puede crear una aplicación web gratuita adicionales como la aplicación de ensayo. También necesita una base de datos y el gasto adicional para que a través de los gastos de la base de datos de producción será ninguno o mínima. En Azure se paga para la cantidad de almacenamiento de base de datos que use en lugar de para cada base de datos, y la cantidad de almacenamiento adicional que usará en ensayo será mínima.

Como se explica en el [implementar en el tutorial de entorno de prueba](deploying-to-iis.md), de ensayo y producción que se va a implementar las dos bases de datos en una base de datos. Si desea mantenerlos independiente, el proceso podría ser el mismo, salvo que debe crear una base de datos adicional para cada entorno y seleccionaría la cadena de destino correcto para cada base de datos al crear el perfil de publicación.

En esta sección del tutorial creará una aplicación web y la base de datos que se usará para el entorno de ensayo y a implementar en ensayo y prueba existe antes de crear e implementar en el entorno de producción.

> [!NOTE]
> Los pasos siguientes muestran cómo crear una aplicación web en Azure App Service mediante el portal de administración de Azure. En la versión más reciente del SDK de Azure, también puede hacerlo sin salir de Visual Studio, utilizando el Explorador de servidores. En Visual Studio 2013, también puede crear una aplicación web directamente desde el cuadro de diálogo Publicar. Para obtener más información, consulte [crear una aplicación web ASP.NET en Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. En el [Portal de administración de Azure](https://manage.windowsazure.com/), haga clic en **Websites**y, a continuación, haga clic en **New**.
2. Haga clic en **sitio Web**y, a continuación, haga clic en **creación personalizada**.

    El **nuevo sitio Web - Creación personalizada** abre el asistente. El **creación personalizada** asistente le permite crear un sitio web y una base de datos al mismo tiempo.
3. En el **crear sitio Web** paso del asistente, escriba una cadena en el **URL** cuadro que se usará como la dirección URL única para la aplicación del entorno de ensayo. Por ejemplo, escriba ContosoUniversity-staging123 (incluidos números aleatorios al final para que sea único en caso de que se toma ContosoUniversity ensayo).

    La dirección URL completa consistirá en lo que escriba más el sufijo que aparece junto al cuadro de texto.
4. En el **región** lista desplegable, elija la región más cercana a usted.

    Esta configuración especifica el centro de datos que se ejecutará la aplicación web en.
5. En el **base de datos** lista desplegable, elija **crear una nueva base de datos SQL**.
6. En el **nombre de cadena de conexión de base de datos** cuadro, deje el valor predeterminado, *DefaultConnection*.
7. Haga clic en la flecha que señala a la derecha en la parte inferior del cuadro.

    La siguiente ilustración muestra el **crear sitio Web** cuadro de diálogo con los valores de ejemplo en ella. La dirección URL y la región que ha escrito será diferente.

    ![Crear sitio Web paso](deploying-to-production/_static/image1.png)

    El asistente avanza a la **especificar la configuración de la base de datos** paso.
8. En el **nombre** , escriba *ContosoUniversity* junto con un número aleatorio para que sea único, por ejemplo *ContosoUniversity123*.
9. En el **Server** cuadro, seleccione **nuevo servidor de base de datos de SQL Server**.
10. Escriba un nombre de administrador y una contraseña.

    No se escribe un nombre existente y la contraseña aquí. Está especificando un nuevo nombre y una contraseña que definirá ahora para usarlos más adelante cuando acceda a la base de datos.
11. En el **región** , seleccione la misma región que eligió para la aplicación web.

    Mantener el servidor web y el servidor de base de datos en la misma región ofrece el mejor rendimiento y reduce los gastos.
12. Haga clic en la marca de verificación en la parte inferior de la casilla para indicar que haya terminado.

    La siguiente ilustración muestra el **especificar la configuración de la base de datos** cuadro de diálogo con los valores de ejemplo en ella. Los valores que ha escrito pueden ser diferentes.

    ![Paso de configuración de la base de datos del sitio Web de New - crear con el Asistente para la base de datos](deploying-to-production/_static/image2.png)

    El Portal de administración que se devuelve a la página de sitios Web y el **estado** columna muestra que se está creando la aplicación web. Después de un tiempo (normalmente menos de un minuto), el **estado** columna muestra que la aplicación web se creó correctamente. En la barra de navegación a la izquierda el número de aplicaciones web que tiene en su cuenta aparece junto a la **Websites** aparece junto al icono y el número de bases de datos la **bases de datos SQL** icono.

    ![Página de sitios Web del Portal de administración, el sitio web creado](deploying-to-production/_static/image3.png)

    Nombre de la aplicación web será diferente de la aplicación de ejemplo en la ilustración.

## <a name="deploy-the-application-to-staging"></a>Implementar la aplicación en ensayo

Ahora que ha creado una aplicación web y la base de datos para el entorno de ensayo, puede implementar el proyecto a él.

> [!NOTE]
> Estas instrucciones muestran cómo crear un perfil de publicación descargando un *.publishsettings* archivo, que no solo funciona para Azure, sino también para los proveedores de hospedaje de terceros. El último SDK de Azure también permite conectarse directamente a Azure desde Visual Studio y elija entre una lista de aplicaciones web que tiene en su cuenta de Azure. En Visual Studio 2013, puede iniciar sesión Azure desde el **Publicar Web** diálogo o desde el **Explorador de servidores** ventana. Para obtener más información, consulte [crear una aplicación web ASP.NET en Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Descargue el archivo .publishsettings

1. Haga clic en el nombre de la aplicación web que acaba de crear.

    ![Haga clic en el sitio para ir al panel](deploying-to-production/_static/image4.png)
2. En **primea** en el **panel** , haga clic **descargar perfil de publicación**.

    ![Perfil de publicación vínculo de descarga](deploying-to-production/_static/image5.png)

    Este paso descarga un archivo que contiene toda la configuración que necesita para implementar una aplicación en la aplicación web. Por lo que no tendrá que escribir esta información manualmente, importará este archivo en Visual Studio.
3. Guardar el *.publishsettings* archivo en una carpeta que pueda acceder desde Visual Studio.

    ![guardar el archivo .publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Seguridad: el *.publishsettings* archivo contiene sus credenciales (sin codificar) que se usan para administrar sus servicios y suscripciones de Azure. La práctica recomendada de seguridad para este archivo consiste en almacenarlo temporalmente fuera de los directorios de origen (por ejemplo, en la carpeta bibliotecas\documentos) y, a continuación, eliminarlo una vez completada la importación. Un usuario malintencionado que obtuviera acceso a la *.publishsettings* archivo puede editar, crear y eliminar los servicios de Azure.

### <a name="create-a-publish-profile"></a>Crear un perfil de publicación

1. En Visual Studio, haga clic en el proyecto ContosoUniversity en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.

    El **publicación Web** abre el asistente.
2. Haga clic en el **perfil** ficha.
3. Haga clic en **importación**.
4. Navegue hasta la *.publishsettings* archivo que descargó anteriormente y, a continuación, haga clic en **abierto**.

    ![Cuadro de diálogo Importar configuración de publicación](deploying-to-production/_static/image7.png)
5. En el **conexión** , haga clic **validar conexión** para asegurarse de que la configuración es correcta.

    Cuando se haya validado la conexión, se muestra junto a una marca de verificación verde el **validar conexión** botón.

    Para algunos proveedores de hospedaje, al hacer clic en **validar conexión**, es posible que vea un **Error de certificado** cuadro de diálogo. Si lo hace, compruebe que el nombre del servidor es el esperado. Si el nombre del servidor es correcto, seleccione **guardar este certificado para sesiones futuras de Visual Studio** y haga clic en **Accept**. (Este error significa que el proveedor de hospedaje ha elegido evitar el gasto de adquirir un certificado SSL para la dirección URL que se va a implementar en. Si desea establecer una conexión segura con un certificado válido, póngase en contacto con su proveedor de hospedaje.)
6. Haga clic en **Siguiente**.

    ![icono de conexión correcta y el botón siguiente en la pestaña conexión](deploying-to-production/_static/image8.png)
7. En el **configuración** , expanda **File Publish Options**y, a continuación, seleccione **excluir archivos de la aplicación\_carpeta de datos**.

    Para obtener información acerca de las demás opciones bajo **File Publish Options**, consulte el [implementar en IIS](deploying-to-iis.md) tutorial. La captura de pantalla que muestra el resultado de este paso y los siguientes pasos de configuración de base de datos es el final de los pasos de configuración de la base de datos.
8. En **DefaultConnection** en el **bases de datos** sección, configurará una implementación base de datos para la base de datos de pertenencia.
9. 1. Seleccione **Actualizar base de datos**.

        El **cadena de conexión remota** directamente debajo del cuadro **DefaultConnection** se rellena con la cadena de conexión desde el archivo .publishsettings. La cadena de conexión incluye las credenciales de SQL Server, que se almacenan en texto sin formato en el *.pubxml* archivo. Si prefiere no almacenarlas de forma permanente no existe, puede quitarlos del perfil de publicación una vez implementada la base de datos y almacenarlos en Azure. Para obtener más información, consulte [cómo mantener la base de datos ASP.NET las cadenas de conexión segura cuando se implementa en Azure desde el origen](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) en el blog de Scott Hanselman.
      2. Haga clic en **configurar actualizaciones de base de datos**.
      3. En el **configurar actualizaciones de base de datos** cuadro de diálogo, haga clic en **Agregar secuencia de comandos SQL**.
      4. En el **Agregar secuencia de comandos SQL** , navegue hasta la *aspnet-data-prod.sql* script que guardó anteriormente en la carpeta de soluciones y, a continuación, haga clic en **abierto**.
      5. Cerrar la **configurar actualizaciones de base de datos** cuadro de diálogo.
10. En **SchoolContext** en el **bases de datos** sección, seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)**.

    Visual Studio muestra **ejecutar migraciones Code First** en lugar de **Actualizar base de datos** para `DbContext` clases. Si desea utilizar el proveedor dbDacFx en lugar de las migraciones para implementar una base de datos que tener acceso mediante un `DbContext` de clases, vea [cómo se puede implementar una base de datos Code First sin migraciones?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) en las preguntas más frecuentes de implementación de Web para Visual Studio y ASP.NET en MSDN.

    El **configuración** ficha ahora es similar al ejemplo siguiente:

    ![Pestaña de configuración para el almacenamiento provisional](deploying-to-production/_static/image9.png)
11. Realice los pasos siguientes para guardar el perfil y cambie su nombre a *ensayo*:

    1. Haga clic en el **perfil** pestaña y, a continuación, haga clic en **administrar perfiles**.
    2. La importación crea nuevos perfiles de dos, uno para FTP y otro para Web Deploy. Configura el perfil de Web Deploy: cambiar el nombre de este perfil para *ensayo*.

        ![Cambiar el nombre de perfil en el almacenamiento provisional](deploying-to-production/_static/image10.png)
    3. Cerrar la **Editar perfiles de publicación Web** cuadro de diálogo.
    4. Cerrar la **publicación Web** asistente.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurar una transformación de perfil de publicación para el indicador de entorno

> [!NOTE]
> En esta sección se muestra cómo configurar una transformación de Web.config para el indicador de entorno. Dado que el indicador tiene el `<appSettings>` elemento, tiene otra alternativa para especificar la transformación cuando se va a implementar en Azure App Service. Para obtener más información, consulte [Web.config especificando su configuración](web-config-transformations.md#watransforms).


1. En **el Explorador de soluciones**, expanda **propiedades**y, a continuación, expanda **PublishProfiles**.
2. Haga clic en *Staging.pubxml*y, a continuación, haga clic en **Agregar transformación Config**.

    ![Agregar transformación de configuración para el almacenamiento provisional](deploying-to-production/_static/image11.png)

    Visual Studio crea el *Web.Staging.config* archivo de transformación y lo abre.
3. En el *Web.Staging.config* transformar el archivo, inserte el código siguiente inmediatamente después de la apertura `configuration` etiqueta.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Cuando se usa el perfil de publicación de ensayo, esta transformación establece el indicador de entorno a "Prod". En la aplicación web implementada, no verá ningún sufijo como "(desarrollo)" o "(prueba)" después del encabezado H1 "Contoso University".
4. Haga clic en el *Web.Staging.config* de archivo y haga clic en **Preview transformar** para asegurarse de que la transformación ha codificado genera los cambios previstos.

    El **Web.config Preview** ventana muestra el resultado de aplicar el *Web.Release.config* transforma y *Web.Staging.config* transforma.

### <a name="prevent-public-use-of-the-test-app"></a>Evitar un uso público de la aplicación de prueba

Una consideración importante para la aplicación de ensayo es que estará activa en Internet, pero no desea que el público para usarlo. Para reducir la probabilidad de que las personas encuentren y usarlo, puede usar uno o varios de los métodos siguientes:

- Establecer reglas de firewall que permiten acceder a la aplicación de ensayo solo desde direcciones IP que usan para probar el almacenamiento provisional.
- Use una URL confusa que sería imposible de adivinar.
- Crear un *robots.txt* archivo para asegurarse de que los motores de búsqueda no rastreará los vínculos de informe y la aplicación de prueba a él en los resultados de búsqueda.

El primero de estos métodos es más eficaz, pero no se trata en este tutorial, ya que requeriría que implemente en un servicio de nube de Azure en lugar de Azure App Service. Para obtener más información acerca de los servicios en la nube y las restricciones de IP en Azure, consulte [opciones de proceso de hospedaje proporcionadas por Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) y [bloque de direcciones IP específicas accedan a un rol Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Si va a implementar en un proveedor de hospedaje de terceros, póngase en contacto con el proveedor para obtener información sobre cómo implementar restricciones de IP.

Para este tutorial, creará un *robots.txt* archivo.

1. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **Agregar nuevo elemento**.
2. Cree un nuevo **archivo de texto** denominado *robots.txt*e inserte el siguiente texto en ella:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    El `User-agent` línea indica los motores de búsqueda que se aplican las reglas en el archivo a todos los rastreadores de web de motor de búsqueda (robots), y el `Disallow` línea especifica que no se deben rastrear las páginas del sitio.

    Desea que los motores de búsqueda en el catálogo de la aplicación de producción, por lo que necesita excluir este archivo de implementación de producción. Para ello, deberá configurar una configuración de la producción de perfil de publicación al crearla.

### <a name="deploy-to-staging"></a>Implementar en ensayo

1. Abra el **publicación Web** Asistente haciendo clic en el proyecto de Contoso University y haga clic en **publicar**.
2. Asegúrese de que el **ensayo** está seleccionado el perfil.
3. Haga clic en **Publicar**.

    El **salida** ventana muestra qué acciones de implementación se realizaron e informa la correcta finalización de la implementación. El explorador predeterminado se abre automáticamente en la dirección URL de la aplicación web implementada.

## <a name="test-in-the-staging-environment"></a>Probar en el entorno de ensayo

Tenga en cuenta que el indicador de entorno esté ausente (no hay ninguna "(prueba)" o "(desarrollo)" después del encabezado H1, que muestra que el *Web.config* transformación para el indicador de entorno fue correcta.

![Almacenamiento provisional de la página principal](deploying-to-production/_static/image12.png)

Ejecute el **estudiantes** página para comprobar que la base de datos implementada no tiene ningún estudiante.

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos de un instructor:

Seleccione **agregar estudiantes** desde el **estudiantes** menú, agregue un estudiante y, a continuación, ver el alumno nuevo en el **estudiantes** página para comprobar que puede escribir correctamente en la base de datos .

Desde el **cursos** página, haga clic en **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **en el registro** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "prodpwd"). El **actualización créditos** página se muestra, que comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

Solicitar una dirección URL no válida para producir un error que ELMAH realizará el seguimiento y, a continuación, solicitar el informe de errores ELMAH. Si va a implementar en un proveedor de hospedaje de terceros, es posible que el informe está vacío para la misma razón que estaba vacío en el tutorial anterior. Tendrá que usar las herramientas de administración de cuenta del proveedor de hospedaje para configurar permisos de carpeta para habilitar ELMAH escribir en la carpeta de registro.

Ahora se ejecuta la aplicación que creó en la nube en una aplicación web que es igual que usará para la producción. Puesto que todo funciona correctamente, el siguiente paso es implementar en producción.

## <a name="deploy-to-production"></a>Implementar en producción

El proceso para crear una aplicación web de producción y la implementación en producción es el mismo que para el almacenamiento provisional, salvo que debe excluir el *robots.txt* de implementación. Hacer modificará el archivo de perfil de publicación.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Crear el entorno de producción y el perfil de publicación de la producción

1. Crear la aplicación web de producción y la base de datos en Azure, el mismo procedimiento que usó para el almacenamiento provisional.

    Cuando se crea la base de datos, puede colocarlo en el mismo servidor que creó anteriormente, o crear un nuevo servidor.
2. Descargue el *.publishsettings* archivo.
3. Crear el perfil de publicación mediante la importación de la producción *.publishsettings* archivo, el mismo procedimiento que usó para el almacenamiento provisional.

    No olvide configurar el script de implementación de datos en **DefaultConnection** en el **bases de datos** sección de la **configuración** ficha.
4. Cambiar el nombre de perfil de publicación para *producción*.
5. Configurar una transformación de perfil de publicación para el indicador de entorno, el mismo procedimiento que usó para el almacenamiento provisional...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edite el archivo .pubxml para excluir robots.txt

Los archivos se denominan de perfil de publicación &lt;profilename&gt;*.pubxml* y se encuentran en el *PublishProfiles* carpeta. El *PublishProfiles* carpeta está bajo el *propiedades* carpeta en una aplicación web de C# del proyecto, en el *mi proyecto* carpeta en un proyecto de aplicación web VB o en el *Aplicación\_datos* carpeta en un proyecto de aplicación web. Cada *.pubxml* archivo contiene valores que se aplican a un perfil de publicación. Los valores que especifique en el Asistente para publicación Web se almacenan en estos archivos, y puede modificarlos para crear o cambiar la configuración que no están disponibles en la IU de Visual Studio.

De forma predeterminada, *.pubxml* archivos se incluyen en el proyecto cuando se crea un perfil de publicación, pero puede excluir del proyecto y Visual Studio todavía se usarán. Visual Studio busca en el *PublishProfiles* carpeta *.pubxml* archivos, independientemente de si se incluyen en el proyecto.

Para cada *.pubxml* archivo hay una *. pubxml.user* archivo. El *. pubxml.user* archivo contiene la contraseña cifrada si ha seleccionado la **Guardar contraseña** opción y se excluye del proyecto de forma predeterminada.

Un *.pubxml* archivo contiene los valores que pertenecen a un perfil de publicación específicos. Si desea configurar las opciones que se aplican a todos los perfiles, puede crear un *. wpp.targets* archivo. El proceso de compilación importa estos archivos en el *.csproj* o *.vbproj* archivo de proyecto, por lo que la mayoría de las opciones que se puede configurar en el archivo de proyecto puede configurarse en estos archivos. Para obtener más información acerca de *.pubxml* archivos y *. wpp.targets* archivos, consulte [Cómo: modificar la configuración de implementación en los archivos de perfil de publicación (.pubxml) y el. wpp.targets archivo en Visual Studio Proyectos Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. En **el Explorador de soluciones**, expanda **propiedades** y expanda **PublishProfiles**.
2. Haga clic en *Production.pubxml* y haga clic en **abierto**.

    ![Abra el archivo .pubxml](deploying-to-production/_static/image13.png)
3. Haga clic en *Production.pubxml* y haga clic en **abierto**.
4. Agregue las líneas siguientes inmediatamente antes del cierre `PropertyGroup` elemento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    El archivo .pubxml ahora es similar al ejemplo siguiente:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Para obtener más información acerca de cómo excluir archivos y carpetas, consulte [puedo excluir determinados archivos o carpetas de la implementación?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) en el **preguntas más frecuentes de implementación de Web para Visual Studio y ASP.NET** en MSDN.

### <a name="deploy-to-production"></a>Implementar en producción

1. Abra el **publicación Web** asistente, asegúrese de que el **producción** perfil de publicación está seleccionada y, a continuación, haga clic en **iniciar vista previa** en el **Preview**pestaña para comprobar que la *robots.txt* no se copiará el archivo en la aplicación de producción.

    ![Vista previa de archivos se publiquen en producción](deploying-to-production/_static/image14.png)

    Revise la lista de archivos que se va a copiar. Verá que todos los *.cs* archivos, incluidos *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, y  *Master.Designer.cs* se omiten los archivos. Todo este código se ha compilado en el *ContosoUniversity.dll* y *ContosUniversity.pdb* archivos que encontrará en el *bin* carpeta. Dado que solo el *.dll* es necesario para ejecutar la aplicación y se especificó anteriormente que se deben implementar sólo los archivos necesarios para ejecutar la aplicación, no *.cs* se copiaron los archivos en el destino entorno. El *obj* carpeta y el *ContosoUniversity.csproj* y *. csproj.user* se omiten los archivos por la misma razón.

    Haga clic en **publicar** para implementar en el entorno de producción.
2. Probar en producción, siga el mismo procedimiento que usó para el almacenamiento provisional.

    Todo lo que es idéntico de ensayo, excepto la dirección URL y la ausencia de la *robots.txt* archivo.

## <a name="summary"></a>Resumen

Ahora ha implementado y probado la aplicación web y está disponible públicamente a través de Internet.

![Producción de la página principal](deploying-to-production/_static/image15.png)

En el siguiente tutorial, deberá actualizar el código de la aplicación e implementar el cambio en los entornos de prueba, ensayo y producción.

> [!NOTE]
> Mientras la aplicación está en uso en el entorno de producción debe implementar un plan de recuperación. Es decir, debe ser periódicamente copia de seguridad de las bases de datos desde la aplicación de producción en una ubicación de almacenamiento seguro y debe mantener varias generaciones de dichas copias de seguridad. Cuando se actualiza la base de datos, debe realizar una copia de seguridad desde inmediatamente antes del cambio. A continuación, si comete un error y no detectarlo hasta después de haber implementado en producción, aún podrá recuperar la base de datos al estado que tenía antes de resultó dañado. Para obtener más información, consulte [copia de seguridad de Azure SQL Database y Restore](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> En este tutorial, el servidor SQL Server edition que se va a implementar en es Azure SQL Database. Aunque el proceso de implementación es similar a otras ediciones de SQL Server, una aplicación de producción real puede requerir código especial para Azure SQL Database en algunos escenarios. Para obtener más información, consulte [trabajar con Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) y [elegir entre SQL Server y Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Anterior](setting-folder-permissions.md)
> [Siguiente](deploying-a-code-update.md)
