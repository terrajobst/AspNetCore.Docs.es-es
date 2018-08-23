---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Database First con MVC de ASP.NET: mejora de la validación de datos | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: riande
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: dbff33a1c4f47474adda82e796d9c292392142f2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835960"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF Database First con MVC de ASP.NET: mejora de la validación de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos.
> 
> Esta parte de la serie se centra en Agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y formato para mostrar. Se ha mejorado en función de los comentarios de los usuarios en la sección Comentarios.


## <a name="add-data-annotations"></a>Agregar anotaciones de datos

Como se vio en un tema anterior, algunas reglas de validación de datos se aplican automáticamente a la entrada del usuario. Por ejemplo, solo puede proporcionar un número para la propiedad de nivel. Para especificar más de las reglas de validación de datos, puede agregar anotaciones de datos a la clase de modelo. Estas anotaciones se aplican en toda la aplicación web para la propiedad especificada. También puede aplicar atributos de formato que cambiar cómo se muestran las propiedades; el valor utilizado para las etiquetas de texto, como el cambio.

En este tutorial, agregará las anotaciones de datos para restringir la longitud de los valores proporcionados para las propiedades FirstName, LastName y MiddleName. En la base de datos, estos valores se limitan a 50 caracteres. Sin embargo, en la aplicación web ese límite de caracteres actualmente no se aplica. Si un usuario proporciona más de 50 caracteres para uno de esos valores, la página se bloqueará al intentar guardar el valor en la base de datos. También limitará el grado de valores entre 0 y 4.

Abra el **Student.cs** de archivos en el **modelos** carpeta. Agregue el código resaltado siguiente a la clase.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

En Enrollment.cs, agregue el siguiente código resaltado.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Compile la solución.

Vaya a la página de edición o creación de un alumno. Si se intenta escribir más de 50 caracteres, se muestra un mensaje de error.

![Mostrar mensaje de error](enhancing-data-validation/_static/image1.png)

Vaya a la página para editar las inscripciones e intenta proporcionar un nivel por encima de 4.

![error de intervalo de nivel](enhancing-data-validation/_static/image2.png)

Para obtener una lista completa de las anotaciones de validación de datos se puede aplicar a las clases y propiedades, vea [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Agregar clases de metadatos

Agregar los atributos de validación directamente a la clase del modelo funciona cuando no tiene previsto cambiar; la base de datos Sin embargo, si los cambios de la base de datos y necesita volver a generar la clase del modelo, perderá todos los atributos que se había aplicado a la clase de modelo. Este enfoque puede ser muy ineficaz y propenso a la pérdida de las reglas de validación importante.

Para evitar este problema, puede agregar una clase de metadatos que contiene los atributos. Al asociar la clase del modelo a la clase de metadatos, esos atributos se aplican al modelo. En este enfoque, puede volver a generar la clase del modelo sin perder todos los atributos que se han aplicado a la clase de metadatos.

En el **modelos** carpeta, agregue una clase denominada **Metadata.cs**.

![Agregar clase de metadatos](enhancing-data-validation/_static/image3.png)

Reemplace el código en Metadata.cs con el código siguiente.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Estas clases de metadatos contienen todos los atributos de validación que había aplicado anteriormente a las clases del modelo. El **mostrar** atributo se utiliza para cambiar el valor utilizado para las etiquetas de texto.

Ahora, debe asociar las clases del modelo con las clases de metadatos.

En el **modelos** carpeta, agregue una clase denominada **PartialClasses.cs**.

Reemplace el contenido del archivo con el código siguiente.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Tenga en cuenta que cada clase está marcada como un `partial` clase y cada uno de ellos coincide con el nombre y espacio de nombres como la clase que se genera automáticamente. Al aplicar el atributo de metadatos a la clase parcial, se asegura que se aplicará a la clase genera automáticamente los atributos de validación de datos. Estos atributos no se perderán al volver a generar las clases del modelo porque se aplica el atributo de metadatos en clases parciales que no se vuelven a generar.

Para volver a generar las clases generadas automáticamente, abra el archivo ContosoModel.edmx. Una vez más, haga doble clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**. Aunque no ha cambiado la base de datos, este proceso volverá a generar las clases. En el **actualizar** ficha, seleccione **tablas** y **finalizar**.

![actualizar tablas](enhancing-data-validation/_static/image4.png)

Guarde el archivo ContosoModel.edmx para aplicar los cambios.

Abra el archivo Student.cs o Enrollment.cs y tenga en cuenta que los atributos de validación de datos que aplicó anteriormente ya no están en el archivo. Sin embargo, ejecute la aplicación y observe que todavía se aplican las reglas de validación al escribir datos.

> [!div class="step-by-step"]
> [Anterior](customizing-a-view.md)
> [Siguiente](publish-to-azure.md)
