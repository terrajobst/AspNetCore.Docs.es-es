El separador `:` no funciona con claves jerárquicas de variables de entorno en todas las plataformas. `__`, el carácter de subrayado doble, tiene las siguientes características:

* Es compatible con todas las plataformas. Por ejemplo, el separador `:` no es compatible con [Bash](https://linuxhint.com/bash-environment-variables/), pero `__` sí.
* Se reemplaza automáticamente por un signo `:`.