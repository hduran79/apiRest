# Guia API Rest

---
- [Guia API Rest](#guia-api-rest)
  - [Guía para crear una API Rest](#guía-para-crear-una-api-rest)
  - [¿Qué es la arquitectura REST?](#qué-es-la-arquitectura-rest)
  - [Características que presenta la arquitectura REST:](#características-que-presenta-la-arquitectura-rest)
  - [Seguridad en servicios rest](#seguridad-en-servicios-rest)
  - [Diseñar bien api rest](#diseñar-bien-api-rest)
    - [📌 1. Representar recursos y no acciones](#-1-representar-recursos-y-no-acciones)
    - [📌 2. Sobrecarga de API con demasiados parámetros 🎛️](#-2-sobrecarga-de-api-con-demasiados-parámetros-️)
    - [📌 3. No devolver siempre 200 Ok](#-3-no-devolver-siempre-200-ok)
    - [📌 4. No hacer todo con POST](#-4-no-hacer-todo-con-post)
    - [📌 5. Seguridad en las APIs](#-5-seguridad-en-las-apis)
    - [📌 6. Versionar las APIs](#-6-versionar-las-apis)
    - [📌 7. Permitir solicitudes ilimitadas (sin límite de velocidad) 🚀](#-7-permitir-solicitudes-ilimitadas-sin-límite-de-velocidad-)
    - [📌 8. Devolver demasiados datos sin paginación 📄](#-8-devolver-demasiados-datos-sin-paginación-)
    - [📌 9.  Ignorar la documentación de la API 📖](#-9--ignorar-la-documentación-de-la-api-)
    - [📌 10. Pruebas y monitoreo](#-10-pruebas-y-monitoreo)
    - [📌 11. Usa JSON, pero úsalo bien](#-11-usa-json-pero-úsalo-bien)
    - [📌 12. Optimización y rendimiento](#-12-optimización-y-rendimiento)
    - [NOTAS](#notas)
- [Reglas de una arquitectura REST](#reglas-de-una-arquitectura-rest)

## Guía para crear una API Rest

- https://opensource.zalando.com/restful-api-guidelines/#meta-information
- https://notasweb.me/entrada/guia-completa-de-buenas-practicas-para-construir-api-restful-seguras-y-escalables/

## ¿Qué es la arquitectura REST?

REST es una arquitectura de desarrollo web que puede ser utilizada en cualquier cliente HTTP. Además, es mucho más simple que otras arquitecturas ya existentes, como pueden ser XML-RPC o SOAP. Esta simplicidad se consigue porque emplea una interfaz web que usa hipermedios para la representación y transición de la información.

Esta arquitectura fue definida por Roy Fielding en el año 2000, que además es unos de los principales artífices de la especificación del protocolo HTTP.

La principal ventaja de esta arquitectura es que ha aportado a la web una mayor escalabilidad, es decir, dan soporte a un mayor número de componentes y las interacciones entre ellos. 

## Características que presenta la arquitectura REST:
- Es un protocolo sin estado, debido a que no se guarda la información en el servidor. Es decir, toda la información será enviada por el cliente en cada mensaje HTTP, consiguiendo un ahorro en variables de sesión y almacenamiento interno del servidor.
- Presenta un conjunto de operaciones bien definidas, como GET, POST, PUT y DELETE, que se emplea en todos los recursos.
- Utiliza URIs únicas siguiendo una sintaxis universal.
- Emplea hipermedios para representar la información, que suelen ser HTML, XML o JSON.

## Seguridad en servicios rest

1.  **Utilizar HTTPS** => La información viaja de manera mas segura porque va cifrada (let's encrypt permite obtener certificados de manera gratis)

2.  **Poner autenticación y autorización en los endpoints** => **_`Autenticación`_** El usuario se debe de identificar de alguna forma y enviar un token de sesión sino no puede acceder al endpoint y **_`autorización`_** Verificar si el usuario tiene permiso para ejecutar el endpoint.
    
    :bulb: **_`Tip`_**: No basta con solo tener una autenticación.

3.  **Utilizar Json Web Tokens (JWT)** => Cuando el usuario se autentique en la APP se debe generar un JWT es uan practica que funciona muy bien y son seguros, una alternativa es utilizar **_`API Keys`_** la desventaja es que estas API Keys son estáticas a no ser que se cambie manualmente, a diferencia de JWT tienen un tiempo de vida.
    
    :bulb: **_`Tip`_**: Existe una plataforma **_`OAth0`_** que nos permite añadir elementos de seguridad como iniciar session a un sistema y este se encarga de la generación del JWT.

    JWT es un estándar de código abierto basado en JSON para crear tokens de acceso que nos permiten securizar las comunicaciones entre cliente y servidor

4.  **No poner información sensible en la URL** => Es un problema porque hay servidores que guardan información de las URLs en sus logs.
    
    :bulb: **_`Tip`_** enviar información sensible por los **`headers`**, por ejemplo los tokens de session.

5.  **Validar los parámetros de entrada** => Esto evita la inyección de código.

## Diseñar bien api rest

¿Para qué sirve API Rest? Recuperar información de una página.

### 📌 1. Representar recursos y no acciones

- Utilice sustantivos en plural para los recursos ( /users, /products).
- Evite los verbos en puntos finales (por ejemplo, /createUser /users).
- Para facilitar la lectura, utilice guiones ( /user-profilesen lugar de /userProfiles).
- Las URI recibirán nombres que no deben implicar una acción, es decir, se debe evitar colocar verbos en ellas. Esto se debe a que se pone énfasis a los sustantivos.
- Deben ser únicas, no debemos tener más de una URI para identificar un mismo recurso.
- Deben ser independiente de formato, es decir, no debe representar ninguna extensión.
- Deben mantener una jerarquía lógica. La jerarquía es el criterio por el que se ordena los elementos.
- Evitar verbos en los endpoints: Los métodos HTTP ya nos dan una idea de lo que está haciendo la solicitud. Así que en lugar de /createUser, utiliza el método POST en el recurso /users.
- Jerarquía limitada: Si tienes relaciones entre recursos (como usuarios y publicaciones), evita anidar más de dos niveles. Por ejemplo, usa /users/123/publicaciones en lugar de algo como /usuarios/123/publicaciones/456/comentarios.

Los filtrados de información de un recurso no se hacen en la URI.

* GET: consultar información de un recurso.
* POST: crear un recurso nuevo.
* PUT: modificar un recurso existente.
* DELETE: eliminar un recurso determinado.
* PATCH: modificar solamente un atributo de un recurso.

```javascript
Mal ejemplo: ❌
GET /deleteproduct/123  =>  De esta forma estamos mostrando acciones que queremos ejecutar.
GET /pictures/12/delete => utiliza un verbo
GET /pictures/12/user/231 => foto 12 tiene usuario 231, situación poco lógica
GET /pictures/date/october => filtrar datos con el mes de octubre.

✔ Buen ejemplo: ✅
GET /product => Esta es la forma correcta para devolver los prodcutos. 
POST /product => Esta es la forma correcta para crear prodcutos. 
DELETE /product/123  =>  Esta es la forma correcta para ejecutar la acción de borrar. 

GET /pictures/12 => Obtiene los datos de la foto con el id 12
GET /pictures?date=octubre => filtrar datos con el mes de octubre.
GET /user/231/picture/12 => usuario 231 tiene foto 12, algo que suena más lógico.

GET /clients/231/project/ => debe volver a la lista de proyectos del cliente 231.
GET /clients/231/project/4 => debe volver al proyecto nº 4 del cliente 231.
```

### 📌 2. Sobrecarga de API con demasiados parámetros 🎛️

Demasiados parámetros en una solicitud de API reducen la legibilidad y la usabilidad .
- Utilice paginación para conjuntos de datos grandes.
- Mantenga los parámetros de consulta mínimos y significativos .
- Utilice filtros cuando sea necesario ( GET /users?status=active).

```javascript
Mal ejemplo: ❌
GET /users?sortBy=name&order=asc&page=2&limit=10&includeInactive=true
✔ Problema : Difícil de mantener y leer.

✔ Buen ejemplo: ✅ (utilice los parámetros de consulta con moderación)
GET /users?page=2&limit=10
```

### 📌 3. No devolver siempre 200 Ok

https://www.izertis.com/es/-/blog/mejores-practicas-para-su-api-rest

ES una mala practica devolver 200 para todo

Ejemplo: Vamos a guardar un cliente y el api me devuelve:

```javascript
Mal ejemplo: ❌

Request
POST /customers

Response
HTTP/1.1 200 OK
Date: Fri, 28 Enero 2022 19:03:01 GMT
Content-Type: application/json
{
    "error": true,
    "message": "Invalid ID"
}

✔ Buen ejemplo: ✅
Response
HTTP/1.1 400 OK
Date: Fri, 28 Enero 2022 19:03:01 GMT
Content-Type: application/json
{
    "error": true,
    "message": "Invalid ID"
}
```
> Nota: En el cuerpo de la respuesta nos envía un json indicando que hubo un error pero el httpCode llega un 200, las API-Rest están diseñadas para tener significado a partir de los métodos y códigos de respuesta, el deber ser es que llega el código http de lo que realmente esta pasando.

> Solución: Utilizar códigos  HTTP para dar significado a las respuestas
- 200 OK. Respuesta estándar para peticiones correctas.
- 201 -> Created. La petición ha sido completada.
- 202 -> Accepted. En procesamiento (asíncronos), pero no ha sido completado, se pueden hacer otros llamados para verificar si el proceso ya termino.
- 204 -> Solicitud exitosa, Respuesta sin contenido. (Borrar un producto pero no devuelve nada en el cuerpo de la respuesta)
- 400 -> Bad Request. La solicitud contiene sintaxis errónea.
- 401 -> No autorizado (usuario no autenticado)
- 403 -> Forbidden. La solicitud fue legal, No tiene los privilegios. (hay usuarios que pueden listar productos pero no pueden eliminar)
- 404 -> Not Found. Recurso no encontrado. Intenta hacer un GET pero el método es un POST.
- 500 -> Internal Server Error. Son situaciones de error ajenas a la naturaleza del servidor web (TimeOut).

### 📌 4. No hacer todo con POST

Se deben de utilizar los métodos HTTP para indicar la intención. 

* GET: Para Obtener datos (select)
* POST: Crear nuevos recuros (insert)
* PUT: Para actualizar recuros (update) -> Desventaja se debe pasar el recurso completo asi se quiera cambiar un solo campo.
* DELETE: Para eliminar un recurso (delete)
* PATCH: Es la versión mejorada de PUT,  solo se pasan los campos que se quieran cambiar, Muchos frameworks de programación REST todavía no lo soportan
 
### 📌 5. Seguridad en las APIs

Uno de los errores más comunes al empezar es subestimar la importancia de la seguridad. Asegúrate de implementar:

- Métodos de autenticación
  * Api Key
  * Bearer token - OAuth 2.0 o JWT: Estos mecanismos son clave para autenticar a los usuarios.
  * Basic Auth
  * Digest Auth
  * OAuth 1.0
  * OAuth 2.0
  * AWS Signature
  * Akami EdgeGrid

- Utilice OAuth2 o JWT para la autenticación.
- Implementar el control de acceso basado en roles (RBAC) .
- Nunca exponga datos confidenciales en las URL.
- HTTPS: Protege todas las comunicaciones entre tu cliente y servidor, encriptando los datos.
- Validación de entrada: Nunca confíes en los datos que recibes del usuario sin validarlos. Esto puede prevenir ataques como inyecciones SQL o XSS.
- CORS: Configura correctamente CORS para que solo ciertos dominios puedan acceder a tu API.
- Vulnerabilidades de seguridad: como las API pueden tener vulnerabilidades de seguridad, llevar a cabo el desarrollo de tu estructura de API teniendo en cuenta la seguridad es una práctica recomendada para los equipos de desarrollo de API. Las consultas SQL, los marcos y los servidores pueden aparecer en errores devueltos por la API, lo que da a los hackers una forma de acceder a tus aplicaciones. 
- Optimiza tu API: la optimización del rendimiento en torno a las solicitudes grandes, el uso de recursos más sustancial y otras situaciones en las que se encuentra tu API puede mejorar la experiencia del usuario y potencialmente evitar interrupciones. 
- Configurar las cuotas y la limitación de solicitudes: los grandes aumentos de tráfico, ya sean ataques de denegación de servicio (DoS) o solo cambios de uso naturales, pueden interrumpir las API. Las cuotas de tráfico y las estrategias de limitación de solicitudes pueden prevenir los grandes picos que contribuyen a los apagones.

```javascript
Mal ejemplo: ❌
  GET /users  // Anyone can access this!
✔ Problema : La falta de autenticación provoca fuga de información .

✔ Buen ejemplo: ✅ (utilizar autenticación segura (OAuth2, JWT))
  Authorization: Bearer <token>
```

**Nota**: Se recomienda que las APIs tengan un método de autenticación para evitar ataques informáticos.

### 📌 6. Versionar las APIs

* Las APIs cambian con el tiempo.
* Se debe dar tiempo a los clientes que se actualicen a la nueva versión
* No se recomienda hacer cambios a las APIs y dejarlos así porque pueden causar problemas de compatibilidad.
* Utilice el control de versiones de URL ( /v1/).
* Alternativamente, utilice encabezados ( Accept: application/vnd.api.v2+json).
* Desaprobar gradualmente las versiones antiguas .

```javascript
Si una API cambia, romper los clientes antiguos es un riesgo enorme .

Mal ejemplo: ❌
  GET /users
✔ Problema : agregar cambios directamente puede romper las integraciones existentes.

Buen ejemplo: ✅ (Implementar control de versiones de API)
  GET /v1/users
  GET /v2/users
```

### 📌 7. Permitir solicitudes ilimitadas (sin límite de velocidad) 🚀
- Error: No hay límite de velocidad de API
- Las API sin límites de velocidad son vulnerables a abusos y ataques DDoS .
- Establecer límites por usuario o IP .
- Utilice puertas de enlace API (AWS API Gateway, Kong) .

```javascript
Mal ejemplo: ❌
  GET /users (solicitudes ilimitadas permitidas)
✔ Problema : los atacantes pueden enviar spam con solicitudes , lo que provoca fallas del servidor .

✅ Solución: Implementar limitación de velocidad de API
✔ Buen ejemplo (Node.js Express): ✅

  const rateLimit = require('express-rate-limit');

  const limiter = rateLimit({ 
      windowMs: 15 * 60 * 1000, // 15 minutos 
      máx.: 100, // Limita cada IP a 100 solicitudes 
  });
  app.use('/api/', limitador);

```

### 📌 8. Devolver demasiados datos sin paginación 📄

- Obtener todos los datos a la vez
- Las API que devuelven grandes cantidades de datos ralentizan las respuestas.
- Utilice paginación basada en límites y desplazamientos .
- Implemente la paginación basada en cursor para conjuntos de datos grandes.

```javascript
Mal ejemplo: ❌
  GET /users  // Returns all users (100,000+ records)
  Problema : Carga útil enorme → Tiempos de respuesta más lentos .

Buen ejemplo: ✅ (Implementar paginación)
  GET /users?page=2&limit=10
```

### 📌 9.  Ignorar la documentación de la API 📖
- No hay documentación de API - Error
- Las API sin documentación frustran a los desarrolladores .
- Utilice Swagger/OpenAPI para obtener documentación de API.
- Generar automáticamente documentación API.

```javascript
Mal ejemplo: ❌
  GET /users
✔ Problema : No hay detalles sobre el formato y los parámetros de la solicitud .

Buen ejemplo: ✅ (Utilizar OpenAPI (Swagger))
  paths:
    /users:
      get:
        summary: Get all users
        parameters:
          - name: page
            in: query
            required: false
            schema:
              type: integer
        responses:
          "200":
            description: Successful response
```

### 📌 10. Pruebas y monitoreo
    
Probar todo antes de lanzar. 
    
- Pruebas unitarias
- Pruebas de integración
- Pruebas de seguridad. 
- Pruebas de rendimiento (Estres)


### 📌 11. Usa JSON, pero úsalo bien

```javascript
Mal ejemplo: ❌ -> esto no es formato json.
ya quedo bien

Buen ejemplo: ✅ -> formato json correcto.
{
    "error": true,
    "message": "Invalid ID"
}
```

### 📌 12. Optimización y rendimiento
Una API lenta puede ser frustrante para los usuarios. Aquí tienes algunos consejos para mejorar su rendimiento:

- Caché: Implementa caché donde sea posible para reducir el tiempo de respuesta.
- Compresión: Utiliza la compresión (como gzip) para disminuir el tamaño de los datos que viajan entre el cliente y el servidor.
- Paginación: Si tu API devuelve grandes cantidades de datos (como una lista de usuarios), usa paginación para no sobrecargar el sistema.
- Rate Limiting: Limita el número de solicitudes que un usuario puede hacer por minuto para evitar abusos y mejorar la estabilidad de tu API.


### NOTAS    
* JSON es el estándar por defecto en REST.
* Se debe retornar un JSON válido.
* No existe un estándar oficial de REST.

# Reglas de una arquitectura REST

1. Interfaz uniforme
* La interfaz de basa en recursos (por ejemplo el recurso Empleado (Id, Nombre, Apellido, Puesto, Sueldo)
* El servidor retornado los datos en algún formato (html, json, xml...)
* El recurso solicitado se suele enviar un parámetro o un objeto en el cuerpo.
* Usar las características del protocolo http para mejorar la semántica
    * HTTP Verbs => /product/123
    * HTTP Status Codes => 200, 401
    * HTTP Authentication => Auth Basic

2. Peticiones sin estado
* Es un protocolo sin estado, debido a que no se guarda la información en el servidor. 
* Toda la información será enviada por el cliente en cada mensaje HTTP, consiguiendo un ahorro en variables de sesión y almacenamiento interno del servidor.
* http es un protocolo sin estado -> mayor rendimiento

3. Cacheable
* En la web los clientes pueden cachear las respuestas del servidor.
* Las respuestas se deben marcar de forma implícita o explícita como cacheables o no.
* En futuras peticiones, el cliente sabrá si puede reutilizar o no los datos que ya ha obtenido.
* Si ahorramos peticiones, mejoraremos la escabilidad de la aplicación y el rendimiento en cliente (evitamos principalmente la latencia).

4. Separación de cliente y servidor
* El cliente y servidor están separados, su unión es mediante el protocolo HTTP (interfaz uniforme).
* Los desarrollos en frontend y backend se hacen por separado, teniendo en cuenta la API.
* Mientras la interfaz no cambie, podremos cambiar el cliente o el servidor sin problemas.

5. Sistema de Capas
* El cliente puede estar conectado mediante la interfaz al servidor o a un intermediario, para el es irrelevante y desconocido.
* Al cliente solo le preocupa que la API REST funcione como debe: no importa el COMO sino el QUE
* El uso de capas o servidores intermedios puede servir para aumentar la escalabilidad (sistemas de balanceo de carga, cachés) o para implementar políticas de seguridad

6. CORS
* En el 2008 se publicó la primera versión de la especificación XMLHttpRequest Level 2.
* CORS es el acrónimo de Cross-origin resource sharing.
* Debemos tener presente, que los navegadores antiguos no soportan CORS.
* Esta nueva especificación, añade funcionalidades nuevas a las peticiones AJAX como las peticiones entre dominios (cross-site), eventos de progreso y envio de datos binarios.
* CORS requiere configuraciones en el servidor o a nivel de desarrollo.