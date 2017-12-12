---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: "Información general sobre ASP.NET MVC | Documentos de Microsoft"
author: microsoft
description: "Obtenga información sobre las diferencias entre la aplicación de ASP.NET MVC y aplicaciones de formularios Web Forms de ASP.NET. Obtenga información acerca de cómo decidir cuándo se debe crear una aplicación de ASP.NET MVC."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-overview"></a>Información general sobre ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre las diferencias entre la aplicación de ASP.NET MVC y aplicaciones de formularios Web Forms de ASP.NET. Obtenga información acerca de cómo decidir cuándo se debe crear una aplicación de ASP.NET MVC.


El modelo de arquitectura Model-View-Controller (MVC) separa una aplicación en tres componentes principales: el modelo, la vista y el controlador. El marco de ASP.NET MVC proporciona una alternativa al modelo de formularios Web Forms de ASP.NET para crear aplicaciones Web basadas en MVC. El marco de ASP.NET MVC es un marco de presentación ligera, alta comprobable que (al igual que con las aplicaciones basadas en formularios Web Forms) se integra con las características ASP.NET existentes, como páginas maestras y la autenticación basada en pertenencia. El marco de MVC se define en el **System.Web.Mvc** espacio de nombres y es una parte fundamental y compatible de la **System.Web** espacio de nombres.   
  
MVC es un modelo de diseño estándar que están familiarizados con muchos desarrolladores. Algunos tipos de aplicaciones Web se beneficiarán del marco de MVC. Otros usuarios continuarán utilizando el modelo de aplicación ASP.NET tradicional basado en formularios Web Forms y devoluciones. Otros tipos de aplicaciones Web combinarán las dos estrategias; no excluye a la otra.   
  
El marco de MVC incluye los siguientes componentes:


[![Invocar una acción de controlador que espera un valor de parámetro](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Figura 01**: invocar una acción de controlador que espera un valor de parámetro ([haga clic aquí para ver la imagen a tamaño completo](asp-net-mvc-overview/_static/image2.png))


- **Modelos**. Objetos de modelo son las partes de la aplicación que implementan la lógica del dominio de aplicación s datos. A menudo, los objetos del modelo recuperar y almacenan el estado del modelo en una base de datos. Por ejemplo, un objeto del producto puede recuperar la información de una base de datos, trabajar con ella y, a continuación, escribir la información actualizada en una tabla de productos de SQL Server.

En las aplicaciones pequeñas, el modelo es a menudo una separación conceptual en lugar de física. Por ejemplo, si la aplicación solo lee un conjunto de datos y lo envía a la vista, la aplicación no tiene un nivel de modelo físico y las clases asociadas. En ese caso, el conjunto de datos asume el rol de un objeto de modelo.

- **Vistas**. Las vistas son los componentes que muestra la interfaz de usuario de aplicación s (UI). Normalmente, esta interfaz de usuario se crea a partir de los datos del modelo. Un ejemplo sería una vista de edición de una tabla de productos que muestra cuadros de texto, listas desplegables y casillas basándose en el estado actual de un objeto de productos.

- **Controladores de**. Los controladores son los componentes que controlen la interacción del usuario, trabajan con el modelo y por último seleccionan una vista para representar la interfaz de usuario. En una aplicación MVC, la vista solo muestra información; el controlador administra y responde a los proporcionados por el usuario y la interacción. Por ejemplo, el controlador administra los valores de cadena de consulta y pasa estos valores para el modelo, que a su vez consulta la base de datos mediante el uso de los valores.

El modelo MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica comercial y lógica de la interfaz de usuario), y proporciona un vago acoplamiento entre estos elementos. El patrón especifica dónde se debe encontrar cada tipo de lógica en la aplicación. La lógica de la interfaz de usuario pertenece a la vista. La lógica de entrada pertenece al controlador. La lógica de negocios pertenece al modelo. Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que le permite centrarse en un aspecto de la implementación a la vez. Por ejemplo, puede centrarse en la vista sin estar condicionado la lógica de negocios.   
  
Además de administrar la complejidad, el modelo de MVC resulta más fácil probar las aplicaciones que probar una aplicación Web ASP.NET basada en formularios Web Forms. Por ejemplo, en una aplicación Web ASP.NET basada en formularios Web Forms, una sola clase se utiliza para mostrar el resultado y para responder a los proporcionados por el usuario. Escribir pruebas automatizadas para aplicaciones ASP.NET basadas en formularios Web Forms puede ser complejo, porque para probar una página individual, deben crear una instancia de la clase de página, todos sus controles secundarios y clases dependientes adicionales de la aplicación. Dado que se crean instancias de tantas clases para ejecutar la página, puede ser difícil escribir pruebas que centran exclusivamente en partes individuales de la aplicación. Pruebas para las aplicaciones ASP.NET basadas en formularios Web Forms pueden ser por consiguiente más difíciles de implementar que las pruebas en una aplicación MVC. Además, las pruebas en una aplicación ASP.NET basada en formularios Web Forms requieren un servidor Web. El marco de MVC desacopla los componentes y hace un uso intensivo de las interfaces, lo que permite probar los componentes individuales aislados del resto de framework.   
  
El acoplamiento flexible entre los tres componentes principales de una aplicación MVC también favorece el desarrollo paralelo. Por ejemplo, un desarrollador de software puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y una tercera puede Centrar en la lógica de negocios en el modelo.

## <a name="deciding-when-to-create-an-mvc-application"></a>Decidir cuándo se debe crear una aplicación MVC

Debe considerar cuidadosamente si desea implementar una aplicación Web mediante el marco de ASP.NET MVC o el modelo de formularios Web Forms de ASP.NET. El marco de MVC no reemplaza el modelo de formularios Web Forms; Puede usar cualquier marco de aplicaciones Web. (Si tiene aplicaciones existentes basadas en formularios Web Forms, estas seguirán funcionando exactamente igual que siempre.)   
  
Antes de decidirse a usar el marco de MVC o el modelo de formularios Web Forms para un sitio Web concreto, sopese las ventajas de cada enfoque.

### <a name="advantages-of-an-mvc-based-web-application"></a>Ventajas de una aplicación Web basada en MVC

El marco de ASP.NET MVC ofrece las siguientes ventajas:

- Resulta más fácil de administrar la complejidad al dividir una aplicación en el modelo, la vista y el controlador.
- No utiliza el estado de vista ni formularios basados en servidor. Esto hace que el marco de MVC ideal para los desarrolladores que deseen un control completo sobre el comportamiento de una aplicación.
- Usa un modelo de controlador frontal que procesa las solicitudes de aplicación Web a través de un único controlador. Esto le permite diseñar una aplicación que admite una infraestructura de enrutamiento avanzada. Para obtener más información, consulte [controlador frontal](https://go.microsoft.com/fwlink/?LinkId=106357 "controlador frontal") en el sitio Web de MSDN.
- Proporciona la mejor compatibilidad para el desarrollo controlado por pruebas (TDD).
- Funciona bien para las aplicaciones Web que son compatibles con equipos grandes de desarrolladores y diseñadores Web que necesitan un alto grado de control sobre el comportamiento de la aplicación.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Ventajas de una aplicación Web basada en formularios de Web

El marco de trabajo basado en formularios Web Forms ofrece las siguientes ventajas:

- Admite un modelo de eventos que conserva el estado a través de HTTP, lo que beneficia a desarrollo de aplicaciones Web de línea de negocio. La aplicación basada en formularios Web Forms proporciona docenas de eventos que se admiten en centenares de controles de servidor.
- Usa un modelo de controlador de página que agrega funcionalidad a las páginas individuales. Para obtener más información, consulte [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") en el sitio Web de MSDN.
- Usa el estado de vista ni formularios basados en servidor, que pueden facilitar la administración de información de estado.
- Funciona bien para los equipos pequeños de desarrolladores Web y diseñadores que deseen aprovechar el gran número de componentes disponibles para el desarrollo rápido de aplicaciones.
- En general, es menos complejo para el desarrollo de aplicaciones, porque los componentes (la **página** clase, controles etc.) se integran estrechamente y suelen requerir menos código que el modelo MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Características del marco de ASP.NET MVC

El marco de ASP.NET MVC ofrece las siguientes características:

- Separación de tareas de aplicación (lógica de entrada, lógica comercial y lógica de la interfaz de usuario), facilidad para pruebas y desarrollo controlado por pruebas (TDD) de forma predeterminada. Todos los contratos principales del marco de MVC están basados en la interfaz y se pueden probar mediante objetos ficticios, que son objetos simulados que imitan el comportamiento de objetos reales en la aplicación. Hacer una prueba unitaria la aplicación sin tener que ejecutar los controladores en un proceso ASP.NET, lo que permite la rápida y flexible de pruebas unitarias. Puede usar cualquier marco de pruebas unitarias que sea compatible con .NET Framework.
- Un marco extensible y conectable. Los componentes del marco de ASP.NET MVC están diseñados para que puedan ser reemplazados o personalizar fácilmente. Puede conectar su propio motor de vista, directiva de enrutamiento de URL, serialización de parámetros de método de acción y otros componentes. El marco de ASP.NET MVC también admite el uso de modelos de contenedor inyección de dependencia (DI) e inversión de Control (IOC). DI permite insertar objetos en una clase, en lugar de depender de la clase para crear el objeto propiamente dicho. IOC especifica que si un objeto requiere otro objeto, el primer objeto debe obtener el segundo objeto de un origen externo como un archivo de configuración. Esto facilita las pruebas.
- Un eficaz componente de asignación de dirección URL que le permite compilar aplicaciones que tienen direcciones URL comprensibles y que admiten búsquedas. Las direcciones URL no tiene que incluir las extensiones de nombre de archivo y están diseñadas para admitir patrones de nombres de dirección URL que funcionan bien para la búsqueda del motor de optimización (SEO) y transferencia de representational state Transfer (REST de) direccionamiento.
- Compatibilidad para usar el marcado en la página ASP.NET existentes (archivos .aspx), control de usuario (archivos .ascx) y archivos de marcado (archivos. master) de página maestra como plantillas de vista. Puede usar las características ASP.NET existentes con el marco de MVC de ASP.NET, como páginas maestras anidadas, expresiones en línea (&lt;% = %&gt;), controles de servidor declarativos, plantillas, enlace de datos, localización y así sucesivamente.
- Compatibilidad con características de ASP.NET existentes. ASP.NET MVC le permite usar características como la autenticación de formularios y autenticación de Windows, autorización de dirección URL, pertenencia y roles, salida y almacenamiento en caché de datos, administración de Estados de sesión y perfil, supervisión de estado, el sistema de configuración y el proveedor arquitectura.
