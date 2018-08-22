---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Uso de anotaciones de datos para la validación del modelo | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 6 se describe el uso de anotaciones de datos para el modelo V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b666c3cef0b09c6d68cee581a3c27c08e3357cca
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829689"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="e019b-104">Parte 6: Usar anotaciones de datos para la validación del modelo</span><span class="sxs-lookup"><span data-stu-id="e019b-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="e019b-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e019b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e019b-106">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="e019b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e019b-107">El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="e019b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="e019b-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e019b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e019b-109">Parte 6 se describe el uso de anotaciones de datos para la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="e019b-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="e019b-110">Tenemos un problema importante con los formularios de Create y Edit: no hacen ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="e019b-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="e019b-111">Podemos hacer cosas como dejar los campos obligatorios en blanco o escriba las letras en el campo de precio y el primer error que veremos es desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e019b-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="e019b-112">Podemos agregar validación a nuestra aplicación fácilmente mediante la adición de anotaciones de datos a nuestras clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="e019b-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="e019b-113">Las anotaciones de datos nos permiten describir las reglas que queremos aplicadas a propiedades del modelo y ASP.NET MVC se encargará de aplicarlas y mostrar los mensajes adecuados a nuestros usuarios.</span><span class="sxs-lookup"><span data-stu-id="e019b-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="e019b-114">Agregar validación a los formularios de álbum</span><span class="sxs-lookup"><span data-stu-id="e019b-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="e019b-115">Vamos a usar los siguientes atributos de anotación de datos:</span><span class="sxs-lookup"><span data-stu-id="e019b-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="e019b-116">**Requiere** : indica que la propiedad es un campo obligatorio</span><span class="sxs-lookup"><span data-stu-id="e019b-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="e019b-117">**DisplayName** : define el texto que desea utilizar en los campos de formulario y los mensajes de validación</span><span class="sxs-lookup"><span data-stu-id="e019b-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="e019b-118">**StringLength** : define una longitud máxima para un campo de cadena</span><span class="sxs-lookup"><span data-stu-id="e019b-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="e019b-119">**Intervalo** : da como resultado un valor máximo y mínimo de un campo numérico</span><span class="sxs-lookup"><span data-stu-id="e019b-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="e019b-120">**Enlazar** : una lista de campos para excluir o incluir al enlazar los valores de parámetro o un formulario con las propiedades del modelo</span><span class="sxs-lookup"><span data-stu-id="e019b-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="e019b-121">**ScaffoldColumn** : permite ocultar los campos de formularios de editor</span><span class="sxs-lookup"><span data-stu-id="e019b-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="e019b-122">*Nota: Para obtener más información sobre la validación del modelo utilizando atributos de anotación de datos, vea la documentación de MSDN en*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="e019b-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="e019b-123">Abra la clase de álbum y agregue la siguiente *mediante* instrucciones a la parte superior.</span><span class="sxs-lookup"><span data-stu-id="e019b-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="e019b-124">A continuación, actualice las propiedades para agregar atributos de presentación y la validación tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="e019b-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="e019b-125">Mientras se encuentra allí, también hemos cambiado el género y el intérprete a propiedades virtuales.</span><span class="sxs-lookup"><span data-stu-id="e019b-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="e019b-126">Esto permite a Entity Framework para la carga diferida ellos según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="e019b-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="e019b-127">Después de tener agregados estos atributos en nuestro modelo de álbum, nuestra pantalla de Create y Edit inmediatamente comenzar la validación de campos y con los nombres para mostrar hemos elegido (por ejemplo, álbum arte Url en lugar de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="e019b-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="e019b-128">Ejecute la aplicación y vaya a /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="e019b-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="e019b-129">A continuación, dividiremos algunas reglas de validación.</span><span class="sxs-lookup"><span data-stu-id="e019b-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="e019b-130">Escriba un precio de 0 y deje que el título en blanco.</span><span class="sxs-lookup"><span data-stu-id="e019b-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="e019b-131">Al hacer clic en el botón Crear, vemos el formulario mostrado validación los mensajes de error que muestra los campos que no cumple las reglas de validación que hemos definido.</span><span class="sxs-lookup"><span data-stu-id="e019b-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="e019b-132">Probar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="e019b-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="e019b-133">Validación del lado servidor es muy importante desde una perspectiva de la aplicación, ya que los usuarios pueden eludir la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="e019b-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="e019b-134">Sin embargo, los formularios de página Web que solo implementan la validación de servidor presentan tres problemas importantes.</span><span class="sxs-lookup"><span data-stu-id="e019b-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="e019b-135">El usuario tiene que esperar para que el formulario se registra, se valida en el servidor y para la respuesta se envíe a su explorador.</span><span class="sxs-lookup"><span data-stu-id="e019b-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="e019b-136">El usuario no recibe una respuesta inmediata cuando corrija un campo para que ahora supera las reglas de validación.</span><span class="sxs-lookup"><span data-stu-id="e019b-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="e019b-137">Nos estamos desperdiciar recursos del servidor para llevar a cabo la lógica de validación en lugar de aprovechar el explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="e019b-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="e019b-138">Afortunadamente, las plantillas de scaffolding de ASP.NET MVC 3 tienen la validación del lado cliente integrado, no requerir ningún trabajo adicional de ningún tipo.</span><span class="sxs-lookup"><span data-stu-id="e019b-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="e019b-139">Escribir una sola letra en el campo Título cumple los requisitos de validación, por lo que se quita inmediatamente el mensaje de validación.</span><span class="sxs-lookup"><span data-stu-id="e019b-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="e019b-140">[Anterior](mvc-music-store-part-5.md)
> [Siguiente](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="e019b-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
