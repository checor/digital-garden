---
title: "Readiness Probe Warning: Probe terminated redirects"
---
Recientemente, me encontré con este problema en Kubernetes, al revisar los eventos que ocurren en mi cluster:

```bash
$ kubectl get events
2m30s       Warning   ProbeWarning             pod/web-6899f548bc-rffxw              Readiness probe warning: Probe terminated redirects, Response body:
```

Como podemos leer, nos marca una alerta relacionada con una redirección, y un *readiness probe*, el cual es una petición que se hace al pod para saber que está listo para empezar a trabajar.

Sabiendo esto, me puse a revisar que petición se hace para validar su disponibilidad:

```bash
$ kubectl describe pod-6899f548bc-rffxw | grep Readiness:
http://:3000/robots.txt
```

Encontré que hacemos una petición a localhost, nada fuera de lo normal. Lo extraño fue al realizar esta misma petición con curl:

```bash
$ kubectl exec -it pod-899f548bc-rffxw -- curl -i http://localhost:3000/robots.txt -L
HTTP/1.1 301 Moved Permanently  
Content-Type: text/html  
Location: https://localhost:3000/robots.txt  
Transfer-Encoding: chunked
error:1408F10B:SSL routines:ssl3_get_record:wrong version number
```

Tenemos dos problemas aquí:
* Se está mandando un código HTTP diferente al esperado 2xx. Esto no genera la alerta, pero es inesperado.
* Se está haciendo una redirección a HTTPS, cuando es local. Esto genera la alerta.

Sobre la redirección, leyendo los *issues* de Github, resulta que [si se siguen redirecciones](https://github.com/kubernetes/kubernetes/issues/73172) a pesar de que la documentación [no lo dice](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-http-request). Así que lo mejor que podemos hacer, es quitar esa redirección.
## Corrigiendo el problema

Para corregirlo, dependerá del framework que se utilice, pero en este caso, la redirección a HTTPS estaba activa para toda la aplicación.

En Ruby on Rails, esto se puede mitigar, evitando que la URL en cuestión no se force a utilizar HTTPS, como marca la [documentación](https://api.rubyonrails.org/v5.2.1/classes/ActionDispatch/SSL.html):

```ruby
/config/enviroments/production.rb
config.ssl_options = { redirect: { exclude: -> request { request.path =~ /robots.txt/ } } }

```

Y listo! Con esto nos evitamos esta alerta que lanza Kubernetes.