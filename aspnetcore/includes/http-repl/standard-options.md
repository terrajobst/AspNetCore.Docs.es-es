* `-F|--no-formatting`

  <span data-ttu-id="cde54-101">Marca cuya presencia suprime el formato de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cde54-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="cde54-102">Establece un encabezado de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="cde54-102">Sets an HTTP request header.</span></span> <span data-ttu-id="cde54-103">Se admiten los dos siguientes formatos de valor:</span><span class="sxs-lookup"><span data-stu-id="cde54-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="cde54-104">Especifica un archivo en el que se debe escribir toda la respuesta HTTP (incluidos los encabezados y el cuerpo).</span><span class="sxs-lookup"><span data-stu-id="cde54-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="cde54-105">Por ejemplo, `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="cde54-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="cde54-106">Si el archivo no existe, se creará.</span><span class="sxs-lookup"><span data-stu-id="cde54-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="cde54-107">Especifica un archivo en el que se debe escribir el cuerpo de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cde54-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="cde54-108">Por ejemplo, `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="cde54-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="cde54-109">Si el archivo no existe, se creará.</span><span class="sxs-lookup"><span data-stu-id="cde54-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="cde54-110">Especifica un archivo en el que se deben escribir los encabezados de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cde54-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="cde54-111">Por ejemplo, `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="cde54-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="cde54-112">Si el archivo no existe, se creará.</span><span class="sxs-lookup"><span data-stu-id="cde54-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="cde54-113">Marca cuya presencia habilita la transmisión en secuencias de la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cde54-113">A flag whose presence enables streaming of the HTTP response.</span></span>
