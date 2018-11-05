---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publicar sitio MVC Database First en Azure | Microsoft Docs
author: Rick-Anderson
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 1d2c26c211c5c8d97076327d01fe59d5ba4dc9ac
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021643"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publicar sitio MVC Database First en Azure
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en la aplicación web y la base de datos de publicación en Azure. Puede leer este tema para obtener información sobre cómo publicar una aplicación web y la base de datos, pero para llevar a cabo los pasos que debe empezar al principio del tutorial. Consulte [Introducción](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Implementar la aplicación web en Azure

Necesita una cuenta de Azure para completar este tutorial:

- También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -obtenga créditos puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.
- También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.

Para publicar la aplicación web, haga clic en el proyecto y seleccione **publicar**.

![Publicar el sitio](publish-to-azure/_static/image1.png)

Seleccione los sitios Web de Microsoft Azure.

![Seleccione Azure](publish-to-azure/_static/image2.png)

Si no inició sesión en Azure, proporcione sus credenciales de cuenta de Azure. A continuación, seleccione Nuevo para crear una nueva aplicación web.

![nuevo sitio](publish-to-azure/_static/image3.png)

Crear un nombre único para la aplicación web. Indica que el nombre es único si ve una marca de verificación verde a la derecha del campo name. Seleccione una región de la aplicación web. Seleccione **crear nuevo servidor** para la base de datos y proporcione un nombre de usuario y una contraseña para este nuevo servidor de base de datos. Cuando termine, haga clic en **crear**.

![Crear sitio](publish-to-azure/_static/image4.png)

Los valores de conexión están ahora listo. Puede dejar estos valores sin modificar.

![connection](publish-to-azure/_static/image5.png)

Haga clic en **Siguiente**.

Para la configuración, tenga en cuenta que dos bases de datos las conexiones se especifican - ApplicationDBContext y ContosoUniversityDataEntities. ApplicationDBContext es la conexión para las tablas de la cuenta de usuario. Estos valores solo muestran las cadenas de conexión para las bases de datos. No significa que se va a publicar estas bases de datos al publicar su sitio. Cuando haya terminado de publicar la aplicación web se publicará el proyecto de base de datos.

![](publish-to-azure/_static/image6.png)

Los puntos suspensivos (...) junto a la conexión de base de datos muestran los detalles de la cadena de conexión. Haga clic en el botón de puntos suspensivos situado junto a ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Tenga en cuenta el nombre del servidor de base de datos y de la base de datos. El nombre del servidor se genera aleatoriamente. El nombre de la base de datos es sencillo el nombre de su sitio con  **\_db** anexado. Necesitará ambos nombres más adelante al publicar la base de datos.

Haga clic en **Aceptar** para cerrar la ventana de cadena de conexión de base de datos.

En la ventana de publicación Web, haga clic en **siguiente** para ver la vista previa.

![](publish-to-azure/_static/image8.png)

Puede hacer clic en iniciar vista previa para ver una lista de los archivos para publicar. Puesto que es la primera vez que se ha publicado este sitio, la lista es todos los archivos pertinentes en el proyecto.

Haga clic en **Publicar**.

El panel de resultados mostrará el resultado de la publicación.

![](publish-to-azure/_static/image9.png)

Después de la publicación, el sitio es immedialely iniciado en un explorador web. Se ha implementado el sitio y puede registrar un nuevo usuario al sitio; Sin embargo, las tablas en el proyecto ContosoUniversityData aún no se han publicado. Si hace clic en la lista de vínculos de los estudiantes recibirán un error.

## <a name="publish-database-to-sql-azure"></a>Publicar la base de datos en SQL Azure

Antes de publicar la base de datos, debe asegurarse de que el equipo local puede conectarse al servidor de base de datos. El firewall para el servidor de base de datos restringe que las máquinas pueden conectarse a la base de datos. Debe agregar la dirección IP del equipo a las direcciones IP permitidas para el firewall.

Inicie sesión en su cuenta de Azure a través del portal de Azure.

Seleccione la base de datos y seleccione **administrar**.

![administrar](publish-to-azure/_static/image10.png)

Debe configurar el servidor de base de datos para permitir conexiones desde su equipo. Al seleccionar administrar, deberá agregar la dirección IP actual, según lo permitido en el servidor de base de datos. Seleccione Sí.

![Agregar dirección ip](publish-to-azure/_static/image11.png)

Es probable que la dirección IP que agregó en el paso anterior no es la única dirección IP que se debe configurar para las conexiones. Puede intentar iniciar sesión en la base de datos para ver si las conexiones se han configurado correctamente. Proporcione el usuario y contraseña que creó anteriormente.

![inicio de sesión](publish-to-azure/_static/image12.png)

Si recibe un mensaje de error, deberá agregar otra dirección IP. Haga clic en el mensaje de error para ver más detalles sobre el error. En los detalles, verá la dirección IP que se debe agregar. Tenga en cuenta esta dirección IP.

![no se permite](publish-to-azure/_static/image13.png)

Cierre esta ventana de inicio de sesión y vuelva al portal de Azure.

Vaya al panel de la base de datos. Haga clic en **administrar direcciones IP permitidas**.

![administración de direcciones ip](publish-to-azure/_static/image14.png)

Ahora debe agregar la dirección IP desde el mensaje de error. Cambie el intervalo de direcciones IP permitidas para incluir uno de los mensajes de error o agregar esa dirección IP como una entrada independiente.

![Agregar nueva dirección](publish-to-azure/_static/image15.png)

Guarde el cambio a las direcciones IP permitidas.

Haga clic en administrar y vuelva a conectarse a la base de datos. Es posible que deba esperar unos minutos antes de que las direcciones IP permitidas están configuradas correctamente para el firewall. Cuando puede iniciar sesión correctamente en la base de datos, se haya terminado de configurar la conexión a la base de datos.

Puede dejar abierta esta ventana de administración porque se comprobará el resultado de la implementación de la base de datos en breve.

Vuelva a su proyecto de base de datos. Haga clic en el proyecto y seleccione **publicar**.

![publicar base de datos](publish-to-azure/_static/image16.png)

En la ventana de la base de datos de publicación, seleccione **editar**.

![editar](publish-to-azure/_static/image17.png)

Proporcione el nombre del servidor de base de datos y de las credenciales de autenticación para el servidor. Después de proporcionar las credenciales, seleccione la base de datos que creó en la lista de bases de datos disponibles. De forma predeterminada, Visual Studio establece el nombre del campo de base de datos en el nombre del proyecto que puede no ser la misma que la base de datos que creó.

![](publish-to-azure/_static/image18.png)

Haga clic en Aceptar.

Probablemente deseará guardar este perfil, por lo que puede publicar actualizaciones en el futuro sin volver a escribir toda la información de conexión. Seleccione **Crear perfil**.

![Guardar perfil](publish-to-azure/_static/image19.png)

Creará un archivo en el proyecto denominado **ContosoUniversityData.publish.xml**. La próxima vez que desea publicar la base de datos en Azure, simplemente cargue dicho perfil.

Ahora, haga clic en **publicar** para crear la base de datos en Azure.

![publicar](publish-to-azure/_static/image20.png)

Después de ejecutarse durante un tiempo, se muestran los resultados de la publicación.

![results](publish-to-azure/_static/image21.png)

Ahora, puede volver al portal de administración para la base de datos. Actualice la vista de diseño y observe que se han implementado las 3 tablas con datos previamente rellenadas.

![nuevas tablas](publish-to-azure/_static/image22.png)

Ahora está listo para probar la aplicación web que se implementa en Azure. Vaya a la aplicación web en Azure (como http://contosositeexample.azurewebsites.net/). Haga clic en el vínculo para obtener la lista de alumnos y debería ver la vista de índice para estudiantes.

![vista](publish-to-azure/_static/image23.png)

En ocasiones, la base de datos y la conexión necesitan un poco de tiempo para configurarse correctamente. Si recibe un error, espere unos minutos y, a continuación, vuelva a intentarlo.

## <a name="conclusion"></a>Conclusión

Esta serie proporciona un ejemplo sencillo de cómo generar código a partir de una base de datos existente que permite a los usuarios editar, actualizar, crear y eliminar datos. ASP.NET MVC 5, Entity Framework y ASP.NET Scaffolding usan para crear el proyecto.

Para obtener un ejemplo introductorio de desarrollo Code First, consulte [Introducción a ASP.NET MVC 5](../introduction/getting-started.md).

Para obtener un ejemplo más avanzado, consulte [crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Tenga en cuenta que la API de DbContext que usan para trabajar con datos en la primera base de datos es igual a la API que se utiliza para trabajar con datos en Code First. Incluso si piensa usar la primera base de datos, puede aprender a administrar los escenarios más complejos, como leer y actualizar datos relacionados, controlar los conflictos de simultaneidad, y así sucesivamente de un tutorial de Code First. La única diferencia está en cómo se crean la base de datos, clase de contexto y clases de entidad.

> [!div class="step-by-step"]
> [Anterior](enhancing-data-validation.md)
