---
title: Generar llave secreta para Rails
---
¿Alguna vez te ha pasado que tu aplicación de Rails falla de la siguiente manera?

```
rails aborted!
ArgumentError: Missing `secret_key_base` for 'production' environment, set this string with `bin/rails credentials:edit`
/workspace/config/environment.rb:5:in `<main>'
Tasks: TOP => environment
```

El cual resulta un poco confuso, dado que el comando que marca, lo que hace es crear o editar un archivo .yml, algo que quieres evitar si el archivo es efímero, como ocurre en Kubernetes.

La mejor opción en este caso es generar una llave, y posteriormente guardarla como una variable de entorno.

```
$ rake secret
```

Y guarda el valor obtenido en una variable de entorno, o mejor, en un administrador de contraseñas.