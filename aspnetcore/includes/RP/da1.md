# <a name="update-the-generated-pages"></a><span data-ttu-id="6ffb6-101">Actualización de las páginas generadas</span><span class="sxs-lookup"><span data-stu-id="6ffb6-101">Update the generated pages</span></span>

<span data-ttu-id="6ffb6-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6ffb6-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6ffb6-103">La aplicación de películas pinta bien, pero la presentación no es ideal.</span><span class="sxs-lookup"><span data-stu-id="6ffb6-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="6ffb6-104">No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** debe ser **Release Date** (dos palabras).</span><span class="sxs-lookup"><span data-stu-id="6ffb6-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![La aplicación Movie se abre en Chrome y muestra datos de la película](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="6ffb6-106">Actualización del código generado</span><span class="sxs-lookup"><span data-stu-id="6ffb6-106">Update the generated code</span></span>

<span data-ttu-id="6ffb6-107">Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas mostradas en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ffb6-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](code/Models/Movie.cs?highlight=2,11-12)]
