# intro-raml
Introducción a RAML con ejemplos.

# ¿Qué es RAML?

_RAML_ es un lenguage de especificación abierta no propietario para describir _APIs REST_,
está basado en [YAML 1.2](http://yaml.org/spec/1.2/spec.html) y _JSON_.

_RAML_ es un lenguaje altamente legible, tanto para personas como máquinas y se enfoca en
describir recursos, métodos, parámetros, respuestas, _media types_ y otras construcciones _HTTP_
que conforman a las _APIs REST_ modernas.


# Definiendo un _API_

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


# Raíz del documento

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
types? | Declaraciones de [tipos de datos](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#defining-types) para uso en la _API_.
traits? | Declaraciones de características [(traits)](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#resource-types-and-traits) para uso en la _API_.
resourceTypes? | Declaraciones de tipos de [recursos](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#resource-types-and-traits) para uso en la _API_.
annotationTypes? | Declaraciones de tipos de [anotación](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#declaring-annotation-types) para uso en las anotaciones.
(\<annotationName\>)? | [Anotaciones](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#annotations) a ser aplicadas en la _API_; una anotación es un mapa que tiene una llave que inicia con `"("` y termina con `")"` donde el texto encerrado en paréntesis es el nombre de la anotación y el valor es una instancia de esa anotación.
securitySchemes? | Declaraciones de [esquemas de seguridad](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#security-schemes) para uso en la _API_.
securedBy? | Los [esquemas de seguridad](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#applying-security-schemes) que aplican a cada recurso y método en la _API_.
uses? | [Librerías](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#libraries) externas importadas para el uso en la _API_.
/\<relativeUri\>? | Los recursos de la _API_ identificados con _URLs_ relativas que comienzan con un slash `/` 

> NOTA: Los nodos _schemas_ y _types_ son mutuamente excluyentes y sinónimos por lo que ambos no deben ser especificados en la definición de la _API_. Se recomienda usar el nodo _types_, ya que _schemas_ está en desuso y será removido en futuras versiones de _RAML_.

> Los nodos marcados con signo de interrogación (?) son opcionales.

## Documentación de usuario.

El nodo opcional **documentation** incluye documentos que sirven como referencia para la _API_; el valor del nodo es una secuencia de documentos, donde cada documento es un mapa que tiene dos pares llave-valor.

Nombre | Descripción
--- | ---
title | El título del documento, debe ser una cadena no vacía.
content | El contenido del documento, debe ser una cadena no vacía y puede estar en formato markdown.

Por ejemplo:

````yaml
#%RAML 1.0
title: basic API
baseUri: https://app.basic.com/api
documentation:
 - title: Home
   content: |
    Bienvenido a la documentación de la _API_ _basic API_. _basic API_ permite conectar
    tu aplicación a nuestros servicios de publicación de noticias.
 - title: Legal
   content: !include docs/legal.markdown
````

## _Base URI_ y _Base URI Parameters_.

El nodo opcional _baseUri_ especifica una _URI_ como un identificador para la _API_ y puede ser usado para especificar el _endpoint_ de la _API_. El valor del nodo _baseUri_ debe seguir la especificación [RFC2396](https://www.ietf.org/rfc/rfc2396.txt) o ser un template _URI_, en caso de ser un template _URI_ debe contener el nodo _version_ definido a nivel de raíz.

> Cualquier otra variable en _baseUri_ debe ser descrita explícitamente dentro del nodo _baseUriParameters_.

Ejemplo de _template URI_ como base _URI_.

````yaml
#%RAML 1.0
title: API REST de consulta de horarios de autobuses.
version: v1.0
baseUri: https://boletos.bus.com/services/data/{version}/horarios
````


Ejemplo con _base URI_ explícito.

````yaml
#%RAML 1.0
title: Amazon S3 REST API
version: 1
baseUri: https://{bucketName}.s3.amazonaws.com
baseUriParameters:
  bucketName:
    description: The name of the bucket
````

> NOTA: Cuando el nodo _baseUri_ termina en uno o más _slashes_, estos son omitidos en los paths absolutos en los recursos que utilizan dicha _baseUri_.

Por ejemplo. para los siguientes recursos: `http://api.test.com/common/users` y `http://api.test.com/common/users/groups`

````yaml
baseUri: http://api.test.com/common/
/users:
  /groups:
````


## Protocolos

El nodo _protocols_ especifica los protocolos que soporta la _API_, si el protocolo no es especificado explícitamente, uno o más protocolos definidos en _baseUri_ serán usados, si el protocolo se especifica, se sobreescribe cualquier protocolo especificado en _baseUri_.

El nodo _protocols_ debe ser un arreglo de strings no vacíos, de valores _HTTP_ o _HTTPS_ y es sensible a mayúsculas.

Ejemplo:

````yaml
#%RAML 1.0
title: Ejemplo de REST API
version: v1.0
protocols: [ HTTP, HTTPS ]
baseUri: https://dev.restapi.com/services/data/{version}
````

## _media types_ por omisión.

Al especificar el nodo _mediaTypes_ se configura el tipo de medio por omisión para los _requests_ y _responses_ que tienen un cuerpo de mensaje. El valor de _mediaTypes_ debe ser una cadena o arreglo que represente el tipo de medio y aplica a los _requests_ que tienen cuerpo de mensaje, a los _responses_ esperados y a los ejemplos que usan la misma secuencia de _media types_.

Por ejemplo, para especificar una _API_ que acepta y responde JSON, se realiza lo siguiente:

````yaml
#%RAML 1.0
title: New API
mediaType: application/json
````

Para una _API_ que acepta y responde JSON o XML:

````yaml
#%RAML 1.0
title: New API
mediaType: [ application/json, application/xml ]
````

Es posible definir un _mediaType_ explícitamente para sobreescribir el definido por omisión.
En el siguiente ejemplo, la _API_ acepta y responde tanto JSON como XML, para el recurso `/send` se sobreescribe el _mediaType_ para que responda únicamente JSON.

````yaml
#%RAML 1.0
title: New API
mediaType: [ application/json, application/xml ]
types:
  Person:
  Another:
/list:
  get:
    responses:
      200:
        body: Person[]
/send:
  post:
    body:
      application/json:
        type: Another
````

## Seguridad

Al especificar el nodo _securedBy_, se configuran los esquemas de seguridad para cada método de cada recurso en la _API_. El valor del nodo debe ser un arreglo con los nombres de los esquemas de seguridad.

Ejemplo:


````yaml
#%RAML 1.0
title: Dropbox API
version: 1
baseUri: https://api.dropbox.com/{version}
securedBy: [oauth_2_0]
securitySchemes:
  oauth_2_0: !include securitySchemes/oauth_2_0.raml
  oauth_1_0: !include securitySchemes/oauth_1_0.raml
/users:
  get:
    securedBy: [oauth_2_0, oauth_1_0]
````


# Definición de recursos

Un recurso es identificado por su _URI_ relativo el cual debe iniciar con un slash ("/").
Un recurso definido a nivel de raíz, es llamado recurso de alto nivel o _top-level resource_. Un recurso definido como hijo de otro recurso, es llamado  recurso anidado.


Ejemplo de definición de recursos, `/gists` como recurso de alto nivel y `/public` como recurso anidado:

````yaml
#%RAML 1.0
title: GitHub API
version: v3
baseUri: https://api.github.com
/gists:
  displayName: Gists
  /public:
    displayName: Public Gists
````


Un recurso anidado puede a su vez tener un recurso hijo, de tal forma que existan recursos con múltiples anidamientos. Como en el siguiente ejemplo:


````yaml
#%RAML 1.0
title: GitHub API
version: v3
baseUri: https://api.github.com
/user:
/users:
  /{userId}:
    uriParameters:
      userId:
        type: integer
    /followers:
    /following:
    /keys:
      /{keyId}:
        uriParameters:
          keyId:
            type: integer
````

Las _URIs_ resueltas para los recursos arriba descritos quedan como sigue, siguiendo el mismo orden en que fueron declaradas.

````text
https://api.github.com/user
https://api.github.com/users
https://api.github.com/users/{userId}
https://api.github.com/users/{userId}/followers
https://api.github.com/users/{userId}/following
https://api.github.com/users/{userId}/keys
https://api.github.com/users/{userId}/keys/{keyId}
````

> NOTA: La definición de las _URIs_ deben ser únicas.

El siguiente ejemplo muestra _URIs_ duplicadas:

````yaml
/users:
  /foo:
/users/foo:
````

En cambio, la siguiente definición es correcta:

````yaml
/users/{userId}:
/users/{username}:
/users/me:
````

# RAML en acción.

Ejemplo de definición de _API_ para el recurso login.

[./examples/login/login.raml](./examples/login/login.raml)

Ejemplo de API con _types_ y mensajes de respuesta.

[./examples/mobile/api.raml](./examples/mobile/api.raml)


# Herramientas:
[Osprey mock service](https://github.com/mulesoft-labs/osprey-mock-service)


# Referencias:
* [raml.org](https://raml.org/)
* [Tutorial RAML 1.0](http://www.baeldung.com/raml-restful-api-modeling-language-tutorial)
* [Especificación de RAML](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md)
* [Uso de types](https://github.com/raml-org/raml-spec/blob/master/versions/raml-10/raml-10.md#defining-types)