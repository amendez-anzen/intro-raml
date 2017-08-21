# intro-raml
Introducción a RAML con ejemplos.

## ¿Qué es RAML?

_RAML_ es un lenguage de especificación abierta no propietario para describir _APIs REST_,
está basado en [YAML 1.2](http://yaml.org/spec/1.2/spec.html) y _JSON_.

_RAML_ es un lenguaje altamente legible, tanto para personas como máquinas y se enfoca en
describir recursos, métodos, parámetros, respuestas, _media types_ y otras construcciones _HTTP_
que conforman a las _APIs REST_ modernas.


## Definiendo un _API_

Crear un archivo de texto con extensión _.raml_ y agregar las siguientes líneas:

````yaml
#%RAML 1.0
title: My first REST API
version: v1
protocols: [ HTTPS ] 
baseUri: http://myapi.mysite.com/api/{version}
mediaType: application/json
````

La primer línea  de un documento _RAML_ debe comenzar con el texto `#%RAML` seguido por un espacio simple, seguido por el texto `1.0` y nada más antes del
fin de línea. Documentos de fragmento de _RAML_ comienzan de forma similar con
el comentario de la versión de _RAML_ y un [identificador de fragmento](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#typed-fragments).


## Raíz del documento

La sección raíz o _root_ del documento _RAML_ describe la información básica acerca de la _API_,
como su título y versión; la raíz también define los _assets_ utilizados en algún lugar del
documento, como _types_ y _traits_.


Nodos en la definición de la _API_ pueden aparecer en cualquier orden, los procesadores deben preservar dicho orden.


Name | Description
--- | ---
title | Una etiqueta para describir la _API_, debe ser una cadena.
description? | La descripción de la _API_, puede estar formateada usando _Markdown_.
version? | La versión de la _API_, por ejemplo "v1"
baseUri? |	Una URI que sirve como la base _URIs_ de todos los recursos.
baseUriParameters? | Parámetros nombrados usados en el _baseUri_ (template).
protocols? | Los protocolos soportados por la _API_.
mediaType? | El tipo de medio por omisión que será utilizado en el cuerpo de cada _request_ y _response_ (_payloads_), por ejemplo `"application/json"`-
documentation? | Documentación adicional de la _API_.
schemas? | Un alias para el equivalente nodo `types` para compatibilidad con _RAML 0.8_; en desuso, soporta _schemas_ _XML_ y _JSON_.
types? | Declaraciones de tipos de datos para uso en la _API_.
traits? | Declaraciones de características para uso en la _API_.
resourceTypes? | Declaraciones de tipos de recursos para uso en la _API_.








# Referencias:
* [raml.org](https://raml.org/)
* [Tutorial RAML 1.0](http://www.baeldung.com/raml-restful-api-modeling-language-tutorial)
