---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publicar sitio MVC Database First en Azure | Documentos de Microsoft
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publicar sitio MVC Database First en Azure
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en la aplicación web y la base de datos de publicación en Azure. Puede leer este tema para obtener información acerca de cómo publicar una aplicación web y la base de datos, pero para llevar a cabo los pasos que debe comenzar al principio del tutorial. Vea [Introducción](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Implementar la aplicación web en Azure

Necesita una cuenta de Azure para completar este tutorial:

- También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -obtendrá créditos puede usar para probar los servicios de Azure de pagados e incluso después de que se utilizan hasta puede mantener la cuenta y libre de usar los servicios de Azure.
- También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -su suscripción a MSDN ofrece créditos cada mes que puede usar para los servicios de Azure de pagados.

Para publicar la aplicación web, haga clic en el proyecto y seleccione **publicar**.

![Publicar sitio](publish-to-azure/_static/image1.png)

Seleccione los sitios Web de Microsoft Azure.

![Seleccionar Azure](publish-to-azure/_static/image2.png)

Si no inició sesión en Azure, proporcione sus credenciales de cuenta de Azure. A continuación, seleccione Nuevo para crear una nueva aplicación web.

![nuevo sitio](publish-to-azure/_static/image3.png)

Crear un nombre único para la aplicación web. Podrá saber que el nombre es único si ve una marca de verificación verde a la derecha del campo name. Seleccione una región de la aplicación web. Seleccione **crear nuevo servidor** para la base de datos y proporcione un nombre de usuario y una contraseña para este nuevo servidor de base de datos. Cuando termine, haga clic en **crear**.

![Crear sitio](publish-to-azure/_static/image4.png)

Los valores de conexión ahora están todos establecidos. Puede dejar estos valores sin cambios.

![connection](publish-to-azure/_static/image5.png)

Haga clic en **Siguiente**.

Para la configuración, observe que dos bases de datos las conexiones se especifican - ApplicationDBContext y ContosoUniversityDataEntities. ApplicationDBContext es la conexión para las tablas de la cuenta de usuario. Estos valores solo muestran las cadenas de conexión para las bases de datos. Eso no significa que estas bases de datos se publicará cuando se publica un sitio. Cuando haya terminado de publicar la aplicación web se publicará el proyecto de base de datos.

![](publish-to-azure/_static/image6.png)

Los puntos suspensivos (...) situado junto a la conexión de base de datos muestran los detalles de la cadena de conexión. Haga clic en el botón de puntos suspensivos situado junto a ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Tenga en cuenta el nombre del servidor de base de datos y la base de datos. El nombre del servidor se genera aleatoriamente. El nombre de la base de datos es sencillo el nombre del sitio con  **\_db** anexado. Necesitará dos nombres posteriormente al publicar la base de datos.

Haga clic en **Aceptar** para cerrar la ventana de cadena de conexión de base de datos.

En la ventana de la publicación Web, haga clic en **siguiente** para ver la vista previa.

![](publish-to-azure/_static/image8.png)

Puede hacer clic en Inicio de vista previa para ver una lista de los archivos que se va a publicar. Puesto que esta es la primera vez que se ha publicado este sitio, la lista es todos los archivos pertinentes en el proyecto.

Haga clic en **Publicar**.

El panel de resultados muestra los resultados de la publicación.

![](publish-to-azure/_static/image9.png)

Después de la publicación, el sitio es immedialely iniciada en un explorador web. El sitio se ha implementado y se puede registrar un nuevo usuario en el sitio; Sin embargo, las tablas en el proyecto ContosoUniversityData aún no se han publicado. Si hace clic en la lista de estudiantes vínculo recibirá un error.

## <a name="publish-database-to-sql-azure"></a>Publicar la base de datos en SQL Azure

Antes de publicar la base de datos, debe asegurarse de que el equipo local puede conectarse al servidor de base de datos. El firewall para el servidor de base de datos restringe que máquinas pueden conectarse a la base de datos. Debe agregar la dirección IP del equipo a las direcciones IP permitidas para el servidor de seguridad.

Inicie sesión en su cuenta de Azure a través del portal de Azure.

Seleccione la base de datos nueva y seleccione **administrar**.

![Administrar](publish-to-azure/_static/image10.png)

Debe configurar el servidor de base de datos para permitir las conexiones desde el equipo. Cuando se selecciona administrar, deberá agregar la dirección IP actual según lo permitido en el servidor de base de datos. Seleccione Sí.

![Agregar dirección ip](publish-to-azure/_static/image11.png)

Hay una posibilidad de que la dirección IP que agregó en el paso anterior no es la única dirección IP que debe configurar para las conexiones. Puede intentar iniciar sesión en la base de datos para ver si las conexiones se han configurado correctamente. Proporcione el usuario y la contraseña que creó anteriormente.

![inicio de sesión](publish-to-azure/_static/image12.png)

Si recibe un mensaje de error, debe agregar otra dirección IP. Haga clic en el mensaje de error para ver más detalles sobre el error. En los detalles, verá la dirección IP que necesite agregar. Tenga en cuenta esta dirección IP.

![no se permite](publish-to-azure/_static/image13.png)

Cierre esta ventana de inicio de sesión y vuelva al portal de Azure.

Ir al panel de la base de datos. Haga clic en **administrar direcciones IP permitida para**.

![administrar direcciones ip](publish-to-azure/_static/image14.png)

Ahora debe agregar la dirección IP desde el mensaje de error. Cambie el intervalo de direcciones IP permitidas para incluir el empezando por el mensaje de error o agregar esa dirección IP como una entrada separada.

![Agregar nueva dirección](publish-to-azure/_static/image15.png)

Guarde el cambio a las direcciones IP permitidas.

Haga clic en administrar y vuelva a conectarse a la base de datos. Debe esperar unos minutos antes de que las direcciones IP permitidas están configuradas correctamente para el servidor de seguridad. Cuando puede iniciar sesión correctamente en la base de datos, haya terminado de configurar su conexión a la base de datos.

Puede dejar esta ventana de administración abierto porque comprobará en el resultado de la implementación de la base de datos, en breve.

Volver a su proyecto de base de datos. Haga clic en el proyecto y seleccione **publicar**.

![publicar la base de datos](publish-to-azure/_static/image16.png)

En la ventana de la base de datos de publicación, seleccione **editar**.

![editar](publish-to-azure/_static/image17.png)

Proporcione el nombre del servidor de base de datos y las credenciales de autenticación para el servidor. Después de proporcionar las credenciales, seleccione la base de datos que creó en la lista de bases de datos disponibles. De forma predeterminada, Visual Studio establece el nombre del campo de base de datos en el nombre del proyecto que puede no ser la misma que la base de datos que creó.

![](publish-to-azure/_static/image18.png)

Haga clic en Aceptar.

Probablemente deseará guardar este perfil, por lo que puede publicar actualizaciones en el futuro sin volver a escribir toda la información de conexión. Seleccione **Crear perfil**.

![Guardar perfil](publish-to-azure/_static/image19.png)

Creará un archivo en el proyecto denominado **ContosoUniversityData.publish.xml**. La próxima vez que desea publicar la base de datos en Azure, basta con cargar dicho perfil.

Ahora, haga clic en **publicar** para crear la base de datos en Azure.

![publicar](publish-to-azure/_static/image20.png)

Después de ejecutarse durante un tiempo, se muestran los resultados de la publicación.

![results](publish-to-azure/_static/image21.png)

Ahora, puede volver al portal de administración de la base de datos. Actualice la vista de diseño y tenga en cuenta que se han implementado las 3 tablas con datos previamente rellenadas.

![nuevas tablas](publish-to-azure/_static/image22.png)

Ahora está listo para probar la aplicación web que se implementa en Azure. Navegue a la aplicación web en Azure (como http://contosositeexample.azurewebsites.net/). Haga clic en el vínculo de la lista de estudiantes y debería ver la vista de índice para estudiantes.

![vista](publish-to-azure/_static/image23.png)

En ocasiones, la conexión y base de datos necesitan un poco de tiempo para que se configure correctamente. Si recibe un error, espere unos minutos y, a continuación, vuelva a intentarlo.

## <a name="conclusion"></a>Conclusión

Esta serie proporciona un ejemplo sencillo de cómo generar código a partir de una base de datos que permite a los usuarios editar, actualizar, crear y eliminar datos. ASP.NET MVC 5, Entity Framework y ASP.NET Scaffolding utilizan para crear el proyecto.

Para obtener un ejemplo de introducción de desarrollo Code First, vea [Introducción a ASP.NET MVC 5](../introduction/getting-started.md).

Para obtener un ejemplo más avanzado, vea [crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Tenga en cuenta que la API de DbContext que usas para trabajar con datos en base de datos en primer lugar es igual a la API que se utiliza para trabajar con datos de Code First. Incluso si piensa usar la primera base de datos, puede aprender a controlar los escenarios más complejos, como leer y actualizar datos relacionados, controlar los conflictos de simultaneidad, etc. de un tutorial de Code First. La única diferencia está en cómo se crean la base de datos, context (clase) y las clases de entidad.

> [!div class="step-by-step"]
> [Anterior](enhancing-data-validation.md)
