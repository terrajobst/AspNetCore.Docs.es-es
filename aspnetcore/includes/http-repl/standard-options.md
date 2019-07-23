* `-F|--no-formatting`

  Marca cuya presencia suprime el formato de respuesta HTTP.

* `-h|--header`

  Establece un encabezado de solicitud HTTP. Se admiten los dos siguientes formatos de valor:

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  Especifica un archivo en el que se debe escribir toda la respuesta HTTP (incluidos los encabezados y el cuerpo). Por ejemplo, `--response "C:\response.txt"`. Si el archivo no existe, se creará.

* `--response:body`

  Especifica un archivo en el que se debe escribir el cuerpo de respuesta HTTP. Por ejemplo, `--response:body "C:\response.json"`. Si el archivo no existe, se creará.

* `--response:headers`

  Especifica un archivo en el que se deben escribir los encabezados de respuesta HTTP. Por ejemplo, `--response:headers "C:\response.txt"`. Si el archivo no existe, se creará.

* `-s|--streaming`

  Marca cuya presencia habilita la transmisión en secuencias de la respuesta HTTP.
