---
title: Kong Admin API v3.5.0
language_tabs: []
toc_footers:
  - <a href="https://docs.konghq.com">Kong Gateway Admin API (OSS)</a>
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="kong-admin-api">Kong Admin API v3.5.0</h1>

> Scroll down for example requests and responses.

OpenAPI 3.0 spec for Kong Gateway's open source Admin API.

You can know more about Kong Gateway at [docs.konghq.com](https://docs.konghq.com)
.Give Kong a star at [Kong/kong](https://github.com/kong/kong) repository.

Base URLs:

* <a href="{protocol}://{hostname}:{port}{path}">{protocol}://{hostname}:{port}{path}</a>

    * **hostname** - Hostname for Kong's Admin API Default: localhost

    * **path** - Base path for Kong's Admin API Default: /

    * **port** - Port for Kong's Admin API Default: 8001

    * **protocol** - Protocol for requests to Kong's Admin API Default: http

        * http

        * https

Email: <a href="mailto:docs@konghq.com">Kong Inc</a> Web: <a href="https://konghq.com">Kong Inc</a> 
License: <a href="https://www.apache.org/licenses/LICENSE-2.0.html">Apache 2.0</a>

<h1 id="kong-admin-api-services">Services</h1>

Service entities are abstractions of your microservice interfaces or formal APIs. For example, a service could be a data transformation microservice or a billing API.
<br><br>
The main attribute of a service is the destination URL for proxying traffic. This URL can be set as a single string or by specifying its protocol, host, port and path individually. 
<br><br>
Services are associated to routes, and a single service can have many routes associated with it. Routes are entrypoints in Kong Gateway which define rules to match client requests. Once a route is matched, Kong Gateway proxies the request to its associated service. See the [Proxy Reference](https://docs.konghq.com/gateway/latest/how-kong-works/routing-traffic/) for a detailed explanation of how Kong proxies traffic.
<br><br>
Services can be both [tagged and filtered by tags](https://docs.konghq.com/gateway/latest/admin-api/#tags).

## list-service

<a id="opIdlist-service"></a>

`GET /services`

*List all services*

List all services

<h3 id="list-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "host": "example.internal",
      "id": "49fd316e-c457-481c-9fc7-8079153e4f3c",
      "name": "example-service",
      "path": "/",
      "port": 80,
      "protocol": "http"
    }
  ],
  "offset": "string"
}
```

<h3 id="list-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing services|Inline|

<h3 id="list-service-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Service](#schemaservice)]|false|none|[service entities are abstractions of upstream services. The main attribute of a service is its URL which can be set as a single string or by specifying the `protocol`, `host`, `port` and `path` individually.]|
|»» Service|[Service](#schemaservice)|false|none|service entities are abstractions of upstream services. The main attribute of a service is its URL which can be set as a single string or by specifying the `protocol`, `host`, `port` and `path` individually.|
|»»» ca_certificates|[string]|false|none|Array of `CA Certificate` object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to `null` when Nginx default is respected. If default CA list in Nginx are not specified and TLS verification is enabled, then handshake with upstream server will always fail (because no CA are trusted).|
|»»» client_certificate|object|false|none|Certificate to be used as client certificate while TLS handshaking to the upstream server.|
|»»»» id|string|false|none|none|
|»»» connect_timeout|integer|false|none|The timeout in milliseconds for establishing a connection to the upstream server.|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» enabled|boolean|false|none|Whether the service is active. If set to `false`, the proxy behavior will be as if any routes attached to it do not exist (404).|
|»»» host|string|false|none|The host of the upstream server. Note that the host value is case sensitive.|
|»»» id|string|false|none|none|
|»»» name|string|false|none|The service name.|
|»»» path|string|false|none|The path to be used in requests to the upstream server.|
|»»» port|integer|false|none|The upstream server port.|
|»»» protocol|string|false|none|The protocol used to communicate with the upstream.|
|»»» read_timeout|integer|false|none|The timeout in milliseconds between two successive read operations for transmitting a request to the upstream server.|
|»»» retries|integer|false|none|The number of retries to execute upon failure to proxy.|
|»»» tags|[string]|false|none|An optional set of strings associated with the service for grouping and filtering.|
|»»» tls_verify|boolean¦null|false|none|Whether to enable verification of upstream server TLS certificate. If set to `null`, then the Nginx default is respected.|
|»»» tls_verify_depth|integer|false|none|Maximum depth of chain while verifying Upstream server's TLS certificate. If set to `null`, then the Nginx default is respected.'|
|»»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|»»» url|string|false|none|Helper field to set `protocol`, `host`, `port` and `path` using a URL. This field is write-only and is not returned in responses.|
|»»» write_timeout|integer|false|none|The timeout in milliseconds between two successive write operations for transmitting a request to the upstream server.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-service

<a id="opIdcreate-service"></a>

`POST /services`

*Create a new service*

Create a new service

> Body parameter

```json
{
  "name": "my-service",
  "retries": 5,
  "protocol": "http",
  "host": "example.com",
  "port": 80,
  "path": "/some_api",
  "connect_timeout": 6000,
  "write_timeout": 6000,
  "read_timeout": 6000,
  "tags": [
    "user-level"
  ],
  "client_certificate": {
    "id": "4e3ad2e4-0bc4-4638-8e34-c84a417ba39b"
  },
  "tls_verify": true,
  "tls_verify_depth": null,
  "ca_certificates": [
    "4e3ad2e4-0bc4-4638-8e34-c84a417ba39b"
  ],
  "enabled": true
}
```

<h3 id="create-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|false|The service name.|
|» retries|body|integer|false|The number of retries to execute upon failure to proxy. Default:`5`.|
|» protocol|body|string|true|The protocol used to communicate with the upstream. Accepted values are: "`grpc`", "`grpcs`", "`http`", "`https`", "`tcp`", "`tls`", "`tls_passthrough`", "`udp`", "`ws`"|
|» host|body|string|true|The host of the upstream server. Note that the host value is case sensitive.|
|» port|body|integer|true|The upstream server port. Default: `80`.|
|» path|body|string|false|The path to be used in requests to the upstream server.|
|» connect_timeout|body|integer|false|The timeout in milliseconds for establishing a connection to the upstream server.|
|» write_timeout|body|integer|false|The timeout in milliseconds between two successive write operations for transmitting a request to the upstream server. Default: `60000`.|
|» read_timeout|body|integer|false|The timeout in milliseconds between two successive read operations for transmitting a request to the upstream server. Default: `60000`.|
|» tags|body|[string]|false|An optional set of strings associated with the service for grouping and filtering.|
|» client_certificate|body|object|false|Certificate to be used as client certificate while TLS handshaking to the upstream server. With form-encoded, the notation is `client_certificate.id=<client_certificate id>`. With JSON, use `"client_certificate":{"id":"<client_certificate id>"}`.|
|»» id|body|string|false|none|
|» tls_verify|body|boolean¦null|false|Whether to enable verification of upstream server TLS certificate. If set to null, then the Nginx default is respected.|
|» tls_verify_depth|body|string¦null|false|Maximum depth of chain while verifying Upstream server's TLS certificate. If set to null, then the Nginx default is respected. Default: null.|
|» ca_certificates|body|[string]|false|Array of CA Certificate object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to null when Nginx default is respected. |
|» enabled|body|boolean|true|Whether the service is active. If set to `false`, the proxy behavior will be as if any routes attached to it do not exist (404). Default: `true`.|

#### Detailed descriptions

**» protocol**: The protocol used to communicate with the upstream. Accepted values are: "`grpc`", "`grpcs`", "`http`", "`https`", "`tcp`", "`tls`", "`tls_passthrough`", "`udp`", "`ws`"
, "`wss`"
. Default: "`http`".

**» ca_certificates**: Array of CA Certificate object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to null when Nginx default is respected. 
With form-encoded, the notation is `ca_certificates[]=4e3ad2e4-0bc4-4638-8e34-c84a417ba39b&ca_certificates[]=51e77dc2-8f3e-4afa-9d0e-0e3bbbcfd515`. With JSON, use an Array.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocol|grpc|
|» protocol|grpcs|
|» protocol|http|
|» protocol|https|
|» protocol|tcp|
|» protocol|tls |
|» protocol|tls_passthrough|
|» protocol|udp|
|» protocol|ws|
|» protocol|wss|

> Example responses

> 201 Response

```json
{
  "host": "example.internal",
  "id": "49fd316e-c457-481c-9fc7-8079153e4f3c",
  "name": "example-service",
  "path": "/",
  "port": 80,
  "protocol": "http"
}
```

<h3 id="create-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created service|[Service](#schemaservice)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid service|Inline|

<h3 id="create-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-service

<a id="opIddelete-service"></a>

`DELETE /services/{service_id_or_name}`

*Delete a service*

Delete a service

<h3 id="delete-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID or name of the service to delete|

<h3 id="delete-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted service or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-service

<a id="opIdget-service"></a>

`GET /services/{service_id_or_name}`

*Fetch a service*

Get a service using ID or name.

<h3 id="get-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|

> Example responses

> 200 Response

```json
{
  "host": "example.internal",
  "id": "49fd316e-c457-481c-9fc7-8079153e4f3c",
  "name": "example-service",
  "path": "/",
  "port": 80,
  "protocol": "http"
}
```

<h3 id="get-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched service|[Service](#schemaservice)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-service

<a id="opIdupdate-service"></a>

`PATCH /services/{service_id_or_name}`

*Update a service*

Update a service

> Body parameter

```json
{
  "name": "my-service",
  "retries": 5,
  "protocol": "http",
  "host": "example.com",
  "port": 80,
  "path": "/some_api",
  "connect_timeout": 6000,
  "write_timeout": 6000,
  "read_timeout": 6000,
  "tags": [
    "user-level"
  ],
  "client_certificate": {
    "id": "4e3ad2e4-0bc4-4638-8e34-c84a417ba39b"
  },
  "tls_verify": true,
  "tls_verify_depth": null,
  "ca_certificates": [
    "4e3ad2e4-0bc4-4638-8e34-c84a417ba39b"
  ],
  "enabled": true
}
```

<h3 id="update-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID or name of the service to update|
|body|body|object|false|none|
|» name|body|string|false|The service name.|
|» retries|body|integer|false|The number of retries to execute upon failure to proxy. Default:`5`.|
|» protocol|body|string|true|The protocol used to communicate with the upstream. Accepted values are: "`grpc`", "`grpcs`", "`http`", "`https`", "`tcp`", "`tls`", "`tls_passthrough`", "`udp`", "`ws`"|
|» host|body|string|true|The host of the upstream server. Note that the host value is case sensitive.|
|» port|body|integer|true|The upstream server port. Default: `80`.|
|» path|body|string|false|The path to be used in requests to the upstream server.|
|» connect_timeout|body|integer|false|The timeout in milliseconds for establishing a connection to the upstream server.|
|» write_timeout|body|integer|false|The timeout in milliseconds between two successive write operations for transmitting a request to the upstream server. Default: `60000`.|
|» read_timeout|body|integer|false|The timeout in milliseconds between two successive read operations for transmitting a request to the upstream server. Default: `60000`.|
|» tags|body|[string]|false|An optional set of strings associated with the service for grouping and filtering.|
|» client_certificate|body|object|false|Certificate to be used as client certificate while TLS handshaking to the upstream server. With form-encoded, the notation is `client_certificate.id=<client_certificate id>`. With JSON, use `"client_certificate":{"id":"<client_certificate id>"}`.|
|»» id|body|string|false|none|
|» tls_verify|body|boolean¦null|false|Whether to enable verification of upstream server TLS certificate. If set to null, then the Nginx default is respected.|
|» tls_verify_depth|body|string¦null|false|Maximum depth of chain while verifying Upstream server's TLS certificate. If set to null, then the Nginx default is respected. Default: null.|
|» ca_certificates|body|[string]|false|Array of CA Certificate object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to null when Nginx default is respected. |
|» enabled|body|boolean|true|Whether the service is active. If set to `false`, the proxy behavior will be as if any routes attached to it do not exist (404). Default: `true`.|

#### Detailed descriptions

**» protocol**: The protocol used to communicate with the upstream. Accepted values are: "`grpc`", "`grpcs`", "`http`", "`https`", "`tcp`", "`tls`", "`tls_passthrough`", "`udp`", "`ws`"
, "`wss`"
. Default: "`http`".

**» ca_certificates**: Array of CA Certificate object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to null when Nginx default is respected. 
With form-encoded, the notation is `ca_certificates[]=4e3ad2e4-0bc4-4638-8e34-c84a417ba39b&ca_certificates[]=51e77dc2-8f3e-4afa-9d0e-0e3bbbcfd515`. With JSON, use an Array.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocol|grpc|
|» protocol|grpcs|
|» protocol|http|
|» protocol|https|
|» protocol|tcp|
|» protocol|tls |
|» protocol|tls_passthrough|
|» protocol|udp|
|» protocol|ws|
|» protocol|wss|

> Example responses

> 200 Response

```json
{
  "host": "example.internal",
  "id": "49fd316e-c457-481c-9fc7-8079153e4f3c",
  "name": "example-service",
  "path": "/",
  "port": 80,
  "protocol": "http"
}
```

<h3 id="update-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated service|[Service](#schemaservice)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid service|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-service

<a id="opIdupsert-service"></a>

`PUT /services/{service_id_or_name}`

*Upsert a service*

Create or Update service using ID or name.

> Body parameter

```json
{
  "name": "my-service",
  "retries": 5,
  "protocol": "http",
  "host": "example.com",
  "port": 80,
  "path": "/some_api",
  "connect_timeout": 6000,
  "write_timeout": 6000,
  "read_timeout": 6000,
  "tags": [
    "user-level"
  ],
  "client_certificate": {
    "id": "4e3ad2e4-0bc4-4638-8e34-c84a417ba39b"
  },
  "tls_verify": true,
  "tls_verify_depth": null,
  "ca_certificates": [
    "4e3ad2e4-0bc4-4638-8e34-c84a417ba39b"
  ],
  "enabled": true
}
```

<h3 id="upsert-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|Name or ID of the service to lookup|
|body|body|object|false|none|
|» name|body|string|false|The service name.|
|» retries|body|integer|false|The number of retries to execute upon failure to proxy. Default:`5`.|
|» protocol|body|string|true|The protocol used to communicate with the upstream. Accepted values are: "`grpc`", "`grpcs`", "`http`", "`https`", "`tcp`", "`tls`", "`tls_passthrough`", "`udp`", "`ws`"|
|» host|body|string|true|The host of the upstream server. Note that the host value is case sensitive.|
|» port|body|integer|true|The upstream server port. Default: `80`.|
|» path|body|string|false|The path to be used in requests to the upstream server.|
|» connect_timeout|body|integer|false|The timeout in milliseconds for establishing a connection to the upstream server.|
|» write_timeout|body|integer|false|The timeout in milliseconds between two successive write operations for transmitting a request to the upstream server. Default: `60000`.|
|» read_timeout|body|integer|false|The timeout in milliseconds between two successive read operations for transmitting a request to the upstream server. Default: `60000`.|
|» tags|body|[string]|false|An optional set of strings associated with the service for grouping and filtering.|
|» client_certificate|body|object|false|Certificate to be used as client certificate while TLS handshaking to the upstream server. With form-encoded, the notation is `client_certificate.id=<client_certificate id>`. With JSON, use `"client_certificate":{"id":"<client_certificate id>"}`.|
|»» id|body|string|false|none|
|» tls_verify|body|boolean¦null|false|Whether to enable verification of upstream server TLS certificate. If set to null, then the Nginx default is respected.|
|» tls_verify_depth|body|string¦null|false|Maximum depth of chain while verifying Upstream server's TLS certificate. If set to null, then the Nginx default is respected. Default: null.|
|» ca_certificates|body|[string]|false|Array of CA Certificate object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to null when Nginx default is respected. |
|» enabled|body|boolean|true|Whether the service is active. If set to `false`, the proxy behavior will be as if any routes attached to it do not exist (404). Default: `true`.|

#### Detailed descriptions

**» protocol**: The protocol used to communicate with the upstream. Accepted values are: "`grpc`", "`grpcs`", "`http`", "`https`", "`tcp`", "`tls`", "`tls_passthrough`", "`udp`", "`ws`"
, "`wss`"
. Default: "`http`".

**» ca_certificates**: Array of CA Certificate object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to null when Nginx default is respected. 
With form-encoded, the notation is `ca_certificates[]=4e3ad2e4-0bc4-4638-8e34-c84a417ba39b&ca_certificates[]=51e77dc2-8f3e-4afa-9d0e-0e3bbbcfd515`. With JSON, use an Array.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocol|grpc|
|» protocol|grpcs|
|» protocol|http|
|» protocol|https|
|» protocol|tcp|
|» protocol|tls |
|» protocol|tls_passthrough|
|» protocol|udp|
|» protocol|ws|
|» protocol|wss|

> Example responses

> 200 Response

```json
{
  "host": "example.internal",
  "id": "49fd316e-c457-481c-9fc7-8079153e4f3c",
  "name": "example-service",
  "path": "/",
  "port": 80,
  "protocol": "http"
}
```

<h3 id="upsert-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted service|[Service](#schemaservice)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid service|Inline|

<h3 id="upsert-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-routes">Routes</h1>

Route entities define rules to match client requests. Each route is associated with a service, and a service may have multiple routes associated to it. Every request matching a given route will be proxied to the associated service. You need at least one matching rule that applies to the protocol being matched by the route.
<br><br>
The combination of routes and services, and the separation of concerns between them, offers a powerful routing mechanism with which it is possible to define fine-grained entrypoints in Kong Gateway leading to different upstream services of your infrastructure.
<br><br>
Depending on the protocol, one of the following attributes must be set:
<br>
* `http`: At least one of `methods`, `hosts`, `headers`, or `paths`
* `https`:  At least one of `methods`, `hosts`, `headers`, `paths`, or `snis`
* `tcp`: At least one of `sources` or `destinations`
* `tls`: at least one of `sources`, `destinations`, or `snis`
* `tls_passthrough`: set `snis`
* `grpc`: At least one of `hosts`, `headers`, or `paths`
* `grpcs`: At least one of `hosts`, `headers`, `paths`, or `snis`
<br>

A route can't have both `tls` and `tls_passthrough` protocols at same time.
<br><br>

Learn more about the router: 
* [Configure routes using expressions](https://docs.konghq.com/gateway/3.4.x/key-concepts/routes/expressions)
* [Router Expressions language reference](https://docs.konghq.com/gateway/3.4.x/reference/router-expressions-language/)

## list-route

<a id="opIdlist-route"></a>

`GET /routes`

*List all routes*

List all routes

route entities define rules to match client requests. Each route is associated with a service, and a service may have multiple routes associated to it. Every request matching a given route will be proxied to its associated service.

> Note: Path handling algorithms v1 was deprecated in Kong 3.0. From Kong 3.0, when router_flavor is set to expressions, route.path_handling will be unconfigurable and the path handling behavior will be "v0"; when router_flavor is set to traditional_compatible, the path handling behavior will be "v0" regardless of the value of route.path_handling. Only router_flavor = traditional will support path_handling "v1" behavior.

<h3 id="list-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "hosts": [
        "foo.example.com",
        "bar.example.com"
      ],
      "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
      "name": "example-route",
      "paths": [
        "/v1",
        "/v2"
      ],
      "service": {
        "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
      }
    }
  ],
  "offset": "string"
}
```

<h3 id="list-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing routes|Inline|

<h3 id="list-route-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Route](#schemaroute)]|false|none|[Route entities define rules to match client requests. Every request matching a given route will be proxied to its associated service.]|
|»» Route|[Route](#schemaroute)|false|none|Route entities define rules to match client requests. Every request matching a given route will be proxied to its associated service.|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» destinations|[object]|false|none|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»»»» default|any|false|none|none|
|»»» headers|object|false|none|One or more lists of values indexed by header name that will cause this route to match if present in the request. The `Host` header cannot be used with this hosts should be specified using the `hosts` attribute. When `headers` contains only one value and that value starts with the special prefix `~*`, the value is interpreted as a regular expression.|
|»»» hosts|[string]|false|none|A list of domain names that match this route. Note that the hosts value is case sensitive.|
|»»» https_redirect_status_code|integer|false|none|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`. `Location` header is injected by Kong if the field is set to 301, 302, 307 or 308. This config applies only if the route is configured to only accept the `https` protocol.|
|»»» id|string|false|none|none|
|»»» methods|[string]|false|none|A list of HTTP methods that match this route.|
|»»» name|string|false|none|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|»»» path_handling|string|false|none|Controls how the service path, route path and requested path are combined when sending a request to the upstream. See above for a detailed description of each behavior.|
|»»» paths|[string]|false|none|A list of paths that match this route.|
|»»» preserve_host|boolean|false|none|When matching a route via one of the `hosts` domain names, use the request `Host` header in the upstream request headers. If set to `false`, the upstream `Host` header will be that of the services `host`.|
|»»» protocols|[string]|false|none|An array of the protocols this route should allow. See the [route Object](#route-object) section for a list of accepted protocols. When set to only `"https"`, HTTP requests are answered with an upgrade error. When set to only `"http"`, HTTPS requests are answered with an error.|
|»»» regex_priority|integer|false|none|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same `regex_priority`, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|»»» request_buffering|boolean|false|none|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding.|
|»»» response_buffering|boolean|false|none|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding.|
|»»» service|object|false|none|The service this route is associated to. This is where the route proxies traffic to.|
|»»»» id|string|false|none|none|
|»»» snis|[string]|false|none|A list of SNIs that match this route when using stream routing.|
|»»» sources|[object]|false|none|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»»»» default|any|false|none|none|
|»»» strip_path|boolean|false|none|When matching a route via one of the `paths`, strip the matching prefix from the upstream request URL.|
|»»» tags|[string]|false|none|An optional set of strings associated with the route for grouping and filtering.|
|»»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-route

<a id="opIdcreate-route"></a>

`POST /routes`

*Create a new route*

Create a new route

> Body parameter

```json
{
  "name": "my-route",
  "protocols": [
    "http",
    "https"
  ],
  "methods": [
    "GET",
    "POST"
  ],
  "hosts": [
    "example.com",
    "foo.test"
  ],
  "paths": [
    "/foo",
    "/bar"
  ],
  "headers": {
    "x-my-header": [
      "foo",
      "bar"
    ],
    "x-another-header": [
      "bla"
    ]
  },
  "https_redirect_status_code": 426,
  "regex_priority": 0,
  "strip_path": true,
  "path_handling": "v0",
  "preserve_host": false,
  "request_buffering": true,
  "response_buffering": true,
  "tags": [
    "user-level",
    "low-priority"
  ],
  "service": {
    "id": "af8330d3-dbdc-48bd-b1be-55b98608834b"
  }
}
```

<h3 id="create-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Route request body|
|» name|body|string|false|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|» protocols|body|[string]|true|An array of the protocols this route should allow|
|» methods|body|[string]|false|A list of HTTP methods that match this route.|
|» hosts|body|[string]|false|A list of domain names that match this route. Note that the hosts value is case sensitive. With form-encoded, the notation is `hosts[]=example.com&hosts[]=foo.test`. With JSON, use an Array.|
|» paths|body|[string]|false|A list of paths that match this route. With form-encoded, the notation is `paths[]=/foo&paths[]=/bar`. With JSON, use an array. The path can be a regular expression, or a plain text pattern.|
|» headers|body|object|false|One or more lists of values indexed by header name that will cause this route to match if present in the request. The Host header cannot be used with this hosts should be specified using the `hosts` attribute. When headers contains only one value and that value starts with the special prefix` ~*`, the value is interpreted as a regular expression.|
|»» x-my-header|body|[string]|false|none|
|»» x-another-header|body|[string]|false|none|
|» https_redirect_status_code|body|integer|true|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`|
|» regex_priority|body|integer|false|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same regex_priority, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|» strip_path|body|boolean|false|When matching a route via one of the paths, strip the matching prefix from the upstream request URL.|
|» path_handling|body|string|false|Controls how the service path, route path and requested path are combined when sending a request to the upstream. Accepted values are "`v0`", "`v1`".|
|» preserve_host|body|boolean|true|When matching a route via one of the `hosts` domain names, use the request `host` header in the upstream request headers. If set to `false`, the upstream Host header will be that of the service's host.|
|» request_buffering|body|boolean|true|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding. Default: true.|
|» response_buffering|body|boolean|true|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding. Default: `true`.|
|» snis|body|[string]|false|A list of SNIs that match this route when using stream routing.|
|» sources|body|[object]|false|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» destinations|body|[object]|false|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the route for grouping and filtering.|
|» service|body|object|false|The service this route is associated to. This is where the route proxies traffic to. With form-encoded, the notation is service.id=<service id> or service.name=<service name>. With JSON, use `"service":{"id":"<service id>"}` or `"service":{"name":"<service name>"}`.|
|»» id|body|string|false|none|

#### Detailed descriptions

**» https_redirect_status_code**: The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`
Location header is injected by Kong if the field is set to `301`, `302`, `307` or `308`. Note: This config applies only if the route is configured to only accept the https protocol. Accepted values are: `426`, `301`, `302`, `307`, `308`. Default: `426`.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» https_redirect_status_code|426|
|» https_redirect_status_code|301|
|» https_redirect_status_code|302|
|» https_redirect_status_code|307|
|» https_redirect_status_code|308|
|» path_handling|v1|
|» path_handling|v0|

> Example responses

> 201 Response

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

<h3 id="create-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created route|[Route](#schemaroute)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid route|Inline|

<h3 id="create-route-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-route

<a id="opIddelete-route"></a>

`DELETE /routes/{route_id_or_name}`

*Delete a route*

Delete a route

> Note: This API is not available in DB-less mode.

<h3 id="delete-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

<h3 id="delete-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted route or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-route

<a id="opIdget-route"></a>

`GET /routes/{route_id_or_name}`

*Fetch a route*

Get a route using ID or name.

<h3 id="get-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

> Example responses

> 200 Response

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

<h3 id="get-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched route|[Route](#schemaroute)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-route

<a id="opIdupdate-route"></a>

`PATCH /routes/{route_id_or_name}`

*Update a route*

Update a route

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "name": "my-route",
  "protocols": [
    "http",
    "https"
  ],
  "methods": [
    "GET",
    "POST"
  ],
  "hosts": [
    "example.com",
    "foo.test"
  ],
  "paths": [
    "/foo",
    "/bar"
  ],
  "headers": {
    "x-my-header": [
      "foo",
      "bar"
    ],
    "x-another-header": [
      "bla"
    ]
  },
  "https_redirect_status_code": 426,
  "regex_priority": 0,
  "strip_path": true,
  "path_handling": "v0",
  "preserve_host": false,
  "request_buffering": true,
  "response_buffering": true,
  "tags": [
    "user-level",
    "low-priority"
  ],
  "service": {
    "id": "af8330d3-dbdc-48bd-b1be-55b98608834b"
  }
}
```

<h3 id="update-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Route request body|
|» name|body|string|false|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|» protocols|body|[string]|true|An array of the protocols this route should allow|
|» methods|body|[string]|false|A list of HTTP methods that match this route.|
|» hosts|body|[string]|false|A list of domain names that match this route. Note that the hosts value is case sensitive. With form-encoded, the notation is `hosts[]=example.com&hosts[]=foo.test`. With JSON, use an Array.|
|» paths|body|[string]|false|A list of paths that match this route. With form-encoded, the notation is `paths[]=/foo&paths[]=/bar`. With JSON, use an array. The path can be a regular expression, or a plain text pattern.|
|» headers|body|object|false|One or more lists of values indexed by header name that will cause this route to match if present in the request. The Host header cannot be used with this hosts should be specified using the `hosts` attribute. When headers contains only one value and that value starts with the special prefix` ~*`, the value is interpreted as a regular expression.|
|»» x-my-header|body|[string]|false|none|
|»» x-another-header|body|[string]|false|none|
|» https_redirect_status_code|body|integer|true|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`|
|» regex_priority|body|integer|false|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same regex_priority, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|» strip_path|body|boolean|false|When matching a route via one of the paths, strip the matching prefix from the upstream request URL.|
|» path_handling|body|string|false|Controls how the service path, route path and requested path are combined when sending a request to the upstream. Accepted values are "`v0`", "`v1`".|
|» preserve_host|body|boolean|true|When matching a route via one of the `hosts` domain names, use the request `host` header in the upstream request headers. If set to `false`, the upstream Host header will be that of the service's host.|
|» request_buffering|body|boolean|true|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding. Default: true.|
|» response_buffering|body|boolean|true|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding. Default: `true`.|
|» snis|body|[string]|false|A list of SNIs that match this route when using stream routing.|
|» sources|body|[object]|false|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» destinations|body|[object]|false|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the route for grouping and filtering.|
|» service|body|object|false|The service this route is associated to. This is where the route proxies traffic to. With form-encoded, the notation is service.id=<service id> or service.name=<service name>. With JSON, use `"service":{"id":"<service id>"}` or `"service":{"name":"<service name>"}`.|
|»» id|body|string|false|none|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

#### Detailed descriptions

**» https_redirect_status_code**: The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`
Location header is injected by Kong if the field is set to `301`, `302`, `307` or `308`. Note: This config applies only if the route is configured to only accept the https protocol. Accepted values are: `426`, `301`, `302`, `307`, `308`. Default: `426`.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» https_redirect_status_code|426|
|» https_redirect_status_code|301|
|» https_redirect_status_code|302|
|» https_redirect_status_code|307|
|» https_redirect_status_code|308|
|» path_handling|v1|
|» path_handling|v0|

> Example responses

> 200 Response

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

<h3 id="update-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated route|[Route](#schemaroute)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid route|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-route-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-route

<a id="opIdupsert-route"></a>

`PUT /routes/{route_id_or_name}`

*Update a route*

Create or Update route using ID or name.

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "name": "my-route",
  "protocols": [
    "http",
    "https"
  ],
  "methods": [
    "GET",
    "POST"
  ],
  "hosts": [
    "example.com",
    "foo.test"
  ],
  "paths": [
    "/foo",
    "/bar"
  ],
  "headers": {
    "x-my-header": [
      "foo",
      "bar"
    ],
    "x-another-header": [
      "bla"
    ]
  },
  "https_redirect_status_code": 426,
  "regex_priority": 0,
  "strip_path": true,
  "path_handling": "v0",
  "preserve_host": false,
  "request_buffering": true,
  "response_buffering": true,
  "tags": [
    "user-level",
    "low-priority"
  ],
  "service": {
    "id": "af8330d3-dbdc-48bd-b1be-55b98608834b"
  }
}
```

<h3 id="upsert-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Route request body|
|» name|body|string|false|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|» protocols|body|[string]|true|An array of the protocols this route should allow|
|» methods|body|[string]|false|A list of HTTP methods that match this route.|
|» hosts|body|[string]|false|A list of domain names that match this route. Note that the hosts value is case sensitive. With form-encoded, the notation is `hosts[]=example.com&hosts[]=foo.test`. With JSON, use an Array.|
|» paths|body|[string]|false|A list of paths that match this route. With form-encoded, the notation is `paths[]=/foo&paths[]=/bar`. With JSON, use an array. The path can be a regular expression, or a plain text pattern.|
|» headers|body|object|false|One or more lists of values indexed by header name that will cause this route to match if present in the request. The Host header cannot be used with this hosts should be specified using the `hosts` attribute. When headers contains only one value and that value starts with the special prefix` ~*`, the value is interpreted as a regular expression.|
|»» x-my-header|body|[string]|false|none|
|»» x-another-header|body|[string]|false|none|
|» https_redirect_status_code|body|integer|true|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`|
|» regex_priority|body|integer|false|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same regex_priority, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|» strip_path|body|boolean|false|When matching a route via one of the paths, strip the matching prefix from the upstream request URL.|
|» path_handling|body|string|false|Controls how the service path, route path and requested path are combined when sending a request to the upstream. Accepted values are "`v0`", "`v1`".|
|» preserve_host|body|boolean|true|When matching a route via one of the `hosts` domain names, use the request `host` header in the upstream request headers. If set to `false`, the upstream Host header will be that of the service's host.|
|» request_buffering|body|boolean|true|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding. Default: true.|
|» response_buffering|body|boolean|true|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding. Default: `true`.|
|» snis|body|[string]|false|A list of SNIs that match this route when using stream routing.|
|» sources|body|[object]|false|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» destinations|body|[object]|false|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the route for grouping and filtering.|
|» service|body|object|false|The service this route is associated to. This is where the route proxies traffic to. With form-encoded, the notation is service.id=<service id> or service.name=<service name>. With JSON, use `"service":{"id":"<service id>"}` or `"service":{"name":"<service name>"}`.|
|»» id|body|string|false|none|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

#### Detailed descriptions

**» https_redirect_status_code**: The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`
Location header is injected by Kong if the field is set to `301`, `302`, `307` or `308`. Note: This config applies only if the route is configured to only accept the https protocol. Accepted values are: `426`, `301`, `302`, `307`, `308`. Default: `426`.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» https_redirect_status_code|426|
|» https_redirect_status_code|301|
|» https_redirect_status_code|302|
|» https_redirect_status_code|307|
|» https_redirect_status_code|308|
|» path_handling|v1|
|» path_handling|v0|

> Example responses

> 200 Response

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

<h3 id="upsert-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted route|[Route](#schemaroute)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid route|Inline|

<h3 id="upsert-route-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## list-routes-for-service

<a id="opIdlist-routes-for-service"></a>

`GET /services/{service_id_or_name}/routes`

*List all routes associated with a service*

List all routes associated with a service

<h3 id="list-routes-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID or name of the related service|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> A successful response listing routes

```json
{
  "data": [
    {
      "hosts": [
        "foo.example.com",
        "bar.example.com"
      ],
      "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
      "name": "example-route",
      "paths": [
        "/v1",
        "/v2"
      ],
      "service": {
        "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
      }
    }
  ],
  "offset": "string"
}
```

<h3 id="list-routes-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing routes|Inline|

<h3 id="list-routes-for-service-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Route](#schemaroute)]|false|none|[Route entities define rules to match client requests. Every request matching a given route will be proxied to its associated service.]|
|»» Route|[Route](#schemaroute)|false|none|Route entities define rules to match client requests. Every request matching a given route will be proxied to its associated service.|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» destinations|[object]|false|none|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»»»» default|any|false|none|none|
|»»» headers|object|false|none|One or more lists of values indexed by header name that will cause this route to match if present in the request. The `Host` header cannot be used with this hosts should be specified using the `hosts` attribute. When `headers` contains only one value and that value starts with the special prefix `~*`, the value is interpreted as a regular expression.|
|»»» hosts|[string]|false|none|A list of domain names that match this route. Note that the hosts value is case sensitive.|
|»»» https_redirect_status_code|integer|false|none|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`. `Location` header is injected by Kong if the field is set to 301, 302, 307 or 308. This config applies only if the route is configured to only accept the `https` protocol.|
|»»» id|string|false|none|none|
|»»» methods|[string]|false|none|A list of HTTP methods that match this route.|
|»»» name|string|false|none|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|»»» path_handling|string|false|none|Controls how the service path, route path and requested path are combined when sending a request to the upstream. See above for a detailed description of each behavior.|
|»»» paths|[string]|false|none|A list of paths that match this route.|
|»»» preserve_host|boolean|false|none|When matching a route via one of the `hosts` domain names, use the request `Host` header in the upstream request headers. If set to `false`, the upstream `Host` header will be that of the services `host`.|
|»»» protocols|[string]|false|none|An array of the protocols this route should allow. See the [route Object](#route-object) section for a list of accepted protocols. When set to only `"https"`, HTTP requests are answered with an upgrade error. When set to only `"http"`, HTTPS requests are answered with an error.|
|»»» regex_priority|integer|false|none|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same `regex_priority`, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|»»» request_buffering|boolean|false|none|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding.|
|»»» response_buffering|boolean|false|none|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding.|
|»»» service|object|false|none|The service this route is associated to. This is where the route proxies traffic to.|
|»»»» id|string|false|none|none|
|»»» snis|[string]|false|none|A list of SNIs that match this route when using stream routing.|
|»»» sources|[object]|false|none|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»»»» default|any|false|none|none|
|»»» strip_path|boolean|false|none|When matching a route via one of the `paths`, strip the matching prefix from the upstream request URL.|
|»»» tags|[string]|false|none|An optional set of strings associated with the route for grouping and filtering.|
|»»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-route-for-service

<a id="opIdcreate-route-for-service"></a>

`POST /services/{service_id_or_name}/routes`

*Create a new route associated with a service*

Create a new route associated with a service

> Body parameter

```json
{
  "name": "my-route",
  "protocols": [
    "http",
    "https"
  ],
  "methods": [
    "GET",
    "POST"
  ],
  "hosts": [
    "example.com",
    "foo.test"
  ],
  "paths": [
    "/foo",
    "/bar"
  ],
  "headers": {
    "x-my-header": [
      "foo",
      "bar"
    ],
    "x-another-header": [
      "bla"
    ]
  },
  "https_redirect_status_code": 426,
  "regex_priority": 0,
  "strip_path": true,
  "path_handling": "v0",
  "preserve_host": false,
  "request_buffering": true,
  "response_buffering": true,
  "tags": [
    "user-level",
    "low-priority"
  ],
  "service": {
    "id": "af8330d3-dbdc-48bd-b1be-55b98608834b"
  }
}
```

<h3 id="create-route-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID or name of the related service|
|body|body|object|false|Route request body|
|» name|body|string|false|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|» protocols|body|[string]|true|An array of the protocols this route should allow|
|» methods|body|[string]|false|A list of HTTP methods that match this route.|
|» hosts|body|[string]|false|A list of domain names that match this route. Note that the hosts value is case sensitive. With form-encoded, the notation is `hosts[]=example.com&hosts[]=foo.test`. With JSON, use an Array.|
|» paths|body|[string]|false|A list of paths that match this route. With form-encoded, the notation is `paths[]=/foo&paths[]=/bar`. With JSON, use an array. The path can be a regular expression, or a plain text pattern.|
|» headers|body|object|false|One or more lists of values indexed by header name that will cause this route to match if present in the request. The Host header cannot be used with this hosts should be specified using the `hosts` attribute. When headers contains only one value and that value starts with the special prefix` ~*`, the value is interpreted as a regular expression.|
|»» x-my-header|body|[string]|false|none|
|»» x-another-header|body|[string]|false|none|
|» https_redirect_status_code|body|integer|true|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`|
|» regex_priority|body|integer|false|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same regex_priority, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|» strip_path|body|boolean|false|When matching a route via one of the paths, strip the matching prefix from the upstream request URL.|
|» path_handling|body|string|false|Controls how the service path, route path and requested path are combined when sending a request to the upstream. Accepted values are "`v0`", "`v1`".|
|» preserve_host|body|boolean|true|When matching a route via one of the `hosts` domain names, use the request `host` header in the upstream request headers. If set to `false`, the upstream Host header will be that of the service's host.|
|» request_buffering|body|boolean|true|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding. Default: true.|
|» response_buffering|body|boolean|true|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding. Default: `true`.|
|» snis|body|[string]|false|A list of SNIs that match this route when using stream routing.|
|» sources|body|[object]|false|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» destinations|body|[object]|false|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the route for grouping and filtering.|
|» service|body|object|false|The service this route is associated to. This is where the route proxies traffic to. With form-encoded, the notation is service.id=<service id> or service.name=<service name>. With JSON, use `"service":{"id":"<service id>"}` or `"service":{"name":"<service name>"}`.|
|»» id|body|string|false|none|

#### Detailed descriptions

**» https_redirect_status_code**: The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`
Location header is injected by Kong if the field is set to `301`, `302`, `307` or `308`. Note: This config applies only if the route is configured to only accept the https protocol. Accepted values are: `426`, `301`, `302`, `307`, `308`. Default: `426`.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» https_redirect_status_code|426|
|» https_redirect_status_code|301|
|» https_redirect_status_code|302|
|» https_redirect_status_code|307|
|» https_redirect_status_code|308|
|» path_handling|v1|
|» path_handling|v0|

> Example responses

> Successfully created route

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

> 400 Response

```json
{}
```

<h3 id="create-route-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created route|[Route](#schemaroute)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid route|Inline|

<h3 id="create-route-for-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-route-for-service

<a id="opIddelete-route-for-service"></a>

`DELETE /services/{service_id_or_name}/routes/{route_id_or_name}`

*Delete a route associated with a service*

Delete a route associated with a service using ID or name.

<h3 id="delete-route-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

<h3 id="delete-route-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted route or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## fetch-route-for-service

<a id="opIdfetch-route-for-service"></a>

`GET /services/{service_id_or_name}/routes/{route_id_or_name}`

*Fetch a route associated with a service*

Get a route associated with a service using ID or name.

<h3 id="fetch-route-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

> Example responses

> Successfully fetched route

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

<h3 id="fetch-route-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched route|[Route](#schemaroute)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-route-for-service

<a id="opIdupdate-route-for-service"></a>

`PATCH /services/{service_id_or_name}/routes/{route_id_or_name}`

*Update a route associated with a service*

Update a route associated with a service using ID or name.

> Body parameter

```json
{
  "name": "my-route",
  "protocols": [
    "http",
    "https"
  ],
  "methods": [
    "GET",
    "POST"
  ],
  "hosts": [
    "example.com",
    "foo.test"
  ],
  "paths": [
    "/foo",
    "/bar"
  ],
  "headers": {
    "x-my-header": [
      "foo",
      "bar"
    ],
    "x-another-header": [
      "bla"
    ]
  },
  "https_redirect_status_code": 426,
  "regex_priority": 0,
  "strip_path": true,
  "path_handling": "v0",
  "preserve_host": false,
  "request_buffering": true,
  "response_buffering": true,
  "tags": [
    "user-level",
    "low-priority"
  ],
  "service": {
    "id": "af8330d3-dbdc-48bd-b1be-55b98608834b"
  }
}
```

<h3 id="update-route-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Route request body|
|» name|body|string|false|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|» protocols|body|[string]|true|An array of the protocols this route should allow|
|» methods|body|[string]|false|A list of HTTP methods that match this route.|
|» hosts|body|[string]|false|A list of domain names that match this route. Note that the hosts value is case sensitive. With form-encoded, the notation is `hosts[]=example.com&hosts[]=foo.test`. With JSON, use an Array.|
|» paths|body|[string]|false|A list of paths that match this route. With form-encoded, the notation is `paths[]=/foo&paths[]=/bar`. With JSON, use an array. The path can be a regular expression, or a plain text pattern.|
|» headers|body|object|false|One or more lists of values indexed by header name that will cause this route to match if present in the request. The Host header cannot be used with this hosts should be specified using the `hosts` attribute. When headers contains only one value and that value starts with the special prefix` ~*`, the value is interpreted as a regular expression.|
|»» x-my-header|body|[string]|false|none|
|»» x-another-header|body|[string]|false|none|
|» https_redirect_status_code|body|integer|true|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`|
|» regex_priority|body|integer|false|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same regex_priority, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|» strip_path|body|boolean|false|When matching a route via one of the paths, strip the matching prefix from the upstream request URL.|
|» path_handling|body|string|false|Controls how the service path, route path and requested path are combined when sending a request to the upstream. Accepted values are "`v0`", "`v1`".|
|» preserve_host|body|boolean|true|When matching a route via one of the `hosts` domain names, use the request `host` header in the upstream request headers. If set to `false`, the upstream Host header will be that of the service's host.|
|» request_buffering|body|boolean|true|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding. Default: true.|
|» response_buffering|body|boolean|true|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding. Default: `true`.|
|» snis|body|[string]|false|A list of SNIs that match this route when using stream routing.|
|» sources|body|[object]|false|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» destinations|body|[object]|false|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the route for grouping and filtering.|
|» service|body|object|false|The service this route is associated to. This is where the route proxies traffic to. With form-encoded, the notation is service.id=<service id> or service.name=<service name>. With JSON, use `"service":{"id":"<service id>"}` or `"service":{"name":"<service name>"}`.|
|»» id|body|string|false|none|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

#### Detailed descriptions

**» https_redirect_status_code**: The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`
Location header is injected by Kong if the field is set to `301`, `302`, `307` or `308`. Note: This config applies only if the route is configured to only accept the https protocol. Accepted values are: `426`, `301`, `302`, `307`, `308`. Default: `426`.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» https_redirect_status_code|426|
|» https_redirect_status_code|301|
|» https_redirect_status_code|302|
|» https_redirect_status_code|307|
|» https_redirect_status_code|308|
|» path_handling|v1|
|» path_handling|v0|

> Example responses

> Successfully updated route

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

> 400 Response

```json
{}
```

<h3 id="update-route-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated route|[Route](#schemaroute)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid route|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-route-for-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-route-for-service

<a id="opIdupsert-route-for-service"></a>

`PUT /services/{service_id_or_name}/routes/{route_id_or_name}`

*Upsert a route associated with a service*

Create or Update a route associated with a service using ID or name.

> Body parameter

```json
{
  "name": "my-route",
  "protocols": [
    "http",
    "https"
  ],
  "methods": [
    "GET",
    "POST"
  ],
  "hosts": [
    "example.com",
    "foo.test"
  ],
  "paths": [
    "/foo",
    "/bar"
  ],
  "headers": {
    "x-my-header": [
      "foo",
      "bar"
    ],
    "x-another-header": [
      "bla"
    ]
  },
  "https_redirect_status_code": 426,
  "regex_priority": 0,
  "strip_path": true,
  "path_handling": "v0",
  "preserve_host": false,
  "request_buffering": true,
  "response_buffering": true,
  "tags": [
    "user-level",
    "low-priority"
  ],
  "service": {
    "id": "af8330d3-dbdc-48bd-b1be-55b98608834b"
  }
}
```

<h3 id="upsert-route-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Route request body|
|» name|body|string|false|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|» protocols|body|[string]|true|An array of the protocols this route should allow|
|» methods|body|[string]|false|A list of HTTP methods that match this route.|
|» hosts|body|[string]|false|A list of domain names that match this route. Note that the hosts value is case sensitive. With form-encoded, the notation is `hosts[]=example.com&hosts[]=foo.test`. With JSON, use an Array.|
|» paths|body|[string]|false|A list of paths that match this route. With form-encoded, the notation is `paths[]=/foo&paths[]=/bar`. With JSON, use an array. The path can be a regular expression, or a plain text pattern.|
|» headers|body|object|false|One or more lists of values indexed by header name that will cause this route to match if present in the request. The Host header cannot be used with this hosts should be specified using the `hosts` attribute. When headers contains only one value and that value starts with the special prefix` ~*`, the value is interpreted as a regular expression.|
|»» x-my-header|body|[string]|false|none|
|»» x-another-header|body|[string]|false|none|
|» https_redirect_status_code|body|integer|true|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`|
|» regex_priority|body|integer|false|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same regex_priority, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|» strip_path|body|boolean|false|When matching a route via one of the paths, strip the matching prefix from the upstream request URL.|
|» path_handling|body|string|false|Controls how the service path, route path and requested path are combined when sending a request to the upstream. Accepted values are "`v0`", "`v1`".|
|» preserve_host|body|boolean|true|When matching a route via one of the `hosts` domain names, use the request `host` header in the upstream request headers. If set to `false`, the upstream Host header will be that of the service's host.|
|» request_buffering|body|boolean|true|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding. Default: true.|
|» response_buffering|body|boolean|true|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding. Default: `true`.|
|» snis|body|[string]|false|A list of SNIs that match this route when using stream routing.|
|» sources|body|[object]|false|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» destinations|body|[object]|false|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|»» ip|body|string|false|none|
|»» port|body|integer|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the route for grouping and filtering.|
|» service|body|object|false|The service this route is associated to. This is where the route proxies traffic to. With form-encoded, the notation is service.id=<service id> or service.name=<service name>. With JSON, use `"service":{"id":"<service id>"}` or `"service":{"name":"<service name>"}`.|
|»» id|body|string|false|none|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

#### Detailed descriptions

**» https_redirect_status_code**: The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`
Location header is injected by Kong if the field is set to `301`, `302`, `307` or `308`. Note: This config applies only if the route is configured to only accept the https protocol. Accepted values are: `426`, `301`, `302`, `307`, `308`. Default: `426`.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» https_redirect_status_code|426|
|» https_redirect_status_code|301|
|» https_redirect_status_code|302|
|» https_redirect_status_code|307|
|» https_redirect_status_code|308|
|» path_handling|v1|
|» path_handling|v0|

> Example responses

> Successfully upserted route

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}
```

> 400 Response

```json
{}
```

<h3 id="upsert-route-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted route|[Route](#schemaroute)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid route|Inline|

<h3 id="upsert-route-for-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-plugins">Plugins</h1>

A plugin entity represents a plugin configuration that will be executed during the HTTP request/response lifecycle. Plugins let you add functionality to services that run behind a Kong Gateway instance, like authentication or rate limiting.
You can find more information about available plugins and which values each plugin accepts at the [Plugin Hub](https://docs.konghq.com/hub/).
<br><br>
When adding a plugin configuration to a service, the plugin will run on every request made by a client to that service. If a plugin needs to be tuned to different values for some specific consumers, you can do so by creating a separate plugin instance that specifies both the service and the consumer, through the service and consumer fields.
<br><br>
Plugins can be both [tagged and filtered by tags](https://docs.konghq.com/gateway/latest/admin-api/#tags).

## list-plugins-for-consumer

<a id="opIdlist-plugins-for-consumer"></a>

`GET /consumers/{consumer_username_or_id}/plugins`

*List all plugins associated with a consumer*

Retrieve a list of all plugins associated with a consumer.

<h3 id="list-plugins-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> Example response

```json
{
  "data": [
    {
      "id": "02621eee-8309-4bf6-b36b-a82017a5393e",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "http",
        "https"
      ],
      "enabled": true,
      "tags": [
        "user-level",
        "low-priority"
      ],
      "ordering": {
        "before": [
          "plugin-name"
        ]
      }
    },
    {
      "id": "66c7b5c4-4aaf-4119-af1e-ee3ad75d0af4",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "tcp",
        "tls"
      ],
      "enabled": true,
      "tags": [
        "admin",
        "high-priority",
        "critical"
      ],
      "ordering": {
        "after": [
          "plugin-name"
        ]
      }
    }
  ],
  "next": "http://localhost:8001/plugins?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="list-plugins-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Example response|Inline|

<h3 id="list-plugins-for-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» created_at|integer|false|none|Unix epoch when the resource was created.|
|» route|string¦null|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|string¦null|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|string¦null|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|string|false|none|The Plugin instance name.|
|» config|object|false|none|The configuration properties for the Plugin|
|»» hour|integer|false|none|none|
|»» minute|integer|false|none|none|
|» protocols|[string]|false|none|A list of the request protocols that will trigger this plugin.|
|» enabled|boolean|false|none|Whether the plugin is applied. Default: `true`.|
|» tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|object|false|none|Describes a dependency to another plugin to determine plugin ordering during the access phase.<br>- `before`: The plugin will be executed before a specified plugin or list of plugins.<br>- `after`: The plugin will be executed after a specified plugin or list of plugins.|
|»» before|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## create-plugin-for-consumer

<a id="opIdcreate-plugin-for-consumer"></a>

`POST /consumers/{consumer_username_or_id}/plugins`

*Create a new Plugin associated with a Consumer*

Create a new Plugin associated with a Consumer

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="create-plugin-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> 201 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="create-plugin-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="create-plugin-for-consumer-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-plugin-for-consumer

<a id="opIddelete-plugin-for-consumer"></a>

`DELETE /consumers/{consumer_username_or_id}/plugins/{plugin_id}`

*Delete a Plugin associated with a Consumer*

Delete a Plugin associated with a Consumer using ID.

<h3 id="delete-plugin-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

<h3 id="delete-plugin-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Plugin or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## fetch-plugin-for-consumer

<a id="opIdfetch-plugin-for-consumer"></a>

`GET /consumers/{consumer_username_or_id}/plugins/{plugin_id}`

*Fetch a Plugin associated with a Consumer*

Get a Plugin associated with a Consumer using ID.

<h3 id="fetch-plugin-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="fetch-plugin-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Plugin|[Plugin](#schemaplugin)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-plugin-for-consumer

<a id="opIdupdate-plugin-for-consumer"></a>

`PATCH /consumers/{consumer_username_or_id}/plugins/{plugin_id}`

*Update a Plugin associated with a Consumer*

Update a Plugin associated with a consumer using the consumer username or ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="update-plugin-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> Example response

```json
{
  "data": [
    {
      "id": "02621eee-8309-4bf6-b36b-a82017a5393e",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "http",
        "https"
      ],
      "enabled": true,
      "tags": [
        "user-level",
        "low-priority"
      ],
      "ordering": {
        "before": [
          "plugin-name"
        ]
      }
    },
    {
      "id": "66c7b5c4-4aaf-4119-af1e-ee3ad75d0af4",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "tcp",
        "tls"
      ],
      "enabled": true,
      "tags": [
        "admin",
        "high-priority",
        "critical"
      ],
      "ordering": {
        "after": [
          "plugin-name"
        ]
      }
    }
  ],
  "next": "http://localhost:8001/plugins?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

> 400 Response

```json
{}
```

<h3 id="update-plugin-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Example response|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-plugin-for-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» created_at|integer|false|none|Unix epoch when the resource was created.|
|» route|string¦null|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|string¦null|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|string¦null|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|string|false|none|The Plugin instance name.|
|» config|object|false|none|The configuration properties for the Plugin|
|»» hour|integer|false|none|none|
|»» minute|integer|false|none|none|
|» protocols|[string]|false|none|A list of the request protocols that will trigger this plugin.|
|» enabled|boolean|false|none|Whether the plugin is applied. Default: `true`.|
|» tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|object|false|none|Describes a dependency to another plugin to determine plugin ordering during the access phase.<br>- `before`: The plugin will be executed before a specified plugin or list of plugins.<br>- `after`: The plugin will be executed after a specified plugin or list of plugins.|
|»» before|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## upsert-plugin-for-consumer

<a id="opIdupsert-plugin-for-consumer"></a>

`PUT /consumers/{consumer_username_or_id}/plugins/{plugin_id}`

*Upsert a Plugin associated with a Consumer*

Create or Update a Plugin associated with a Consumer using ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="upsert-plugin-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> Example response

```json
{
  "data": [
    {
      "id": "02621eee-8309-4bf6-b36b-a82017a5393e",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "http",
        "https"
      ],
      "enabled": true,
      "tags": [
        "user-level",
        "low-priority"
      ],
      "ordering": {
        "before": [
          "plugin-name"
        ]
      }
    },
    {
      "id": "66c7b5c4-4aaf-4119-af1e-ee3ad75d0af4",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "tcp",
        "tls"
      ],
      "enabled": true,
      "tags": [
        "admin",
        "high-priority",
        "critical"
      ],
      "ordering": {
        "after": [
          "plugin-name"
        ]
      }
    }
  ],
  "next": "http://localhost:8001/plugins?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="upsert-plugin-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Example response|Inline|

<h3 id="upsert-plugin-for-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» created_at|integer|false|none|Unix epoch when the resource was created.|
|» route|string¦null|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|string¦null|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|string¦null|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|string|false|none|The Plugin instance name.|
|» config|object|false|none|The configuration properties for the Plugin|
|»» hour|integer|false|none|none|
|»» minute|integer|false|none|none|
|» protocols|[string]|false|none|A list of the request protocols that will trigger this plugin.|
|» enabled|boolean|false|none|Whether the plugin is applied. Default: `true`.|
|» tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|object|false|none|Describes a dependency to another plugin to determine plugin ordering during the access phase.<br>- `before`: The plugin will be executed before a specified plugin or list of plugins.<br>- `after`: The plugin will be executed after a specified plugin or list of plugins.|
|»» before|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## list-plugin

<a id="opIdlist-plugin"></a>

`GET /plugins`

*List all Plugins*

This endpoint allows you to list all the plugins. You can use query parameters to filter the results by size or tags, for example `/plugins?size=50&tags=enterprise`.

<h3 id="list-plugin-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> Example response

```json
{
  "data": [
    {
      "id": "02621eee-8309-4bf6-b36b-a82017a5393e",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "http",
        "https"
      ],
      "enabled": true,
      "tags": [
        "user-level",
        "low-priority"
      ],
      "ordering": {
        "before": [
          "plugin-name"
        ]
      }
    },
    {
      "id": "66c7b5c4-4aaf-4119-af1e-ee3ad75d0af4",
      "name": "rate-limiting",
      "created_at": 1422386534,
      "route": null,
      "service": null,
      "consumer": null,
      "config": {
        "hour": 500,
        "minute": 20
      },
      "protocols": [
        "tcp",
        "tls"
      ],
      "enabled": true,
      "tags": [
        "admin",
        "high-priority",
        "critical"
      ],
      "ordering": {
        "after": [
          "plugin-name"
        ]
      }
    }
  ],
  "next": "http://localhost:8001/plugins?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="list-plugin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Example response|Inline|

<h3 id="list-plugin-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» created_at|integer|false|none|Unix epoch when the resource was created.|
|» route|string¦null|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|string¦null|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|string¦null|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|string|false|none|The Plugin instance name.|
|» config|object|false|none|The configuration properties for the Plugin|
|»» hour|integer|false|none|none|
|»» minute|integer|false|none|none|
|» protocols|[string]|false|none|A list of the request protocols that will trigger this plugin.|
|» enabled|boolean|false|none|Whether the plugin is applied. Default: `true`.|
|» tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|object|false|none|Describes a dependency to another plugin to determine plugin ordering during the access phase.<br>- `before`: The plugin will be executed before a specified plugin or list of plugins.<br>- `after`: The plugin will be executed after a specified plugin or list of plugins.|
|»» before|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## create-plugin

<a id="opIdcreate-plugin"></a>

`POST /plugins`

*Create a new Plugin*

Create a new Plugin

>Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="create-plugin-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> Successfully created Plugin

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

> 400 Response

```json
{}
```

<h3 id="create-plugin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="create-plugin-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-plugin

<a id="opIddelete-plugin"></a>

`DELETE /plugins/{plugin_id}`

*Delete a Plugin*

Delete a Plugin

<h3 id="delete-plugin-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

<h3 id="delete-plugin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Plugin or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-plugin

<a id="opIdget-plugin"></a>

`GET /plugins/{plugin_id}`

*Fetch a Plugin*

Get a Plugin using ID.

<h3 id="get-plugin-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="get-plugin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Plugin|[Plugin](#schemaplugin)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-plugin

<a id="opIdupdate-plugin"></a>

`PATCH /plugins/{plugin_id}`

*Update a Plugin*

Update a Plugin

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="update-plugin-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> Successfully updated Plugin

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

> 400 Response

```json
{}
```

<h3 id="update-plugin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-plugin-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-plugin

<a id="opIdupsert-plugin"></a>

`PUT /plugins/{plugin_id}`

*Upsert a Plugin*

Create or Update Plugin using ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="upsert-plugin-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> Successfully upserted Plugin

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

> 400 Response

```json
{}
```

<h3 id="upsert-plugin-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="upsert-plugin-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## list-plugins-for-route

<a id="opIdlist-plugins-for-route"></a>

`GET /routes/{route_id_or_name}/plugins`

*List all Plugins associated with a route*

List all Plugins associated with a route

<h3 id="list-plugins-for-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "config": {
        "anonymous": null,
        "hide_credentials": false,
        "key_in_body": false,
        "key_in_header": true,
        "key_in_query": true,
        "key_names": [
          "apikey"
        ],
        "run_on_preflight": true
      },
      "enabled": true,
      "id": "3fd1eea1-885a-4011-b986-289943ff8177",
      "name": "key-auth",
      "protocols": [
        "grpc",
        "grpcs",
        "http",
        "https"
      ]
    }
  ],
  "offset": "string"
}
```

<h3 id="list-plugins-for-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Plugins|Inline|

<h3 id="list-plugins-for-route-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Plugin](#schemaplugin)]|false|none|[A Plugin entity represents a plugin configuration that will be executed during the HTTP request/response lifecycle.]|
|»» Plugin|[Plugin](#schemaplugin)|false|none|A Plugin entity represents a plugin configuration that will be executed during the HTTP request/response lifecycle.|
|»»» config|object|false|none|The configuration properties for the Plugin which can be found on the plugins documentation page in the [Kong Hub](https://docs.konghq.com/hub/).|
|»»» consumer|object|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.). Leave unset for the plugin to activate regardless of the authenticated Consumer.|
|»»»» id|string|false|none|none|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» enabled|boolean|false|none|Whether the plugin is applied.|
|»»» id|string|false|none|none|
|»»» instance_name|string|false|none|none|
|»»» name|string|false|none|The name of the Plugin thats going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|»»» ordering|object|false|none|none|
|»»» protocols|[string]|false|none|A list of the request protocols that will trigger this plugin. The default value, as well as the possible values allowed on this field, may change depending on the plugin type. For example, plugins that only work in stream mode will only support `"tcp"` and `"tls"`.|
|»»» route|object|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used.|
|»»»» id|string|false|none|none|
|»»» service|object|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service. Leave unset for the plugin to activate regardless of the service being matched.|
|»»»» id|string|false|none|none|
|»»» tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|»»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-plugin-for-route

<a id="opIdcreate-plugin-for-route"></a>

`POST /routes/{route_id_or_name}/plugins`

*Create a new Plugin associated with a route*

Create a new Plugin associated with a route

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="create-plugin-for-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> Successfully created Plugin

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

> 400 Response

```json
{}
```

<h3 id="create-plugin-for-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="create-plugin-for-route-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-plugin-for-route

<a id="opIddelete-plugin-for-route"></a>

`DELETE /routes/{route_id_or_name}/plugins/{plugin_id}`

*Delete a Plugin associated with a route*

Delete a Plugin associated with a route using ID.

<h3 id="delete-plugin-for-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

<h3 id="delete-plugin-for-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Plugin or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## fetch-plugin-for-route

<a id="opIdfetch-plugin-for-route"></a>

`GET /routes/{route_id_or_name}/plugins/{plugin_id}`

*Fetch a Plugin associated with a route*

Get a Plugin associated with a route using ID.

<h3 id="fetch-plugin-for-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|route_id_or_name|path|string|true|ID or name of the related route|
|plugin_id|path|string|true|ID of the route to lookup|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="fetch-plugin-for-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Plugin|[Plugin](#schemaplugin)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-plugin-for-route

<a id="opIdupdate-plugin-for-route"></a>

`PATCH /routes/{route_id_or_name}/plugins/{plugin_id}`

*Update a Plugin associated with a route*

Update a Plugin associated with a route using ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="update-plugin-for-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="update-plugin-for-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-plugin-for-route-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-plugin-for-route

<a id="opIdupsert-plugin-for-route"></a>

`PUT /routes/{route_id_or_name}/plugins/{plugin_id}`

*Upsert a Plugin associated with a route*

Create or Update a Plugin associated with a route using ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="upsert-plugin-for-route-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|route_id_or_name|path|string|true|The unique identifier or the name of the route to retrieve.|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="upsert-plugin-for-route-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="upsert-plugin-for-route-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## get-plugins-for-service

<a id="opIdget-plugins-for-service"></a>

`GET /services/{service_id_or_name}plugins`

*List all Plugins associated with a service*

List all Plugins associated with a service

<h3 id="get-plugins-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "config": {
        "anonymous": null,
        "hide_credentials": false,
        "key_in_body": false,
        "key_in_header": true,
        "key_in_query": true,
        "key_names": [
          "apikey"
        ],
        "run_on_preflight": true
      },
      "enabled": true,
      "id": "3fd1eea1-885a-4011-b986-289943ff8177",
      "name": "key-auth",
      "protocols": [
        "grpc",
        "grpcs",
        "http",
        "https"
      ]
    }
  ],
  "offset": "string"
}
```

<h3 id="get-plugins-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Plugins|Inline|

<h3 id="get-plugins-for-service-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Plugin](#schemaplugin)]|false|none|[A Plugin entity represents a plugin configuration that will be executed during the HTTP request/response lifecycle.]|
|»» Plugin|[Plugin](#schemaplugin)|false|none|A Plugin entity represents a plugin configuration that will be executed during the HTTP request/response lifecycle.|
|»»» config|object|false|none|The configuration properties for the Plugin which can be found on the plugins documentation page in the [Kong Hub](https://docs.konghq.com/hub/).|
|»»» consumer|object|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.). Leave unset for the plugin to activate regardless of the authenticated Consumer.|
|»»»» id|string|false|none|none|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» enabled|boolean|false|none|Whether the plugin is applied.|
|»»» id|string|false|none|none|
|»»» instance_name|string|false|none|none|
|»»» name|string|false|none|The name of the Plugin thats going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|»»» ordering|object|false|none|none|
|»»» protocols|[string]|false|none|A list of the request protocols that will trigger this plugin. The default value, as well as the possible values allowed on this field, may change depending on the plugin type. For example, plugins that only work in stream mode will only support `"tcp"` and `"tls"`.|
|»»» route|object|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used.|
|»»»» id|string|false|none|none|
|»»» service|object|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service. Leave unset for the plugin to activate regardless of the service being matched.|
|»»»» id|string|false|none|none|
|»»» tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|»»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-plugin-for-service

<a id="opIdcreate-plugin-for-service"></a>

`POST /services/{service_id_or_name}plugins`

*Create a new Plugin associated with a service*

Create a new Plugin associated with a service

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="create-plugin-for-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> 201 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="create-plugin-for-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="create-plugin-for-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-plugin-for-a-service

<a id="opIddelete-plugin-for-a-service"></a>

`DELETE /services/{service_id_or_name}/plugins/{plugin_id}`

*Delete a plugin associated with a service*

Delete a Plugin associated with a service using ID.

<h3 id="delete-plugin-for-a-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

<h3 id="delete-plugin-for-a-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Plugin or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## fetch-plugin-with-a-service

<a id="opIdfetch-plugin-with-a-service"></a>

`GET /services/{service_id_or_name}/plugins/{plugin_id}`

*Fetch a Plugin associated with a service*

Get a Plugin associated with a service using ID.

<h3 id="fetch-plugin-with-a-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="fetch-plugin-with-a-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Plugin|[Plugin](#schemaplugin)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-plugin-for-a-service

<a id="opIdupdate-plugin-for-a-service"></a>

`PATCH /services/{service_id_or_name}/plugins/{plugin_id}`

*Update a plugin associated with a service*

Update a Plugin associated with a service using ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="update-plugin-for-a-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="update-plugin-for-a-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-plugin-for-a-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-plugin-for-a-service

<a id="opIdupsert-plugin-for-a-service"></a>

`PUT /services/{service_id_or_name}/plugins/{plugin_id}`

*Upsert a plugin associated with a service*

Create or Update a Plugin associated with a service using ID.

> Body parameter

```json
{
  "name": "rate-limiting",
  "route": "string",
  "service": "string",
  "consumer": "string",
  "instance_name": "rate-limiting-foo",
  "config": {
    "hour": 500,
    "minute": 500
  },
  "protocols": [
    "http"
  ],
  "enabled": true,
  "tags": [
    "string"
  ],
  "ordering": {
    "before": [
      "string"
    ]
  }
}
```

<h3 id="upsert-plugin-for-a-service-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Plugin request body|
|» name|body|string|false|The name of the Plugin that's going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|» route|body|string¦null|false|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used. Default: `null`. With form-encoded, the notation is `route.id=<route id> or route.name=<route name>`. With JSON, use `"route":{"id":"<route id>"}` or `"route":{"name":"<route name>"}`.|
|» service|body|string¦null|false|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service.|
|» consumer|body|string¦null|false|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.)|
|» instance_name|body|string|false|The Plugin instance name.|
|» config|body|object|false|The configuration properties for the Plugin|
|» protocols|body|[string]|false|A list of the request protocols that will trigger this plugin.|
|» enabled|body|boolean|false|Whether the plugin is applied. Default: `true`.|
|» tags|body|[string]|false|An optional set of strings associated with the Plugin for grouping and filtering.|
|» ordering|body|object|false|Describes a dependency to another plugin to determine plugin ordering during the access phase.|
|»» before|body|[string]|false|none|
|service_id_or_name|path|string|true|ID **or** name of the service to lookup|
|plugin_id|path|string|true|The unique identifier of the Plugin to create or update.|

#### Detailed descriptions

**» ordering**: Describes a dependency to another plugin to determine plugin ordering during the access phase.
- `before`: The plugin will be executed before a specified plugin or list of plugins.
- `after`: The plugin will be executed after a specified plugin or list of plugins.

#### Enumerated Values

|Parameter|Value|
|---|---|
|» protocols|http|
|» protocols|grpc|
|» protocols|grpcs|
|» protocols|tls|
|» protocols|tcp|

> Example responses

> 200 Response

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}
```

<h3 id="upsert-plugin-for-a-service-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Plugin|[Plugin](#schemaplugin)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Plugin|Inline|

<h3 id="upsert-plugin-for-a-service-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-consumers">Consumers</h1>

The consumer object represents a consumer - or a user - of a service. 
You can either rely on Kong Gateway as the primary datastore, or you can map the consumer list with your database to keep consistency between Kong Gateway and your existing primary datastore.

## list-consumer

<a id="opIdlist-consumer"></a>

`GET /consumers`

*List all Consumers*

Retrieve a list of all consumers.You can use query parameters to filter the results by size or tags, for example `/consumers?size=50&tags=enterprise`.

<h3 id="list-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "custom_id": "4200",
      "id": "8a388226-80e8-4027-a486-25e4f7db5d21",
      "tags": [
        "silver-tier"
      ],
      "username": "bob-the-builder"
    }
  ],
  "offset": "string"
}
```

<h3 id="list-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Consumers|Inline|

<h3 id="list-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Consumer](#schemaconsumer)]|false|none|[The Consumer object represents a consumer - or a user - of a service. You can either rely on Kong as the primary datastore, or you can map the consumer list with your database to keep consistency between Kong and your existing primary datastore.]|
|»» Consumer|[Consumer](#schemaconsumer)|false|none|The Consumer object represents a consumer - or a user - of a service. You can either rely on Kong as the primary datastore, or you can map the consumer list with your database to keep consistency between Kong and your existing primary datastore.|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» custom_id|string|false|none|Field for storing an existing unique ID for the Consumer - useful for mapping Kong with users in your existing database. You must send either this field or `username` with the request.|
|»»» id|string|false|none|none|
|»»» tags|[string]|false|none|An optional set of strings associated with the Consumer for grouping and filtering.|
|»»» username|string|false|none|The unique username of the Consumer. You must send either this field or `custom_id` with the request.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-consumer

<a id="opIdcreate-consumer"></a>

`POST /consumers`

*Create a new Consumer*

Create a new Consumer

> Body parameter

```json
{
  "username": "string",
  "custom_id": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Consumer request body|
|» username|body|string|true|The unique username of the Consumer. You must send either this field or custom_id with the request.|
|» custom_id|body|string|true|Field for storing an existing unique ID for the Consumer - useful for mapping Kong with users in your existing database. You must send either this field or username with the request.|
|» tags|body|[string]|false|An optional set of strings associated with the Consumer for grouping and filtering.|

> Example responses

> 201 Response

```json
{
  "custom_id": "4200",
  "id": "8a388226-80e8-4027-a486-25e4f7db5d21",
  "tags": [
    "silver-tier"
  ],
  "username": "bob-the-builder"
}
```

<h3 id="create-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Consumer|[Consumer](#schemaconsumer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Consumer|Inline|

<h3 id="create-consumer-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-consumer

<a id="opIddelete-consumer"></a>

`DELETE /consumers/{consumer_username_or_id}`

*Delete a Consumer*

Delete a specific consumer from the system using either the consumer ID or the consumer username. This operation is irreversible and permanently removes all data associated with the specified consumer. If the consumer was deleted succesfully the endpoint will return a 204 response indicating that the resource did not exist.

<h3 id="delete-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

<h3 id="delete-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Consumer or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-consumer

<a id="opIdget-consumer"></a>

`GET /consumers/{consumer_username_or_id}`

*Fetch a Consumer*

Retrieve the details of a specific consumer in the system using either the consumer ID or the consumer username. If the consumer with the specified ID or username cannot be found, the endpoint will return a 404.

<h3 id="get-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 200 Response

```json
{
  "custom_id": "4200",
  "id": "8a388226-80e8-4027-a486-25e4f7db5d21",
  "tags": [
    "silver-tier"
  ],
  "username": "bob-the-builder"
}
```

<h3 id="get-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Consumer|[Consumer](#schemaconsumer)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-consumer

<a id="opIdupdate-consumer"></a>

`PATCH /consumers/{consumer_username_or_id}`

*Update a Consumer*

Update the details of a specific consumer in the system using either the consumer ID or the consumer username.If the consumer with the specified ID or username cannot be found, the endpoint will return a 404.

> Body parameter

```json
{
  "username": "string",
  "custom_id": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="update-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Consumer request body|
|» username|body|string|true|The unique username of the Consumer. You must send either this field or custom_id with the request.|
|» custom_id|body|string|true|Field for storing an existing unique ID for the Consumer - useful for mapping Kong with users in your existing database. You must send either this field or username with the request.|
|» tags|body|[string]|false|An optional set of strings associated with the Consumer for grouping and filtering.|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 200 Response

```json
{
  "custom_id": "4200",
  "id": "8a388226-80e8-4027-a486-25e4f7db5d21",
  "tags": [
    "silver-tier"
  ],
  "username": "bob-the-builder"
}
```

<h3 id="update-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Consumer|[Consumer](#schemaconsumer)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Consumer|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-consumer-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-consumer

<a id="opIdupsert-consumer"></a>

`PUT /consumers/{consumer_username_or_id}`

*Update a Consumer*

Create or Update Consumer using ID or username. The consumer will be identified via the username or id attribute.If the consumer with the specified ID or username cannot be found, the endpoint will return a 404.

When the username or id attribute has the structure of a UUID, the Consumer being inserted/replaced will be identified by its id. Otherwise it will be identified by its username.

When creating a new Consumer without specifying id (neither in the URL nor in the body), then it will be auto-generated.

Notice that specifying a username in the URL and a different one in the request body is not allowed.

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "username": "string",
  "custom_id": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="upsert-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|Consumer request body|
|» username|body|string|true|The unique username of the Consumer. You must send either this field or custom_id with the request.|
|» custom_id|body|string|true|Field for storing an existing unique ID for the Consumer - useful for mapping Kong with users in your existing database. You must send either this field or username with the request.|
|» tags|body|[string]|false|An optional set of strings associated with the Consumer for grouping and filtering.|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "a4407883-c166-43fd-80ca-3ca035b0cdb7",
      "created_at": 1422386534,
      "username": "my-username",
      "custom_id": "my-custom-id",
      "tags": [
        "admin"
      ]
    }
  ],
  "next": "http://localhost:8001/consumers?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="upsert-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The consumer object response body|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not Found|None|

<h3 id="upsert-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|none|
|»» id|string|false|none|The unique identifier or the name attribute of the consumer.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» username|string|false|none|The unique username of the consumer. You must send either this field or` custom_i`d with the request.|
|»» custom_id|string|false|none|Field for storing an existing unique ID for the Consumer - useful for mapping Kong with users in your existing database.|
|»» tags|[string]|false|none|An optional set of strings associated with the Consumer for grouping and filtering.|
|» next|string|false|none|Pagination information|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-certificates">Certificates</h1>

A certificate object represents a public certificate, and can be optionally paired with the corresponding private key. These objects are used by Kong Gateway to handle SSL/TLS termination for encrypted requests, or for use as a trusted CA store when validating peer certificate of client/service. 
<br><br>  
Certificates are optionally associated with SNI objects to tie a cert/key pair to one or more hostnames. 
<br><br>
If intermediate certificates are required in addition to the main certificate, they should be concatenated together into one string.

## list-certificate

<a id="opIdlist-certificate"></a>

`GET /certificates`

*List all certificates*

Retrieve a list of all available CA Certificate Authority (CA) certificates. You can use query parameters to filter the results by size or tags, for example `/certificates?size=50&tags=enterprise`.

<h3 id="list-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
      "id": "b2f34145-0343-41a4-9602-4c69dec2f269",
      "key": "-----BEGIN PRIVATE KEY-----\nprivate-key-content\n-----END PRIVATE KEY-----"
    }
  ],
  "offset": "string"
}
```

<h3 id="list-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Certificates|Inline|

<h3 id="list-certificate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Certificate](#schemacertificate)]|false|none|[A certificate object represents a public certificate. These fields are _referenceable_, and can be stored as [secrets](http://docs.konqhq.com/gateway/latest/plan-and-deploy/security/secrets-management/getting-started) in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).]|
|»» Certificate|[Certificate](#schemacertificate)|false|none|A certificate object represents a public certificate. These fields are _referenceable_, and can be stored as [secrets](http://docs.konqhq.com/gateway/latest/plan-and-deploy/security/secrets-management/getting-started) in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|»»» cert|string|false|none|PEM-encoded public certificate chain of the SSL key This field is referenceable and can be stored in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|»»» cert_alt|string|false|none|PEM-encoded public certificate chain of the alternate SSL key pair. This should only be set if you have both RSA and ECDSA types of certificate available and would like Kong to prefer serving using ECDSA certs.|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» id|string(uuid)|false|none|The UUID representation of the certificate object.|
|»»» key|string|false|none|PEM-encoded private key of the SSL key pair. This field is _referenceable_, which means it can be securely stored as a [secret](/gateway/latest/plan-and-deploy/security/secrets-management/getting-started) in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|»»» key_alt|string|false|none|PEM-encoded private key of the alternate SSL key pair. This should only be set if you have both RSA and ECDSA types of certificate available and would like Kong to prefer serving using ECDSA certs when client advertises support for it. This field is _referenceable_, which means it can be securely stored as a [secret](/gateway/latest/plan-and-deploy/security/secrets-management/getting-started) in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|»»» tags|[string]|false|none|An optional set of strings associated with the Certificate for grouping and filtering.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-certificate

<a id="opIdcreate-certificate"></a>

`POST /certificates`

*Create a new Certificate*

Create a new certificate with the provided details. Use this endpoint to add a new certificate to the system. The request body must include the certificate data and other details required for creating a new certificate.

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "cert": "\"-----BEGIN CERTIFICATE-----...\",",
  "key": "\"-----BEGIN RSA PRIVATE KEY-----...\"",
  "cert_alt": "string",
  "key_alt": "\"-----BEGIN EC PRIVATE KEY-----...\"",
  "snis": [
    "string"
  ],
  "tags": [
    "string"
  ],
  "passphrase": "example"
}
```

<h3 id="create-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» cert|body|string|true|PEM-encoded public certificate chain of the SSL key pair.|
|» key|body|string|true|PEM-encoded private key of the SSL key pair.|
|» cert_alt|body|string|false|PEM-encoded public certificate chain of the alternate SSL key pair.|
|» key_alt|body|string|false|PEM-encoded private key of the alternate SSL key pair.|
|» snis|body|[string]|false|An array of zero or more hostnames to associate with this certificate as SNIs.|
|» tags|body|[string]|false|An optional set of strings associated with the Certificate for grouping and filtering.|
|» passphrase|body|string|false|To load an encrypted private key into Kong, specify the passphrase using this attributKong will decrypt the private key and store it in its database. e. Enterprise Only|

> Example responses

> 201 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f269",
  "key": "-----BEGIN PRIVATE KEY-----\nprivate-key-content\n-----END PRIVATE KEY-----"
}
```

<h3 id="create-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Certificate|[Certificate](#schemacertificate)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Certificate|Inline|

<h3 id="create-certificate-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-certificate

<a id="opIddelete-certificate"></a>

`DELETE /certificates/{certificate_id}`

*Delete a Certificate*

Delete a Certificate

>Note: This API is not available in DB-less mode.

<h3 id="delete-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|certificate_id|path|string|true|ID of the Certificate to delete|

<h3 id="delete-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Certificate or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-certificate

<a id="opIdget-certificate"></a>

`GET /certificates/{certificate_id}`

*Fetch a Certificate*

Retrieve details about the specified certificate using the provided path parameter `certificate_id`.

<h3 id="get-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|certificate_id|path|string|true|The unique identifier of the Certificate to retrieve.|

> Example responses

> 200 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f269",
  "key": "-----BEGIN PRIVATE KEY-----\nprivate-key-content\n-----END PRIVATE KEY-----"
}
```

<h3 id="get-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|HTTP 200 OK|[Certificate](#schemacertificate)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-certificate

<a id="opIdupdate-certificate"></a>

`PATCH /certificates/{certificate_id}`

*Update a Certificate*

Update a Certificate

Inserts (or replaces) the certificate under the requested `certificate_id`with the definition specified in the request body. When the `name` or `id` attribute has the structure of a UUID, the certificate being inserted/replaced will be identified by its `id`. Otherwise it will be identified by the `name`.

When creating a new Certificate without specifying `id` (neither in the path or the request body), then it will be auto-generated.

>Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "cert": "\"-----BEGIN CERTIFICATE-----...\",",
  "key": "\"-----BEGIN RSA PRIVATE KEY-----...\"",
  "cert_alt": "string",
  "key_alt": "\"-----BEGIN EC PRIVATE KEY-----...\"",
  "snis": [
    "string"
  ],
  "tags": [
    "string"
  ],
  "passphrase": "example"
}
```

<h3 id="update-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|certificate_id|path|string|true|ID of the Certificate to update|
|body|body|object|false|none|
|» cert|body|string|true|PEM-encoded public certificate chain of the SSL key pair.|
|» key|body|string|true|PEM-encoded private key of the SSL key pair.|
|» cert_alt|body|string|false|PEM-encoded public certificate chain of the alternate SSL key pair.|
|» key_alt|body|string|false|PEM-encoded private key of the alternate SSL key pair.|
|» snis|body|[string]|false|An array of zero or more hostnames to associate with this certificate as SNIs.|
|» tags|body|[string]|false|An optional set of strings associated with the Certificate for grouping and filtering.|
|» passphrase|body|string|false|To load an encrypted private key into Kong, specify the passphrase using this attributKong will decrypt the private key and store it in its database. e. Enterprise Only|

> Example responses

> 200 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f269",
  "key": "-----BEGIN PRIVATE KEY-----\nprivate-key-content\n-----END PRIVATE KEY-----"
}
```

<h3 id="update-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Certificate|[Certificate](#schemacertificate)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Certificate|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-certificate-put

<a id="opIdupdate-certificate-put"></a>

`PUT /certificates/{certificate_id}`

*Update a Certificate*

Update details about the specified certificate using the provided path parameter `certificate_id`.

Inserts (or replaces) the certificate under the requested `certificate_id`with the definition specified in the request body. When the `name` or `id` attribute has the structure of a UUID, the certificate being inserted/replaced will be identified by its `id`. Otherwise it will be identified by the `name`.

When creating a new Certificate without specifying `id` (neither in the path or the request body), then it will be auto-generated.

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "cert": "\"-----BEGIN CERTIFICATE-----...\",",
  "key": "\"-----BEGIN RSA PRIVATE KEY-----...\"",
  "cert_alt": "string",
  "key_alt": "\"-----BEGIN EC PRIVATE KEY-----...\"",
  "snis": [
    "string"
  ],
  "tags": [
    "string"
  ],
  "passphrase": "example"
}
```

<h3 id="update-certificate-put-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» cert|body|string|true|PEM-encoded public certificate chain of the SSL key pair.|
|» key|body|string|true|PEM-encoded private key of the SSL key pair.|
|» cert_alt|body|string|false|PEM-encoded public certificate chain of the alternate SSL key pair.|
|» key_alt|body|string|false|PEM-encoded private key of the alternate SSL key pair.|
|» snis|body|[string]|false|An array of zero or more hostnames to associate with this certificate as SNIs.|
|» tags|body|[string]|false|An optional set of strings associated with the Certificate for grouping and filtering.|
|» passphrase|body|string|false|To load an encrypted private key into Kong, specify the passphrase using this attributKong will decrypt the private key and store it in its database. e. Enterprise Only|
|certificate_id|path|string|true|The unique identifier of the Certificate to retrieve.|

> Example responses

> 200 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f269",
  "key": "-----BEGIN PRIVATE KEY-----\nprivate-key-content\n-----END PRIVATE KEY-----"
}
```

<h3 id="update-certificate-put-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Certificate|[Certificate](#schemacertificate)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Certificate|Inline|

<h3 id="update-certificate-put-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-snis">SNIs</h1>

An SNI object represents a many-to-one mapping of hostnames to a certificate. 
<br><br>
A certificate object can have many hostnames associated with it. When Kong Gateway receives an SSL request, it uses the SNI field in the Client Hello to look up the certificate object based on the SNI associated with the certificate.

## get-sni-with-certificate

<a id="opIdget-sni-with-certificate"></a>

`GET /certificates/{certificate_name_or_id}/snis`

*List SNIs associated with a certificate*

Retrieve a paginated list of all SNIs associated with a certificate. Use this endpoint to retrieve a list of SNIs that are linked to a specific certificate. You can use the optional query parameters to filter the results based on specific criteria. The response will include the list of SNIs and pagination information. See the response schema for details on the expected format of the response body.

<h3 id="get-sni-with-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|
|certificate_name_or_id|path|string|true|The unique identifier or the `name` attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|certificate_name_or_id|a3ad71a8-6685-4b03-a101-980a953544f6|
|certificate_name_or_id|name|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "147f5ef0-1ed6-4711-b77f-489262f8bff7",
      "name": "my-sni",
      "created_at": 1422386534,
      "tags": [
        "string"
      ],
      "certificate": {
        "id": "2e013e8-7623-4494-a347-6d29108ff68b"
      }
    }
  ],
  "next": "http://localhost:8001/snis?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="get-sni-with-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SNI response object|Inline|

<h3 id="get-sni-with-certificate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|Array of SNIs|
|»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|
|»» name|string|false|none|The SNI name to associate with the given certificate.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|
|»» certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|»»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-sni-for-certificate

<a id="opIdcreate-sni-for-certificate"></a>

`POST /certificates/{certificate_name_or_id}/snis`

*Create a new SNI associated with a Certificate*

Create a new SNI and associate it with a certificate in the system. Use this endpoint to add a new SNI to the system and link it to a specific certificate.

> Body parameter

```json
{
  "name": "my-sni",
  "tags": [
    "[\"user-level\", \"low-priority\"]"
  ],
  "certificate": {
    "id": "91020192-062d-416f-a275-9addeeaffaf2"
  }
}
```

<h3 id="create-sni-for-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|A JSON object containing the details of the new SNI, including the name, certificate, and tags.|
|» name|body|string|true|The SNI name to associate with the given certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the SNIs for grouping and filtering.|
|» certificate|body|object|true|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object. With form-encoded, the notation is `certificate.id=<certificate id>`. With JSON, use `"certificate":{"id":"<certificate id>"}`.|
|»» id|body|string|false|91020192-062d-416f-a275-9addeeaffaf2|
|certificate_name_or_id|path|string|true|The unique identifier or the `name` attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|certificate_name_or_id|a3ad71a8-6685-4b03-a101-980a953544f6|
|certificate_name_or_id|name|

> Example responses

> 201 Response

```json
{
  "certificate": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  },
  "id": "36c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "some.example.org"
}
```

<h3 id="create-sni-for-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created SNI|[SNI](#schemasni)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid SNI|Inline|

<h3 id="create-sni-for-certificate-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-sni-for-certificate

<a id="opIddelete-sni-for-certificate"></a>

`DELETE /certificates/{certificate_id}/snis/{sni_name_or_id}`

*Delete a an SNI associated with a Certificate*

Delete a an SNI associated with a Certificate using ID or name.

<h3 id="delete-sni-for-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|certificate_id|path|string|true|The unique identifier of the Certificate to retrieve.|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

<h3 id="delete-sni-for-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted SNI or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## fetch-sni-with-certificate

<a id="opIdfetch-sni-with-certificate"></a>

`GET /certificates/{certificate_id}/snis/{sni_name_or_id}`

*Fetch an SNI associated with a certificate*

Get an SNI associated with a Certificate using ID or name.

<h3 id="fetch-sni-with-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|certificate_id|path|string|true|The unique identifier of the Certificate to retrieve.|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

> Example responses

> 200 Response

```json
{
  "certificate": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  },
  "id": "36c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "some.example.org"
}
```

<h3 id="fetch-sni-with-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched SNI|[SNI](#schemasni)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-sni-for-certificate

<a id="opIdupdate-sni-for-certificate"></a>

`PATCH /certificates/{certificate_id}/snis/{sni_name_or_id}`

*Update SNI associated to a certificate*

 Update an existing SNI associated with a certificate in the system using the SNI ID or name. The request body should include the fields of the SNI that need to be updated, such as the name, description, or other properties. If the request body contains valid data, the endpoint will update the SNI and return a success response.

> Body parameter

```json
{
  "name": "my-sni",
  "tags": [
    "[\"user-level\", \"low-priority\"]"
  ],
  "certificate": {
    "id": "91020192-062d-416f-a275-9addeeaffaf2"
  }
}
```

<h3 id="update-sni-for-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|A JSON object containing the details of the new SNI, including the name, certificate, and tags.|
|» name|body|string|true|The SNI name to associate with the given certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the SNIs for grouping and filtering.|
|» certificate|body|object|true|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object. With form-encoded, the notation is `certificate.id=<certificate id>`. With JSON, use `"certificate":{"id":"<certificate id>"}`.|
|»» id|body|string|false|91020192-062d-416f-a275-9addeeaffaf2|
|certificate_id|path|string|true|The unique identifier of the Certificate to retrieve.|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

> Example responses

> 200 Response

```json
{
  "certificate": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  },
  "id": "36c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "some.example.org"
}
```

<h3 id="update-sni-for-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated SNI|[SNI](#schemasni)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid SNI|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-sni-for-certificate-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-sni-for-certificate

<a id="opIdupsert-sni-for-certificate"></a>

`PUT /certificates/{certificate_id}/snis/{sni_name_or_id}`

*Upsert an SNI associated with a certificate*

Create or Update an SNI associated with a Certificate using ID or name.

Inserts (or replaces) the SNI under the requested resource with the definition specified in the body. The SNI will be identified via the name or id attribute.

When the name or id attribute has the structure of a UUID, the SNI being inserted/replaced will be identified by its id. Otherwise it will be identified by its name.

When creating a new SNI without specifying id (neither in the URL nor in the body), then it will be auto-generated.

> Body parameter

```json
{
  "name": "my-sni",
  "tags": [
    "[\"user-level\", \"low-priority\"]"
  ],
  "certificate": {
    "id": "91020192-062d-416f-a275-9addeeaffaf2"
  }
}
```

<h3 id="upsert-sni-for-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|A JSON object containing the details of the new SNI, including the name, certificate, and tags.|
|» name|body|string|true|The SNI name to associate with the given certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the SNIs for grouping and filtering.|
|» certificate|body|object|true|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object. With form-encoded, the notation is `certificate.id=<certificate id>`. With JSON, use `"certificate":{"id":"<certificate id>"}`.|
|»» id|body|string|false|91020192-062d-416f-a275-9addeeaffaf2|
|certificate_id|path|string|true|The unique identifier of the Certificate to retrieve.|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

> Example responses

> 200 Response

```json
{
  "certificate": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  },
  "id": "36c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "some.example.org"
}
```

<h3 id="upsert-sni-for-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted SNI|[SNI](#schemasni)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid SNI|Inline|

<h3 id="upsert-sni-for-certificate-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## list-sni

<a id="opIdlist-sni"></a>

`GET /snis`

*List all SNIs*

List all SNIs

<h3 id="list-sni-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "147f5ef0-1ed6-4711-b77f-489262f8bff7",
      "name": "my-sni",
      "created_at": 1422386534,
      "tags": [
        "string"
      ],
      "certificate": {
        "id": "2e013e8-7623-4494-a347-6d29108ff68b"
      }
    }
  ],
  "next": "http://localhost:8001/snis?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="list-sni-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SNI response object|Inline|

<h3 id="list-sni-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|Array of SNIs|
|»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|
|»» name|string|false|none|The SNI name to associate with the given certificate.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|
|»» certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|»»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-sni

<a id="opIdcreate-sni"></a>

`POST /snis`

*Create a new SNI*

Create a new SNI

> Body parameter

```json
{
  "name": "my-sni",
  "tags": [
    "[\"user-level\", \"low-priority\"]"
  ],
  "certificate": {
    "id": "91020192-062d-416f-a275-9addeeaffaf2"
  }
}
```

<h3 id="create-sni-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|A JSON object containing the details of the new SNI, including the name, certificate, and tags.|
|» name|body|string|true|The SNI name to associate with the given certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the SNIs for grouping and filtering.|
|» certificate|body|object|true|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object. With form-encoded, the notation is `certificate.id=<certificate id>`. With JSON, use `"certificate":{"id":"<certificate id>"}`.|
|»» id|body|string|false|91020192-062d-416f-a275-9addeeaffaf2|

> Example responses

> 201 Response

```json
{
  "data": [
    {
      "id": "147f5ef0-1ed6-4711-b77f-489262f8bff7",
      "name": "my-sni",
      "created_at": 1422386534,
      "tags": [
        "string"
      ],
      "certificate": {
        "id": "2e013e8-7623-4494-a347-6d29108ff68b"
      }
    }
  ],
  "next": "http://localhost:8001/snis?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="create-sni-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|SNI response object|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid SNI|Inline|

<h3 id="create-sni-responseschema">Response Schema</h3>

Status Code **201**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|Array of SNIs|
|»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|
|»» name|string|false|none|The SNI name to associate with the given certificate.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|
|»» certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|»»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## delete-sni

<a id="opIddelete-sni"></a>

`DELETE /snis/{sni_name_or_id}`

*Delete an SNI*

Delete an SNI

<h3 id="delete-sni-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

<h3 id="delete-sni-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted SNI or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-sni

<a id="opIdget-sni"></a>

`GET /snis/{sni_name_or_id}`

*Fetch an SNI*

Get an SNI using ID or name.

<h3 id="get-sni-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "147f5ef0-1ed6-4711-b77f-489262f8bff7",
      "name": "my-sni",
      "created_at": 1422386534,
      "tags": [
        "string"
      ],
      "certificate": {
        "id": "2e013e8-7623-4494-a347-6d29108ff68b"
      }
    }
  ],
  "next": "http://localhost:8001/snis?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="get-sni-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SNI response object|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="get-sni-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|Array of SNIs|
|»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|
|»» name|string|false|none|The SNI name to associate with the given certificate.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|
|»» certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|»»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## update-sni

<a id="opIdupdate-sni"></a>

`PATCH /snis/{sni_name_or_id}`

*Update an SNI*

Update an SNI

> Body parameter

```json
{
  "name": "my-sni",
  "tags": [
    "[\"user-level\", \"low-priority\"]"
  ],
  "certificate": {
    "id": "91020192-062d-416f-a275-9addeeaffaf2"
  }
}
```

<h3 id="update-sni-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|A JSON object containing the details of the new SNI, including the name, certificate, and tags.|
|» name|body|string|true|The SNI name to associate with the given certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the SNIs for grouping and filtering.|
|» certificate|body|object|true|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object. With form-encoded, the notation is `certificate.id=<certificate id>`. With JSON, use `"certificate":{"id":"<certificate id>"}`.|
|»» id|body|string|false|91020192-062d-416f-a275-9addeeaffaf2|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "147f5ef0-1ed6-4711-b77f-489262f8bff7",
      "name": "my-sni",
      "created_at": 1422386534,
      "tags": [
        "string"
      ],
      "certificate": {
        "id": "2e013e8-7623-4494-a347-6d29108ff68b"
      }
    }
  ],
  "next": "http://localhost:8001/snis?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="update-sni-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SNI response object|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid SNI|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-sni-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|Array of SNIs|
|»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|
|»» name|string|false|none|The SNI name to associate with the given certificate.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|
|»» certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|»»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## upsert-sni

<a id="opIdupsert-sni"></a>

`PUT /snis/{sni_name_or_id}`

*Update an SNI*

Create or Update SNI using ID or name.

> Body parameter

```json
{
  "name": "my-sni",
  "tags": [
    "[\"user-level\", \"low-priority\"]"
  ],
  "certificate": {
    "id": "91020192-062d-416f-a275-9addeeaffaf2"
  }
}
```

<h3 id="upsert-sni-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|A JSON object containing the details of the new SNI, including the name, certificate, and tags.|
|» name|body|string|true|The SNI name to associate with the given certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the SNIs for grouping and filtering.|
|» certificate|body|object|true|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object. With form-encoded, the notation is `certificate.id=<certificate id>`. With JSON, use `"certificate":{"id":"<certificate id>"}`.|
|»» id|body|string|false|91020192-062d-416f-a275-9addeeaffaf2|
|sni_name_or_id|path|string|true|The unique identifier or the name of the SNI to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "147f5ef0-1ed6-4711-b77f-489262f8bff7",
      "name": "my-sni",
      "created_at": 1422386534,
      "tags": [
        "string"
      ],
      "certificate": {
        "id": "2e013e8-7623-4494-a347-6d29108ff68b"
      }
    }
  ],
  "next": "http://localhost:8001/snis?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="upsert-sni-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SNI response object|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid SNI|Inline|

<h3 id="upsert-sni-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|Array of SNIs|
|»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs are to be retrieved. When using this endpoint, only SNIs associated to the specified Certificate will be listed.|
|»» name|string|false|none|The SNI name to associate with the given certificate.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|
|»» certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|»»» id|string|false|none|The unique identifier or the name attribute of the Certificate whose SNIs|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-ca-certificates">CA Certificates</h1>

A CA certificate object represents a trusted certificate authority. 
These objects are used by Kong Gateway to verify the validity of a client or server certificate.

## list-ca_certificate

<a id="opIdlist-ca_certificate"></a>

`GET /ca_certificates`

*List all CA certificates*

Retrieve a list of all available Certificate Authority (CA) certificates, including the certificate ID, creation date, and other details. You can use query parameters to filter the results by size or tags, for example `/ca-certificates?size=50&tags=enterprise`.

<h3 id="list-ca_certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```
{"cert":"-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----","id":"b2f34145-0343-41a4-9602-4c69dec2f260"}
```

<h3 id="list-ca_certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing CA Certificates|[CA-Certificate](#schemaca-certificate)|

<aside class="success">
This operation does not require authentication
</aside>

## create-ca_certificate

<a id="opIdcreate-ca_certificate"></a>

`POST /ca_certificates`

*Create a new CA certificate*

Create a new Certificate Authority (CA) certificate. The request body must include the `cert` property, the certificate data in PEM format; it can also include `cert_digest`, a digest of the certificate in hex format for verifying the certificates integrity, and `tags`, an optional list of tags to categorize the certificate.

> Body parameter

```json
{
  "cert": "\"-----BEGIN CERTIFICATE-----...\"",
  "cert_digest": "c641e28d77e93544f2fa87b2cf3f3d51...",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-ca_certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|This request body represents a new Certificate Authority (CA) certificate and includes the properties required to create a new certificate.|
|» cert|body|string|true|PEM-encoded public certificate of the CA.|
|» cert_digest|body|string|false|SHA256 hex digest of the public certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the Certificate for grouping and filtering.|

> Example responses

> 201 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f260"
}
```

<h3 id="create-ca_certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|The created certificate object.|[CA-Certificate](#schemaca-certificate)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid CA certificate|None|

<aside class="success">
This operation does not require authentication
</aside>

## delete-ca_certificate

<a id="opIddelete-ca_certificate"></a>

`DELETE /ca_certificates/{ca_certificate_id}`

*Delete a CA Certificate*

Delete the specified Certificate Authority (CA) certificate using the provided ca_certificate_id.

<h3 id="delete-ca_certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ca_certificate_id|path|string|true|ID of the related certificate|

<h3 id="delete-ca_certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted CA Certificate or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-ca_certificate

<a id="opIdget-ca_certificate"></a>

`GET /ca_certificates/{ca_certificate_id}`

*Fetch a CA certificate*

Retrieve details about the specified Certificate Authority (CA) certificate using the provided path parameter `ca_certificate_id`.

<h3 id="get-ca_certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ca_certificate_id|path|string|true|The unique identifier of the certificate to retrieve.|

> Example responses

> 200 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f260"
}
```

<h3 id="get-ca_certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The specified CA certificate exists in the system, and the response includes details about the certificate.|[CA-Certificate](#schemaca-certificate)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-ca_certificate

<a id="opIdupdate-ca_certificate"></a>

`PATCH /ca_certificates/{ca_certificate_id}`

*Update a CA Certificate*

Update the specified Certificate Authority (CA) certificate using the provided ca_certificate_id. Use this endpoint to modify an existing CA certificate in the system. The request body should include the fields of the CA certificate that need to be updated.

> This API is not available in DB-less mode.

> Body parameter

```json
{
  "cert": "\"-----BEGIN CERTIFICATE-----...\"",
  "cert_digest": "c641e28d77e93544f2fa87b2cf3f3d51...",
  "tags": [
    "string"
  ]
}
```

<h3 id="update-ca_certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|This request body represents a new Certificate Authority (CA) certificate and includes the properties required to create a new certificate.|
|» cert|body|string|true|PEM-encoded public certificate of the CA.|
|» cert_digest|body|string|false|SHA256 hex digest of the public certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the Certificate for grouping and filtering.|
|ca_certificate_id|path|string|true|ID of the related certificate|

> Example responses

> 200 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f260"
}
```

<h3 id="update-ca_certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated CA Certificate|[CA-Certificate](#schemaca-certificate)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid CA Certificate|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-ca_certificate-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## updatet-ca_certificate

<a id="opIdupdatet-ca_certificate"></a>

`PUT /ca_certificates/{ca_certificate_id}`

*Update a CA Certificate*

Create or Update a CA Certificate using the provided path parameter `ca_certificate_id`.

> Body parameter

```json
{
  "cert": "\"-----BEGIN CERTIFICATE-----...\"",
  "cert_digest": "c641e28d77e93544f2fa87b2cf3f3d51...",
  "tags": [
    "string"
  ]
}
```

<h3 id="updatet-ca_certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|This request body represents a new Certificate Authority (CA) certificate and includes the properties required to create a new certificate.|
|» cert|body|string|true|PEM-encoded public certificate of the CA.|
|» cert_digest|body|string|false|SHA256 hex digest of the public certificate.|
|» tags|body|[string]|false|An optional set of strings associated with the Certificate for grouping and filtering.|
|ca_certificate_id|path|string|true|ID of the related certificate|

> Example responses

> 200 Response

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f260"
}
```

<h3 id="updatet-ca_certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted CA Certificate|[CA-Certificate](#schemaca-certificate)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid CA Certificate|Inline|

<h3 id="updatet-ca_certificate-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-upstreams">Upstreams</h1>

The upstream object represents a virtual hostname and can be used to load balance incoming requests over multiple services (targets). 
<br><br>
An upstream also includes a [health checker](https://docs.konghq.com/gateway/latest/how-kong-works/health-checks/), which can enable and disable targets based on their ability or inability to serve requests. 
The configuration for the health checker is stored in the upstream object, and applies to all of its targets.

## list-upstream

<a id="opIdlist-upstream"></a>

`GET /upstreams`

*List all Upstreams*

List all registered upstreams. You can filter the results by pagination size, offset, or tags like `/upstreams?size=10&offset=0`.

<h3 id="list-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> A successful response listing Upstreams

```json
{
  "data": [
    {
      "algorithm": "round-robin",
      "hash_fallback": "none",
      "hash_on": "none",
      "hash_on_cookie_path": "/",
      "healthchecks": {
        "active": {
          "concurrency": 10,
          "healthy": {
            "http_statuses": [
              200,
              302
            ],
            "interval": 0,
            "successes": 0
          },
          "http_path": "/",
          "https_verify_certificate": true,
          "timeout": 1,
          "type": "http",
          "unhealthy": {
            "http_failures": 0,
            "http_statuses": [
              429,
              404,
              500,
              501,
              502,
              503,
              504,
              505
            ],
            "interval": 0,
            "tcp_failures": 0,
            "timeouts": 0
          }
        },
        "passive": {
          "healthy": {
            "http_statuses": [
              200,
              201,
              202,
              203,
              204,
              205,
              206,
              207,
              208,
              226,
              300,
              301,
              302,
              303,
              304,
              305,
              306,
              307,
              308
            ],
            "successes": 0
          },
          "type": "http",
          "unhealthy": {
            "http_failures": 0,
            "http_statuses": [
              429,
              500,
              503
            ],
            "tcp_failures": 0,
            "timeouts": 0
          }
        },
        "threshold": 0
      },
      "id": "6eed5e9c-5398-4026-9a4c-d48f18a2431e",
      "name": "api.example.internal",
      "slots": 10000
    }
  ],
  "offset": "string"
}
```

<h3 id="list-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Upstreams|Inline|

<h3 id="list-upstream-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Upstream](#schemaupstream)]|false|none|[The upstream object represents a virtual hostname and can be used to loadbalance incoming requests over multiple services (targets). So for example an upstream named `service.v1.xyz` for a service object whose `host` is `service.v1.xyz`. Requests for this service would be proxied to the targets defined within the upstream. An upstream also includes a health check, which is able to enable and disable targets based on their ability or inability to serve requests. The configuration for the health checker is stored in the upstream object, and applies to all of its targets.]|
|»» algorithm|string|false|none|Which load balancing algorithm to use.|
|»» client_certificate|object|false|none|If set, the certificate to be used as client certificate while TLS handshaking to the upstream server.|
|»»» id|string|false|none|none|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» hash_fallback|string|false|none|What to use as hashing input if the primary `hash_on` does not return a hash (eg. header is missing, or no Consumer identified). Not available if `hash_on` is set to `cookie`.|
|»» hash_fallback_header|string|false|none|The header name to take the value from as hash input. Only required when `hash_fallback` is set to `header`.|
|»» hash_fallback_query_arg|string|false|none|The name of the query string argument to take the value from as hash input. Only required when `hash_fallback` is set to `query_arg`.|
|»» hash_fallback_uri_capture|string|false|none|The name of the route URI capture to take the value from as hash input. Only required when `hash_fallback` is set to `uri_capture`.|
|»» hash_on|string|false|none|What to use as hashing input. Using `none` results in a weighted-round-robin scheme with no hashing.|
|»» hash_on_cookie|string|false|none|The cookie name to take the value from as hash input. Only required when `hash_on` or `hash_fallback` is set to `cookie`. If the specified cookie is not in the request, Kong will generate a value and set the cookie in the response.|
|»» hash_on_cookie_path|string|false|none|The cookie path to set in the response headers. Only required when `hash_on` or `hash_fallback` is set to `cookie`.|
|»» hash_on_header|string|false|none|The header name to take the value from as hash input. Only required when `hash_on` is set to `header`.|
|»» hash_on_query_arg|string|false|none|The name of the query string argument to take the value from as hash input. Only required when `hash_on` is set to `query_arg`.|
|»» hash_on_uri_capture|string|false|none|The name of the route URI capture to take the value from as hash input. Only required when `hash_on` is set to `uri_capture`.|
|»» healthchecks|object|false|none|none|
|»»» active|object|false|none|none|
|»»»» concurrency|integer|false|none|none|
|»»»» headers|object|false|none|none|
|»»»» healthy|object|false|none|none|
|»»»»» http_statuses|[integer]|false|none|none|
|»»»»» interval|number|false|none|none|
|»»»»» successes|integer|false|none|none|
|»»»» http_path|string|false|none|none|
|»»»» https_sni|string|false|none|none|
|»»»» https_verify_certificate|boolean|false|none|none|
|»»»» timeout|number|false|none|none|
|»»»» type|string|false|none|none|
|»»»» unhealthy|object|false|none|none|
|»»»»» http_failures|integer|false|none|none|
|»»»»» http_statuses|[integer]|false|none|none|
|»»»»» interval|number|false|none|none|
|»»»»» tcp_failures|integer|false|none|none|
|»»»»» timeouts|integer|false|none|none|
|»»» passive|object|false|none|none|
|»»»» healthy|object|false|none|none|
|»»»»» http_statuses|[integer]|false|none|none|
|»»»»» successes|integer|false|none|none|
|»»»» type|string|false|none|none|
|»»»» unhealthy|object|false|none|none|
|»»»»» http_failures|integer|false|none|none|
|»»»»» http_statuses|[integer]|false|none|none|
|»»»»» tcp_failures|integer|false|none|none|
|»»»»» timeouts|integer|false|none|none|
|»»» threshold|number|false|none|none|
|»» host_header|string|false|none|The hostname to be used as `Host` header when proxying requests through Kong.|
|»» id|string|false|none|none|
|»» name|string|false|none|This is a hostname, which must be equal to the `host` of a service.|
|»» slots|integer|false|none|The number of slots in the load balancer algorithm. If `algorithm` is set to `round-robin`, this setting determines the maximum number of slots. If `algorithm` is set to `consistent-hashing`, this setting determines the actual number of slots in the algorithm. Accepts an integer in the range `10`-`65536`.|
|»» tags|[string]|false|none|An optional set of strings associated with the Upstream for grouping and filtering.|
|»» use_srv_name|boolean|false|none|If set, the balancer will use SRV hostname(if DNS Answer has SRV record) as the proxy upstream `Host`.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-upstream

<a id="opIdcreate-upstream"></a>

`POST /upstreams`

*Create a new Upstream*

Create a new Upstream

> Body parameter

```json
{
  "name": "my-upstream",
  "algorithm": "round-robin",
  "hash_on": "none",
  "hash_fallback": "none",
  "hash_on_cookie_path": "/",
  "slots": 10000,
  "healthchecks": {
    "passive": {
      "type": "http",
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "unhealthy": {
        "http_statuses": [
          429,
          500,
          503
        ],
        "timeouts": 0,
        "http_failures": 0,
        "tcp_failures": 0
      }
    },
    "active": {
      "https_verify_certificate": true,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "successes": 0,
        "interval": 0
      },
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "timeouts": 0,
        "tcp_failures": 0,
        "interval": 0
      },
      "type": "http",
      "concurrency": 10,
      "headers": {
        "type": "object",
        "properties": {
          "x-my-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-my-header header."
          },
          "x-another-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-another-header header."
          }
        }
      },
      "timeout": 1,
      "http_path": "/",
      "https_sni": "example.com"
    },
    "threshold": 0
  },
  "tags": [
    "user-level",
    "low-priority"
  ],
  "host_header": "example.com",
  "client_certificate": {
    "id": "ea29aaa3-3b2d-488c-b90c-56df8e0dd8c6"
  },
  "use_srv_name": false
}
```

<h3 id="create-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|true|This is a hostname, which must be equal to the `host` of a service.|
|» algorithm|body|string|false|Which load balancing algorithm to use. Accepted values are: `"consistent-hashing"`, `"least-connections"`,` "round-robin"`. Default: `"round-robin"`.|
|» hash_on|body|string|false|What to use as hashing input. Using none results in a weighted-round-robin scheme with no hashing|
|» hash_fallback|body|string|false|What to use as hashing input if the primary hash_on does not return a hash (eg. header is missing, or no Consumer identified). Not available if hash_on is set to cookie.|
|» hash_on_header|body|string|false|The header name to take the value from as hash input. Only required when `hash_on` is set to header.|
|» hash_fallback_header|body|string|false|The header name to take the value from as hash input. Only required when hash_fallback is set to header.|
|» hash_on_cookie|body|string|false|The cookie name to take the value from as hash input. Only required when `hash_on` or `hash_fallback` is set to `cookie`. If the specified cookie is not in the request, Kong will generate a value and set the cookie in the response.|
|» hash_on_cookie_path|body|string|false|The cookie path to set in the response headers. Only required when `hash_on` or `hash_fallback` is set to `cookie`.|
|» hash_on_query_arg|body|string|false|The name of the query string argument to take the value from as hash input. Only required when `hash_on` is set to `query_arg`.|
|» hash_fallback_query_arg|body|string|false|The name of the query string argument to take the value from as hash input. Only required when `hash_fallback` is set to `query_arg`.|
|» hash_on_uri_capture|body|string|false|The name of the route URI capture to take the value from as hash input. Only required when `hash_on` is set to `uri_capture`.|
|» hash_fallback_uri_capture|body|string|false|The name of the route URI capture to take the value from as hash input. Only required when `hash_fallback` is set to `uri_capture`.|
|» slots|body|integer|false|The number of slots in the load balancer algorithm. If the algorithm is set to `round-robin`, this setting determines the maximum number of slots. If the algorithm is set to `consistent-hashing`, this setting determines the actual number of slots in the algorithm. Accepts an integer in the range 10-65536.|
|» healthchecks|body|object|false|none|
|»» passive|body|object|false|none|
|»»» type|body|string|false|Whether to perform passive health checks interpreting HTTP/HTTPS statuses, or just check for TCP connection success. In passive checks, http and https options are equivalent. Accepted values are `tcp`, `http`, `https`, `grpc`, `grpcs`.|
|»»» healthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses which represent healthiness when produced by proxied traffic, as observed by passive health checks.  With form-encoded, the notation is `http_statuses[]=200&http_statuses[]=201`. With JSON, use an array.|
|»»»» successes|body|integer|false|Number of successes in proxied traffic (as defined by `healthchecks.passive.healthy.http_statuses`) to consider a target healthy, as observed by passive health checks.|
|»»» unhealthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses which represent unhealthiness when produced by proxied traffic, as observed by passive health checks. With form-encoded, the notation is `http_statuses[]=429&http_statuses[]=500`. With JSON, use an array.|
|»»»» timeouts|body|integer|false|Number of timeouts in proxied traffic to consider a target unhealthy, as observed by passive health checks.|
|»»»» http_failures|body|integer|false|Number of HTTP failures in proxied traffic (as defined by `healthchecks.passive.unhealthy.http_statuses`) to consider a target unhealthy, as observed by passive health checks.|
|»»»» tcp_failures|body|integer|false|Number of TCP connection failures to consider a target unhealthy, as observed by passive health checks.|
|»» active|body|object|false|none|
|»»» https_verify_certificate|body|boolean|false|none|
|»»» healthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses to consider a success, indicating healthiness, when returned by a probe in active health checks. With form-encoded, the notation is `http_statuses[]=200&http_statuses[]=302`. With JSON, use an array.|
|»»»» successes|body|integer|false|Number of successes in active probes (as defined by `healthchecks.active.healthy.http_statuses`) to consider a target healthy.|
|»»»» interval|body|integer|false|Interval between active health checks for healthy targets (in seconds). A value of zero indicates that active probes for healthy targets should not be performed.|
|»»» unhealthy|body|object|false|none|
|»»»» http_failures|body|integer|false|Number of HTTP failures in active probes (as defined by `healthchecks.active.unhealthy.http_statuses`) to consider a target unhealthy.|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses to consider a failure, indicating unhealthiness, when returned by a probe in active health checks. With form-encoded, the notation is `http_statuses[]=429&http_statuses[]=404`. With JSON, use an array.|
|»»»» timeouts|body|integer|false|Number of timeouts in active probes to consider a target unhealthy.|
|»»»» tcp_failures|body|integer|false|Number of TCP failures in active probes to consider a target unhealthy.|
|»»»» interval|body|integer|false|Interval between active health checks for unhealthy targets (in seconds). A value of zero indicates that active probes for unhealthy targets should not be performed.|
|»»» type|body|string|false|Whether to perform active health checks using HTTP or HTTPS, or just attempt a TCP connection.|
|»»» concurrency|body|integer|false|Number of targets to check concurrently in active health checks.|
|»»» headers|body|object|false|One or more lists of values indexed by header name to use in GET HTTP request to run as a probe on active health checks. Values must be pre-formatted.|
|»»» timeout|body|integer|false|Socket timeout for active health checks (in seconds).|
|»»» http_path|body|string|false|Path to use in GET HTTP request to run as a probe on active health checks.|
|»»» https_sni|body|string|false|The hostname to use as an SNI (Server Name Identification) when performing active health checks using HTTPS. This is particularly useful when Targets are configured using IPs, so that the target host's certificate can be verified with the proper SNI.|
|»» threshold|body|integer|false|The minimum percentage of the upstream's targets' weight that must be available for the whole upstream to be considered healthy.|
|» tags|body|[string]|false|An optional set of strings associated with the Upstream for grouping and filtering.|
|» host_header|body|string|false|The hostname to be used as Host header when proxying requests through Kong.|
|» client_certificate|body|object|false|If set, the certificate to be used as client certificate while TLS handshaking to the upstream server.|
|»» id|body|string|false|none|
|» use_srv_name|body|boolean|false|If set, the balancer will use SRV hostname(if DNS Answer has SRV record) as the proxy upstream Host.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» algorithm|consistent-hashing|
|» algorithm|least-connections|
|» algorithm|round-robin|
|» algorithm|latency|
|» hash_on|none|
|» hash_on|consumer|
|» hash_on|ip|
|» hash_on|cookie|
|» hash_on|uri_capture|
|» hash_on|path|
|» hash_on|query_arg|
|» hash_fallback|none|
|» hash_fallback|consumer|
|» hash_fallback|ip|
|» hash_fallback|cookie|
|» hash_fallback|uri_capture|
|» hash_fallback|path|
|» hash_fallback|query_arg|
|»»» type|tcp|
|»»» type|http|
|»»» type|https|
|»»» type|grpc|
|»»» type|grpcs|
|»»»» http_statuses|200|
|»»»» http_statuses|201|
|»»»» http_statuses|202|
|»»»» http_statuses|203|
|»»»» http_statuses|204|
|»»»» http_statuses|205|
|»»»» http_statuses|206|
|»»»» http_statuses|207|
|»»»» http_statuses|208|
|»»»» http_statuses|226|
|»»»» http_statuses|300|
|»»»» http_statuses|301|
|»»»» http_statuses|302|
|»»»» http_statuses|303|
|»»»» http_statuses|304|
|»»»» http_statuses|305|
|»»»» http_statuses|306|
|»»»» http_statuses|307|
|»»»» http_statuses|308|
|»»»» http_statuses|429|
|»»»» http_statuses|500|
|»»»» http_statuses|503|
|»»» type|tcp|
|»»» type|http|
|»»» type|https|
|»»» type|grpc|
|»»» type|grpcs|

> Example responses

> 201 Response

```json
{
  "algorithm": "round-robin",
  "hash_fallback": "none",
  "hash_on": "none",
  "hash_on_cookie_path": "/",
  "healthchecks": {
    "active": {
      "concurrency": 10,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "interval": 0,
        "successes": 0
      },
      "http_path": "/",
      "https_verify_certificate": true,
      "timeout": 1,
      "type": "http",
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "interval": 0,
        "tcp_failures": 0,
        "timeouts": 0
      }
    },
    "passive": {
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "type": "http",
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          500,
          503
        ],
        "tcp_failures": 0,
        "timeouts": 0
      }
    },
    "threshold": 0
  },
  "id": "6eed5e9c-5398-4026-9a4c-d48f18a2431e",
  "name": "api.example.internal",
  "slots": 10000
}
```

<h3 id="create-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Upstream|[Upstream](#schemaupstream)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Upstream|Inline|

<h3 id="create-upstream-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-upstream

<a id="opIddelete-upstream"></a>

`DELETE /upstreams/{upstream_id_or_name}`

*Delete an Upstream*

Delete an Upstream

<h3 id="delete-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|

<h3 id="delete-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Upstream or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-upstream

<a id="opIdget-upstream"></a>

`GET /upstreams/{upstream_id_or_name}`

*Fetch an Upstream*

Get an Upstream using ID or name.

<h3 id="get-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|

> Example responses

> 200 Response

```json
{
  "algorithm": "round-robin",
  "hash_fallback": "none",
  "hash_on": "none",
  "hash_on_cookie_path": "/",
  "healthchecks": {
    "active": {
      "concurrency": 10,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "interval": 0,
        "successes": 0
      },
      "http_path": "/",
      "https_verify_certificate": true,
      "timeout": 1,
      "type": "http",
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "interval": 0,
        "tcp_failures": 0,
        "timeouts": 0
      }
    },
    "passive": {
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "type": "http",
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          500,
          503
        ],
        "tcp_failures": 0,
        "timeouts": 0
      }
    },
    "threshold": 0
  },
  "id": "6eed5e9c-5398-4026-9a4c-d48f18a2431e",
  "name": "api.example.internal",
  "slots": 10000
}
```

<h3 id="get-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Upstream|[Upstream](#schemaupstream)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-upstream

<a id="opIdupdate-upstream"></a>

`PATCH /upstreams/{upstream_id_or_name}`

*Update an Upstream*

Update an Upstream

> Body parameter

```json
{
  "name": "my-upstream",
  "algorithm": "round-robin",
  "hash_on": "none",
  "hash_fallback": "none",
  "hash_on_cookie_path": "/",
  "slots": 10000,
  "healthchecks": {
    "passive": {
      "type": "http",
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "unhealthy": {
        "http_statuses": [
          429,
          500,
          503
        ],
        "timeouts": 0,
        "http_failures": 0,
        "tcp_failures": 0
      }
    },
    "active": {
      "https_verify_certificate": true,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "successes": 0,
        "interval": 0
      },
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "timeouts": 0,
        "tcp_failures": 0,
        "interval": 0
      },
      "type": "http",
      "concurrency": 10,
      "headers": {
        "type": "object",
        "properties": {
          "x-my-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-my-header header."
          },
          "x-another-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-another-header header."
          }
        }
      },
      "timeout": 1,
      "http_path": "/",
      "https_sni": "example.com"
    },
    "threshold": 0
  },
  "tags": [
    "user-level",
    "low-priority"
  ],
  "host_header": "example.com",
  "client_certificate": {
    "id": "ea29aaa3-3b2d-488c-b90c-56df8e0dd8c6"
  },
  "use_srv_name": false
}
```

<h3 id="update-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|true|This is a hostname, which must be equal to the `host` of a service.|
|» algorithm|body|string|false|Which load balancing algorithm to use. Accepted values are: `"consistent-hashing"`, `"least-connections"`,` "round-robin"`. Default: `"round-robin"`.|
|» hash_on|body|string|false|What to use as hashing input. Using none results in a weighted-round-robin scheme with no hashing|
|» hash_fallback|body|string|false|What to use as hashing input if the primary hash_on does not return a hash (eg. header is missing, or no Consumer identified). Not available if hash_on is set to cookie.|
|» hash_on_header|body|string|false|The header name to take the value from as hash input. Only required when `hash_on` is set to header.|
|» hash_fallback_header|body|string|false|The header name to take the value from as hash input. Only required when hash_fallback is set to header.|
|» hash_on_cookie|body|string|false|The cookie name to take the value from as hash input. Only required when `hash_on` or `hash_fallback` is set to `cookie`. If the specified cookie is not in the request, Kong will generate a value and set the cookie in the response.|
|» hash_on_cookie_path|body|string|false|The cookie path to set in the response headers. Only required when `hash_on` or `hash_fallback` is set to `cookie`.|
|» hash_on_query_arg|body|string|false|The name of the query string argument to take the value from as hash input. Only required when `hash_on` is set to `query_arg`.|
|» hash_fallback_query_arg|body|string|false|The name of the query string argument to take the value from as hash input. Only required when `hash_fallback` is set to `query_arg`.|
|» hash_on_uri_capture|body|string|false|The name of the route URI capture to take the value from as hash input. Only required when `hash_on` is set to `uri_capture`.|
|» hash_fallback_uri_capture|body|string|false|The name of the route URI capture to take the value from as hash input. Only required when `hash_fallback` is set to `uri_capture`.|
|» slots|body|integer|false|The number of slots in the load balancer algorithm. If the algorithm is set to `round-robin`, this setting determines the maximum number of slots. If the algorithm is set to `consistent-hashing`, this setting determines the actual number of slots in the algorithm. Accepts an integer in the range 10-65536.|
|» healthchecks|body|object|false|none|
|»» passive|body|object|false|none|
|»»» type|body|string|false|Whether to perform passive health checks interpreting HTTP/HTTPS statuses, or just check for TCP connection success. In passive checks, http and https options are equivalent. Accepted values are `tcp`, `http`, `https`, `grpc`, `grpcs`.|
|»»» healthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses which represent healthiness when produced by proxied traffic, as observed by passive health checks.  With form-encoded, the notation is `http_statuses[]=200&http_statuses[]=201`. With JSON, use an array.|
|»»»» successes|body|integer|false|Number of successes in proxied traffic (as defined by `healthchecks.passive.healthy.http_statuses`) to consider a target healthy, as observed by passive health checks.|
|»»» unhealthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses which represent unhealthiness when produced by proxied traffic, as observed by passive health checks. With form-encoded, the notation is `http_statuses[]=429&http_statuses[]=500`. With JSON, use an array.|
|»»»» timeouts|body|integer|false|Number of timeouts in proxied traffic to consider a target unhealthy, as observed by passive health checks.|
|»»»» http_failures|body|integer|false|Number of HTTP failures in proxied traffic (as defined by `healthchecks.passive.unhealthy.http_statuses`) to consider a target unhealthy, as observed by passive health checks.|
|»»»» tcp_failures|body|integer|false|Number of TCP connection failures to consider a target unhealthy, as observed by passive health checks.|
|»» active|body|object|false|none|
|»»» https_verify_certificate|body|boolean|false|none|
|»»» healthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses to consider a success, indicating healthiness, when returned by a probe in active health checks. With form-encoded, the notation is `http_statuses[]=200&http_statuses[]=302`. With JSON, use an array.|
|»»»» successes|body|integer|false|Number of successes in active probes (as defined by `healthchecks.active.healthy.http_statuses`) to consider a target healthy.|
|»»»» interval|body|integer|false|Interval between active health checks for healthy targets (in seconds). A value of zero indicates that active probes for healthy targets should not be performed.|
|»»» unhealthy|body|object|false|none|
|»»»» http_failures|body|integer|false|Number of HTTP failures in active probes (as defined by `healthchecks.active.unhealthy.http_statuses`) to consider a target unhealthy.|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses to consider a failure, indicating unhealthiness, when returned by a probe in active health checks. With form-encoded, the notation is `http_statuses[]=429&http_statuses[]=404`. With JSON, use an array.|
|»»»» timeouts|body|integer|false|Number of timeouts in active probes to consider a target unhealthy.|
|»»»» tcp_failures|body|integer|false|Number of TCP failures in active probes to consider a target unhealthy.|
|»»»» interval|body|integer|false|Interval between active health checks for unhealthy targets (in seconds). A value of zero indicates that active probes for unhealthy targets should not be performed.|
|»»» type|body|string|false|Whether to perform active health checks using HTTP or HTTPS, or just attempt a TCP connection.|
|»»» concurrency|body|integer|false|Number of targets to check concurrently in active health checks.|
|»»» headers|body|object|false|One or more lists of values indexed by header name to use in GET HTTP request to run as a probe on active health checks. Values must be pre-formatted.|
|»»» timeout|body|integer|false|Socket timeout for active health checks (in seconds).|
|»»» http_path|body|string|false|Path to use in GET HTTP request to run as a probe on active health checks.|
|»»» https_sni|body|string|false|The hostname to use as an SNI (Server Name Identification) when performing active health checks using HTTPS. This is particularly useful when Targets are configured using IPs, so that the target host's certificate can be verified with the proper SNI.|
|»» threshold|body|integer|false|The minimum percentage of the upstream's targets' weight that must be available for the whole upstream to be considered healthy.|
|» tags|body|[string]|false|An optional set of strings associated with the Upstream for grouping and filtering.|
|» host_header|body|string|false|The hostname to be used as Host header when proxying requests through Kong.|
|» client_certificate|body|object|false|If set, the certificate to be used as client certificate while TLS handshaking to the upstream server.|
|»» id|body|string|false|none|
|» use_srv_name|body|boolean|false|If set, the balancer will use SRV hostname(if DNS Answer has SRV record) as the proxy upstream Host.|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» algorithm|consistent-hashing|
|» algorithm|least-connections|
|» algorithm|round-robin|
|» algorithm|latency|
|» hash_on|none|
|» hash_on|consumer|
|» hash_on|ip|
|» hash_on|cookie|
|» hash_on|uri_capture|
|» hash_on|path|
|» hash_on|query_arg|
|» hash_fallback|none|
|» hash_fallback|consumer|
|» hash_fallback|ip|
|» hash_fallback|cookie|
|» hash_fallback|uri_capture|
|» hash_fallback|path|
|» hash_fallback|query_arg|
|»»» type|tcp|
|»»» type|http|
|»»» type|https|
|»»» type|grpc|
|»»» type|grpcs|
|»»»» http_statuses|200|
|»»»» http_statuses|201|
|»»»» http_statuses|202|
|»»»» http_statuses|203|
|»»»» http_statuses|204|
|»»»» http_statuses|205|
|»»»» http_statuses|206|
|»»»» http_statuses|207|
|»»»» http_statuses|208|
|»»»» http_statuses|226|
|»»»» http_statuses|300|
|»»»» http_statuses|301|
|»»»» http_statuses|302|
|»»»» http_statuses|303|
|»»»» http_statuses|304|
|»»»» http_statuses|305|
|»»»» http_statuses|306|
|»»»» http_statuses|307|
|»»»» http_statuses|308|
|»»»» http_statuses|429|
|»»»» http_statuses|500|
|»»»» http_statuses|503|
|»»» type|tcp|
|»»» type|http|
|»»» type|https|
|»»» type|grpc|
|»»» type|grpcs|

> Example responses

> Successfully updated Upstream

```json
{
  "id": "58c8ccbb-eafb-4566-991f-2ed4f678fa70",
  "created_at": 1422386534,
  "name": "my-upstream",
  "algorithm": "round-robin",
  "hash_on": "none",
  "hash_fallback": "none",
  "hash_on_cookie_path": "/",
  "slots": 10000,
  "healthchecks": {
    "passive": {
      "type": "http",
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "unhealthy": {
        "http_statuses": [
          429,
          500,
          503
        ],
        "timeouts": 0,
        "http_failures": 0,
        "tcp_failures": 0
      }
    },
    "active": {
      "https_verify_certificate": true,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "successes": 0,
        "interval": 0
      },
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "timeouts": 0,
        "tcp_failures": 0,
        "interval": 0
      },
      "type": "http",
      "concurrency": 10,
      "headers": {
        "x-my-header": [
          "foo",
          "bar"
        ],
        "x-another-header": [
          "bla"
        ]
      },
      "timeout": 1,
      "http_path": "/",
      "https_sni": "example.com"
    },
    "threshold": 0
  },
  "tags": [
    "user-level",
    "low-priority"
  ],
  "host_header": "example.com",
  "client_certificate": {
    "id": "ea29aaa3-3b2d-488c-b90c-56df8e0dd8c6"
  },
  "use_srv_name": false
}
```

> 400 Response

```json
{}
```

<h3 id="update-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Upstream|[Upstream](#schemaupstream)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Upstream|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-upstream-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-upstream

<a id="opIdupsert-upstream"></a>

`PUT /upstreams/{upstream_id_or_name}`

*Update an Upstream*

Create or Update Upstream using ID or name.

> Body parameter

```json
{
  "name": "my-upstream",
  "algorithm": "round-robin",
  "hash_on": "none",
  "hash_fallback": "none",
  "hash_on_cookie_path": "/",
  "slots": 10000,
  "healthchecks": {
    "passive": {
      "type": "http",
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "unhealthy": {
        "http_statuses": [
          429,
          500,
          503
        ],
        "timeouts": 0,
        "http_failures": 0,
        "tcp_failures": 0
      }
    },
    "active": {
      "https_verify_certificate": true,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "successes": 0,
        "interval": 0
      },
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "timeouts": 0,
        "tcp_failures": 0,
        "interval": 0
      },
      "type": "http",
      "concurrency": 10,
      "headers": {
        "type": "object",
        "properties": {
          "x-my-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-my-header header."
          },
          "x-another-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-another-header header."
          }
        }
      },
      "timeout": 1,
      "http_path": "/",
      "https_sni": "example.com"
    },
    "threshold": 0
  },
  "tags": [
    "user-level",
    "low-priority"
  ],
  "host_header": "example.com",
  "client_certificate": {
    "id": "ea29aaa3-3b2d-488c-b90c-56df8e0dd8c6"
  },
  "use_srv_name": false
}
```

<h3 id="upsert-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|true|This is a hostname, which must be equal to the `host` of a service.|
|» algorithm|body|string|false|Which load balancing algorithm to use. Accepted values are: `"consistent-hashing"`, `"least-connections"`,` "round-robin"`. Default: `"round-robin"`.|
|» hash_on|body|string|false|What to use as hashing input. Using none results in a weighted-round-robin scheme with no hashing|
|» hash_fallback|body|string|false|What to use as hashing input if the primary hash_on does not return a hash (eg. header is missing, or no Consumer identified). Not available if hash_on is set to cookie.|
|» hash_on_header|body|string|false|The header name to take the value from as hash input. Only required when `hash_on` is set to header.|
|» hash_fallback_header|body|string|false|The header name to take the value from as hash input. Only required when hash_fallback is set to header.|
|» hash_on_cookie|body|string|false|The cookie name to take the value from as hash input. Only required when `hash_on` or `hash_fallback` is set to `cookie`. If the specified cookie is not in the request, Kong will generate a value and set the cookie in the response.|
|» hash_on_cookie_path|body|string|false|The cookie path to set in the response headers. Only required when `hash_on` or `hash_fallback` is set to `cookie`.|
|» hash_on_query_arg|body|string|false|The name of the query string argument to take the value from as hash input. Only required when `hash_on` is set to `query_arg`.|
|» hash_fallback_query_arg|body|string|false|The name of the query string argument to take the value from as hash input. Only required when `hash_fallback` is set to `query_arg`.|
|» hash_on_uri_capture|body|string|false|The name of the route URI capture to take the value from as hash input. Only required when `hash_on` is set to `uri_capture`.|
|» hash_fallback_uri_capture|body|string|false|The name of the route URI capture to take the value from as hash input. Only required when `hash_fallback` is set to `uri_capture`.|
|» slots|body|integer|false|The number of slots in the load balancer algorithm. If the algorithm is set to `round-robin`, this setting determines the maximum number of slots. If the algorithm is set to `consistent-hashing`, this setting determines the actual number of slots in the algorithm. Accepts an integer in the range 10-65536.|
|» healthchecks|body|object|false|none|
|»» passive|body|object|false|none|
|»»» type|body|string|false|Whether to perform passive health checks interpreting HTTP/HTTPS statuses, or just check for TCP connection success. In passive checks, http and https options are equivalent. Accepted values are `tcp`, `http`, `https`, `grpc`, `grpcs`.|
|»»» healthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses which represent healthiness when produced by proxied traffic, as observed by passive health checks.  With form-encoded, the notation is `http_statuses[]=200&http_statuses[]=201`. With JSON, use an array.|
|»»»» successes|body|integer|false|Number of successes in proxied traffic (as defined by `healthchecks.passive.healthy.http_statuses`) to consider a target healthy, as observed by passive health checks.|
|»»» unhealthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses which represent unhealthiness when produced by proxied traffic, as observed by passive health checks. With form-encoded, the notation is `http_statuses[]=429&http_statuses[]=500`. With JSON, use an array.|
|»»»» timeouts|body|integer|false|Number of timeouts in proxied traffic to consider a target unhealthy, as observed by passive health checks.|
|»»»» http_failures|body|integer|false|Number of HTTP failures in proxied traffic (as defined by `healthchecks.passive.unhealthy.http_statuses`) to consider a target unhealthy, as observed by passive health checks.|
|»»»» tcp_failures|body|integer|false|Number of TCP connection failures to consider a target unhealthy, as observed by passive health checks.|
|»» active|body|object|false|none|
|»»» https_verify_certificate|body|boolean|false|none|
|»»» healthy|body|object|false|none|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses to consider a success, indicating healthiness, when returned by a probe in active health checks. With form-encoded, the notation is `http_statuses[]=200&http_statuses[]=302`. With JSON, use an array.|
|»»»» successes|body|integer|false|Number of successes in active probes (as defined by `healthchecks.active.healthy.http_statuses`) to consider a target healthy.|
|»»»» interval|body|integer|false|Interval between active health checks for healthy targets (in seconds). A value of zero indicates that active probes for healthy targets should not be performed.|
|»»» unhealthy|body|object|false|none|
|»»»» http_failures|body|integer|false|Number of HTTP failures in active probes (as defined by `healthchecks.active.unhealthy.http_statuses`) to consider a target unhealthy.|
|»»»» http_statuses|body|[integer]|false|An array of HTTP statuses to consider a failure, indicating unhealthiness, when returned by a probe in active health checks. With form-encoded, the notation is `http_statuses[]=429&http_statuses[]=404`. With JSON, use an array.|
|»»»» timeouts|body|integer|false|Number of timeouts in active probes to consider a target unhealthy.|
|»»»» tcp_failures|body|integer|false|Number of TCP failures in active probes to consider a target unhealthy.|
|»»»» interval|body|integer|false|Interval between active health checks for unhealthy targets (in seconds). A value of zero indicates that active probes for unhealthy targets should not be performed.|
|»»» type|body|string|false|Whether to perform active health checks using HTTP or HTTPS, or just attempt a TCP connection.|
|»»» concurrency|body|integer|false|Number of targets to check concurrently in active health checks.|
|»»» headers|body|object|false|One or more lists of values indexed by header name to use in GET HTTP request to run as a probe on active health checks. Values must be pre-formatted.|
|»»» timeout|body|integer|false|Socket timeout for active health checks (in seconds).|
|»»» http_path|body|string|false|Path to use in GET HTTP request to run as a probe on active health checks.|
|»»» https_sni|body|string|false|The hostname to use as an SNI (Server Name Identification) when performing active health checks using HTTPS. This is particularly useful when Targets are configured using IPs, so that the target host's certificate can be verified with the proper SNI.|
|»» threshold|body|integer|false|The minimum percentage of the upstream's targets' weight that must be available for the whole upstream to be considered healthy.|
|» tags|body|[string]|false|An optional set of strings associated with the Upstream for grouping and filtering.|
|» host_header|body|string|false|The hostname to be used as Host header when proxying requests through Kong.|
|» client_certificate|body|object|false|If set, the certificate to be used as client certificate while TLS handshaking to the upstream server.|
|»» id|body|string|false|none|
|» use_srv_name|body|boolean|false|If set, the balancer will use SRV hostname(if DNS Answer has SRV record) as the proxy upstream Host.|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» algorithm|consistent-hashing|
|» algorithm|least-connections|
|» algorithm|round-robin|
|» algorithm|latency|
|» hash_on|none|
|» hash_on|consumer|
|» hash_on|ip|
|» hash_on|cookie|
|» hash_on|uri_capture|
|» hash_on|path|
|» hash_on|query_arg|
|» hash_fallback|none|
|» hash_fallback|consumer|
|» hash_fallback|ip|
|» hash_fallback|cookie|
|» hash_fallback|uri_capture|
|» hash_fallback|path|
|» hash_fallback|query_arg|
|»»» type|tcp|
|»»» type|http|
|»»» type|https|
|»»» type|grpc|
|»»» type|grpcs|
|»»»» http_statuses|200|
|»»»» http_statuses|201|
|»»»» http_statuses|202|
|»»»» http_statuses|203|
|»»»» http_statuses|204|
|»»»» http_statuses|205|
|»»»» http_statuses|206|
|»»»» http_statuses|207|
|»»»» http_statuses|208|
|»»»» http_statuses|226|
|»»»» http_statuses|300|
|»»»» http_statuses|301|
|»»»» http_statuses|302|
|»»»» http_statuses|303|
|»»»» http_statuses|304|
|»»»» http_statuses|305|
|»»»» http_statuses|306|
|»»»» http_statuses|307|
|»»»» http_statuses|308|
|»»»» http_statuses|429|
|»»»» http_statuses|500|
|»»»» http_statuses|503|
|»»» type|tcp|
|»»» type|http|
|»»» type|https|
|»»» type|grpc|
|»»» type|grpcs|

> Example responses

> Successfully upserted Upstream

```json
{
  "id": "58c8ccbb-eafb-4566-991f-2ed4f678fa70",
  "created_at": 1422386534,
  "name": "my-upstream",
  "algorithm": "round-robin",
  "hash_on": "none",
  "hash_fallback": "none",
  "hash_on_cookie_path": "/",
  "slots": 10000,
  "healthchecks": {
    "passive": {
      "type": "http",
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "unhealthy": {
        "http_statuses": [
          429,
          500,
          503
        ],
        "timeouts": 0,
        "http_failures": 0,
        "tcp_failures": 0
      }
    },
    "active": {
      "https_verify_certificate": true,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "successes": 0,
        "interval": 0
      },
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "timeouts": 0,
        "tcp_failures": 0,
        "interval": 0
      },
      "type": "http",
      "concurrency": 10,
      "headers": {
        "type": "object",
        "properties": {
          "x-my-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-my-header header."
          },
          "x-another-header": {
            "type": "array",
            "items": {
              "type": "string"
            },
            "description": "The value(s) of the x-another-header header."
          }
        }
      },
      "timeout": 1,
      "http_path": "/",
      "https_sni": "example.com"
    },
    "threshold": 0
  },
  "tags": [
    "user-level",
    "low-priority"
  ],
  "host_header": "example.com",
  "client_certificate": {
    "id": "ea29aaa3-3b2d-488c-b90c-56df8e0dd8c6"
  },
  "use_srv_name": false
}
```

> 400 Response

```json
{}
```

<h3 id="upsert-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Upstream|[Upstream](#schemaupstream)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Upstream|Inline|

<h3 id="upsert-upstream-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-vaults">Vaults</h1>

Vault objects are used to configure different vault connectors for [managing secrets](https://docs.konghq.com/gateway/latest/kong-enterprise/secrets-management/). 
Configuring a vault lets you reference secrets from other entities.
This allows for a proper separation of secrets and configuration and prevents secret sprawl. 
<br><br>
For example, you could store a certificate and a key in a vault, then reference them from a certificate entity. This way, the certificate and key are not stored in the entity directly and are more secure.
<br><br>
Secrets rotation can be managed using [TTLs](https://docs.konghq.com/gateway/latest/kong-enterprise/secrets-management/advanced-usage/).

## list-vault

<a id="opIdlist-vault"></a>

`GET /vaults`

*List all Vaults*

List all Vaults

<h3 id="list-vault-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "config": {
        "prefix": "vaults.config.resurrect_ttl"
      },
      "description": "environment variable based vault",
      "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
      "name": "env",
      "tags": [
        "foo",
        "bar"
      ]
    }
  ],
  "offset": "string"
}
```

<h3 id="list-vault-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Vaults|Inline|

<h3 id="list-vault-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Vault](#schemavault)]|false|none|[Vault entities are used to configure different Vault connectors. Examples of Vaults are Environment Variables, Hashicorp Vault and AWS Secrets Manager. Configuring a Vault allows referencing the secrets with other entities. For example a certificate entity can store a reference to a certificate and key, stored in a vault, instead of storing the certificate and key within the entity. This allows a proper separation of secrets and configuration and prevents secret sprawl.]|
|»» config|object|false|none|The configuration properties for the Vault which can be found on the [vaults' documentation page](https://docs.konghq.com/gateway/latest/kong-enterprise/secrets-management/advanced-usage/).|
|»»» prefix|string|false|none|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»» description|string|false|none|The description of the Vault entity.|
|»» id|string|false|none|none|
|»» name|string|false|none|The name of the Vault thats going to be added. Currently, the Vault implementation must be installed in every Kong instance.|
|»» prefix|string|false|none|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|»» tags|[string]|false|none|An optional set of strings associated with the Vault for grouping and filtering.|
|»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

#### Enumerated Values

|Property|Value|
|---|---|
|prefix|vaults.config.resurrect_ttl|
|prefix|vaults.config.neg_ttl|
|prefix|vaults.config.ttl|

<aside class="success">
This operation does not require authentication
</aside>

## create-vault

<a id="opIdcreate-vault"></a>

`POST /vaults`

*Create a new Vault*

Create a new Vault

> Body parameter

```json
{
  "prefix": "env",
  "name": "env",
  "description": "This vault is used to retrieve redis database access credentials",
  "config": {
    "prefix": "SSL_"
  },
  "tags": [
    "database-credentials",
    "data-plane"
  ]
}
```

<h3 id="create-vault-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» prefix|body|string|false|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|» name|body|string|false|The name of the Vault that's going to be added. Currently, the Vault implementation must be installed in every Kong instance.|
|» description|body|string|false|The description of the Vault object.|
|» config|body|object|false|The configuration properties for the Vault which can be found on the vaults' documentation page.|
|»» prefix|body|string|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the Vault for grouping and filtering.|

> Example responses

> 201 Response

```json
{
  "config": {
    "prefix": "vaults.config.resurrect_ttl"
  },
  "description": "environment variable based vault",
  "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
  "name": "env",
  "tags": [
    "foo",
    "bar"
  ]
}
```

<h3 id="create-vault-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Vault|[Vault](#schemavault)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Vault|Inline|

<h3 id="create-vault-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-vault

<a id="opIddelete-vault"></a>

`DELETE /vaults/{vault_id_or_prefix}`

*Delete a Vault*

Delete a Vault

<h3 id="delete-vault-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|vault_id_or_prefix|path|string|true|ID or prefix of the Vault to delete|

<h3 id="delete-vault-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Vault or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-vault

<a id="opIdget-vault"></a>

`GET /vaults/{vault_id_or_prefix}`

*Fetch a Vault*

Get a Vault using ID or prefix.

Vault entities are used to configure different Vault connectors.

<h3 id="get-vault-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|vault_id_or_prefix|path|string|true|The unique identifier or the prefix of the Vault to retrieve.|

> Example responses

> 200 Response

```json
{
  "config": {
    "prefix": "vaults.config.resurrect_ttl"
  },
  "description": "environment variable based vault",
  "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
  "name": "env",
  "tags": [
    "foo",
    "bar"
  ]
}
```

<h3 id="get-vault-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Vault|[Vault](#schemavault)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-vault

<a id="opIdupdate-vault"></a>

`PATCH /vaults/{vault_id_or_prefix}`

*Update a Vault*

Update a Vault

> Body parameter

```json
{
  "prefix": "env",
  "name": "env",
  "description": "This vault is used to retrieve redis database access credentials",
  "config": {
    "prefix": "SSL_"
  },
  "tags": [
    "database-credentials",
    "data-plane"
  ]
}
```

<h3 id="update-vault-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|vault_id_or_prefix|path|string|true|ID or prefix of the Vault to update|
|body|body|object|false|none|
|» prefix|body|string|false|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|» name|body|string|false|The name of the Vault that's going to be added. Currently, the Vault implementation must be installed in every Kong instance.|
|» description|body|string|false|The description of the Vault object.|
|» config|body|object|false|The configuration properties for the Vault which can be found on the vaults' documentation page.|
|»» prefix|body|string|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the Vault for grouping and filtering.|

> Example responses

> 200 Response

```json
{
  "config": {
    "prefix": "vaults.config.resurrect_ttl"
  },
  "description": "environment variable based vault",
  "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
  "name": "env",
  "tags": [
    "foo",
    "bar"
  ]
}
```

<h3 id="update-vault-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Vault|[Vault](#schemavault)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Vault|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-vault-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-vault

<a id="opIdupsert-vault"></a>

`PUT /vaults/{vault_id_or_prefix}`

*Upsert a Vault*

Create or Update Vault using ID or prefix.

> Body parameter

```json
{
  "prefix": "env",
  "name": "env",
  "description": "This vault is used to retrieve redis database access credentials",
  "config": {
    "prefix": "SSL_"
  },
  "tags": [
    "database-credentials",
    "data-plane"
  ]
}
```

<h3 id="upsert-vault-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|vault_id_or_prefix|path|string|true|Name or ID of the Vault to lookup|
|body|body|object|false|none|
|» prefix|body|string|false|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|» name|body|string|false|The name of the Vault that's going to be added. Currently, the Vault implementation must be installed in every Kong instance.|
|» description|body|string|false|The description of the Vault object.|
|» config|body|object|false|The configuration properties for the Vault which can be found on the vaults' documentation page.|
|»» prefix|body|string|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the Vault for grouping and filtering.|

> Example responses

> 200 Response

```json
{
  "config": {
    "prefix": "vaults.config.resurrect_ttl"
  },
  "description": "environment variable based vault",
  "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
  "name": "env",
  "tags": [
    "foo",
    "bar"
  ]
}
```

<h3 id="upsert-vault-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Vault|[Vault](#schemavault)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Vault|Inline|

<h3 id="upsert-vault-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-keys">Keys</h1>

A key object holds a representation of asymmetric keys in various formats. When Kong Gateway or a Kong plugin requires a specific public or private key to perform certain operations, it can use this entity.

## list-key

<a id="opIdlist-key"></a>

`GET /keys`

*List all Keys*

List all Keys

<h3 id="list-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "d958f66b-8e99-44d2-b0b4-edd5bbf24658",
      "jwk": "{\"alg\":\"RSA\",  \"kid\": \"42\",  ...}",
      "kid": "42",
      "name": "a-key",
      "pem": {
        "private_key": "-----BEGIN",
        "public_key": "-----BEGIN"
      },
      "set": {
        "id": "b86b331c-dcd0-4b3e-97ce-47c5a9543031"
      }
    }
  ],
  "offset": "string"
}
```

<h3 id="list-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Keys|Inline|

<h3 id="list-key-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Key](#schemakey)]|false|none|[A Key object holds a representation of asymmetric keys in various formats. When Kong or a Kong plugin requires a specific public or private key to perform certain operations, it can use this entity.]|
|»» Key|[Key](#schemakey)|false|none|A Key object holds a representation of asymmetric keys in various formats. When Kong or a Kong plugin requires a specific public or private key to perform certain operations, it can use this entity.|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» id|string|false|none|The unique identifier or the prefix of the Vault to delete.|
|»»» jwk|string|false|none|A JSON Web Key represented as a string.|
|»»» kid|string|false|none|A unique identifier for a key.|
|»»» name|string|false|none|The name to associate with the given keys.|
|»»» pem|object|false|none|A keypair in PEM format.|
|»»»» private_key|string|false|none|none|
|»»»» public_key|string|false|none|none|
|»»» set|object|false|none|The id (an UUID) of the key-set with which to associate the key.|
|»»»» id|string|false|none|none|
|»»» tags|[string]|false|none|An optional set of strings associated with the Key for grouping and filtering.|
|»»» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-key

<a id="opIdcreate-key"></a>

`POST /keys`

*Create a new Key*

This API endpoint allows you to create a new key. When the request is successful, the API will respond with a 200 status code and a JSON object that represents the newly created key. If the request is invalid, the API will respond with a `400` status code and an error message in the response body.

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "set": {
    "id": "string"
  },
  "name": "my-key",
  "kid": "42",
  "jwk": "{\\\"alg\\\":\\\"RSA\\\",  \\\"kid\\\": \\\"42\\\",  ...}",
  "pem": {
    "private_key": "private_key\": \"-----BEGIN",
    "public_key": "public_key\": \"-----BEGIN"
  },
  "tags": [
    "string"
  ]
}
```

<h3 id="create-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» set|body|object|false|The id (an UUID) of the key-set with which to associate the key .With form-encoded, the notation is `set.id=<set id>` or `set.name=<set name>`. With JSON, use `"set":{"id":"<set id>"}` or `"set":{"name":"<set name>"}.`|
|»» id|body|string|false|46CA83EE-671C-11ED-BFAB-2FE47512C77A|
|» name|body|string|false|The name to associate with the given keys.|
|» kid|body|string|true|A unique identifier for a key.|
|» jwk|body|string|false|A JSON Web Key represented as a string.|
|» pem|body|object|false|A keypair in PEM format.|
|»» private_key|body|string|false|none|
|»» public_key|body|string|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|

> Example responses

> 201 Response

```json
{
  "id": "d958f66b-8e99-44d2-b0b4-edd5bbf24658",
  "jwk": "{\"alg\":\"RSA\",  \"kid\": \"42\",  ...}",
  "kid": "42",
  "name": "a-key",
  "pem": {
    "private_key": "-----BEGIN",
    "public_key": "-----BEGIN"
  },
  "set": {
    "id": "b86b331c-dcd0-4b3e-97ce-47c5a9543031"
  }
}
```

<h3 id="create-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Key|[Key](#schemakey)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Key|Inline|

<h3 id="create-key-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-key

<a id="opIddelete-key"></a>

`DELETE /keys/{key_id_or_name}`

*Delete a Key*

Delete a Key

<h3 id="delete-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|key_id_or_name|path|string|true|The unique identifier or the name of the Key to retrieve.|

<h3 id="delete-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Key or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-key

<a id="opIdget-key"></a>

`GET /keys/{key_id_or_name}`

*Fetch a Key*

Get a Key using ID or name.

<h3 id="get-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|key_id_or_name|path|string|true|The unique identifier or the name of the Key to retrieve.|

> Example responses

> Successfully fetched Key

```json
{
  "id": "d958f66b-8e99-44d2-b0b4-edd5bbf24658",
  "jwk": "{\"alg\":\"RSA\",  \"kid\": \"42\",  ...}",
  "kid": "42",
  "name": "a-key",
  "pem": {
    "private_key": "-----BEGIN",
    "public_key": "-----BEGIN"
  },
  "set": {
    "id": "b86b331c-dcd0-4b3e-97ce-47c5a9543031"
  }
}
```

<h3 id="get-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Key|[Key](#schemakey)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-key

<a id="opIdupdate-key"></a>

`PATCH /keys/{key_id_or_name}`

*Update a Key*

Update a Key

> Body parameter

```json
{
  "set": {
    "id": "string"
  },
  "name": "my-key",
  "kid": "42",
  "jwk": "{\\\"alg\\\":\\\"RSA\\\",  \\\"kid\\\": \\\"42\\\",  ...}",
  "pem": {
    "private_key": "private_key\": \"-----BEGIN",
    "public_key": "public_key\": \"-----BEGIN"
  },
  "tags": [
    "string"
  ]
}
```

<h3 id="update-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» set|body|object|false|The id (an UUID) of the key-set with which to associate the key .With form-encoded, the notation is `set.id=<set id>` or `set.name=<set name>`. With JSON, use `"set":{"id":"<set id>"}` or `"set":{"name":"<set name>"}.`|
|»» id|body|string|false|46CA83EE-671C-11ED-BFAB-2FE47512C77A|
|» name|body|string|false|The name to associate with the given keys.|
|» kid|body|string|true|A unique identifier for a key.|
|» jwk|body|string|false|A JSON Web Key represented as a string.|
|» pem|body|object|false|A keypair in PEM format.|
|»» private_key|body|string|false|none|
|»» public_key|body|string|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|
|key_id_or_name|path|string|true|The unique identifier or the name of the Key to retrieve.|

> Example responses

> Successfully updated Key

```json
{
  "id": "24D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "set": {
    "id": "46CA83EE-671C-11ED-BFAB-2FE47512C77A"
  },
  "name": "my-key",
  "kid": "42",
  "jwk": "{\"alg\":\"RSA\",  \"kid\": \"42\",  ...}",
  "pem": {
    "private_key": "-----BEGIN",
    "public_key": "-----BEGIN"
  },
  "tags": [
    "application-a",
    "public-key-xyz"
  ],
  "created_at": 1422386534,
  "updated_at": 1422386534
}
```

> 400 Response

```json
{}
```

<h3 id="update-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Key|[Key](#schemakey)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Key|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-key-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-key

<a id="opIdupsert-key"></a>

`PUT /keys/{key_id_or_name}`

*Upsert a Key*

Create or Update Key using ID or name.

> Body parameter

```json
{
  "set": {
    "id": "string"
  },
  "name": "my-key",
  "kid": "42",
  "jwk": "{\\\"alg\\\":\\\"RSA\\\",  \\\"kid\\\": \\\"42\\\",  ...}",
  "pem": {
    "private_key": "private_key\": \"-----BEGIN",
    "public_key": "public_key\": \"-----BEGIN"
  },
  "tags": [
    "string"
  ]
}
```

<h3 id="upsert-key-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» set|body|object|false|The id (an UUID) of the key-set with which to associate the key .With form-encoded, the notation is `set.id=<set id>` or `set.name=<set name>`. With JSON, use `"set":{"id":"<set id>"}` or `"set":{"name":"<set name>"}.`|
|»» id|body|string|false|46CA83EE-671C-11ED-BFAB-2FE47512C77A|
|» name|body|string|false|The name to associate with the given keys.|
|» kid|body|string|true|A unique identifier for a key.|
|» jwk|body|string|false|A JSON Web Key represented as a string.|
|» pem|body|object|false|A keypair in PEM format.|
|»» private_key|body|string|false|none|
|»» public_key|body|string|false|none|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|
|key_id_or_name|path|string|true|The unique identifier or the name of the Key to retrieve.|

> Example responses

> Successfully upserted Key

```json
{
  "id": "d958f66b-8e99-44d2-b0b4-edd5bbf24658",
  "jwk": "{\"alg\":\"RSA\",  \"kid\": \"42\",  ...}",
  "kid": "42",
  "name": "a-key",
  "pem": {
    "private_key": "-----BEGIN",
    "public_key": "-----BEGIN"
  },
  "set": {
    "id": "b86b331c-dcd0-4b3e-97ce-47c5a9543031"
  }
}
```

> 400 Response

```json
{}
```

<h3 id="upsert-key-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Key|[Key](#schemakey)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Key|Inline|

<h3 id="upsert-key-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-key-sets">Key-sets</h1>

A JSON Web key set. Key sets are the preferred way to expose keys to plugins because they tell the plugin where to look for keys or have a scoping mechanism to restrict plugins to specific keys.

## list-key-set

<a id="opIdlist-key-set"></a>

`GET /key-sets`

*List all Key-sets*

Retrieve a list of all Key-sets in the system. A Key Set object holds a collection of asymmetric key objects. This entity allows to logically group keys by their purpose. Key Sets can be both tagged and filtered by tags.

<h3 id="list-key-set-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> Key set object response body

```json
{
  "id": "4D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "name": "my-key_set",
  "created_at": 1422386534,
  "updated_at": 1422386534,
  "tags": [
    "string"
  ],
  "next": "http://localhost:8001/key-sets?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="list-key-set-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Key set object response body|Inline|

<h3 id="list-key-set-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name to associate with the given key-set.|
|» created_at|integer|false|none|Unix epoch when the resource was last created.|
|» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» tags|[string]|false|none|The name to associate with the given key-set.|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-key-set

<a id="opIdcreate-key-set"></a>

`POST /key-sets`

*Create a new Key-set*

This endpoint allows creating a new Key-set by sending a JSON object that describes the Key-set to be created.The request body must contain all the fields required to create a new Key-set.

> Body parameter

```json
{
  "name": "my-key_set",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-key-set-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|false|The name to associate with the given key-set.|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|

> Example responses

> Key set object response body

```json
{
  "id": "4D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "name": "my-key_set",
  "created_at": 1422386534,
  "updated_at": 1422386534,
  "tags": [
    "string"
  ],
  "next": "http://localhost:8001/key-sets?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="create-key-set-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Key set object response body|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Returned if the request contains invalid data.|None|

<h3 id="create-key-set-responseschema">Response Schema</h3>

Status Code **201**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name to associate with the given key-set.|
|» created_at|integer|false|none|Unix epoch when the resource was last created.|
|» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» tags|[string]|false|none|The name to associate with the given key-set.|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## delete-key-set

<a id="opIddelete-key-set"></a>

`DELETE /key-sets/{key-set_id_or_name}`

*Delete a Key-set*

Delete a Key-set

> Note: This API is not available in DB-less mode.

<h3 id="delete-key-set-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|key-set_id_or_name|path|string|true|The unique identifier or the `name` attribute of the Key Set that should be associated to the newly-created Key.|

<h3 id="delete-key-set-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Key-set or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## get-key-set

<a id="opIdget-key-set"></a>

`GET /key-sets/{key-set_id_or_name}`

*Fetch a Key-set*

This endpoint retrieves information about a specific key-set based on its ID or name.

<h3 id="get-key-set-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|key-set_id_or_name|path|string|true|The unique identifier or the `name` attribute of the Key Set that should be associated to the newly-created Key.|

> Example responses

> Key set object response body

```json
{
  "id": "4D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "name": "my-key_set",
  "created_at": 1422386534,
  "updated_at": 1422386534,
  "tags": [
    "string"
  ],
  "next": "http://localhost:8001/key-sets?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

<h3 id="get-key-set-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Key set object response body|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="get-key-set-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name to associate with the given key-set.|
|» created_at|integer|false|none|Unix epoch when the resource was last created.|
|» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» tags|[string]|false|none|The name to associate with the given key-set.|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## update-key-set

<a id="opIdupdate-key-set"></a>

`PATCH /key-sets/{key-set_id_or_name}`

*Update a Key-set*

Update a Key-set using ID or name.

> Note: This API is not available in DB-less mode.

Inserts (or replaces) the Key Set under the requested resource with the definition specified in the body. The Key Set will be identified via the name or id attribute.

When the name or id attribute has the structure of a UUID, the Key Set being inserted/replaced will be identified by its id. Otherwise it will be identified by its name.

When creating a new Key Set without specifying id (neither in the URL nor in the body), then it will be auto-generated.

Notice that specifying a name in the URL and a different one in the request body is not allowed.

> Body parameter

```json
{
  "name": "my-key_set",
  "tags": [
    "string"
  ]
}
```

<h3 id="update-key-set-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|false|The name to associate with the given key-set.|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|
|key-set_id_or_name|path|string|true|The unique identifier or the `name` attribute of the Key Set that should be associated to the newly-created Key.|

> Example responses

> Key set object response body

```json
{
  "id": "4D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "name": "my-key_set",
  "created_at": 1422386534,
  "updated_at": 1422386534,
  "tags": [
    "string"
  ],
  "next": "http://localhost:8001/key-sets?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

> 400 Response

```json
{}
```

<h3 id="update-key-set-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Key set object response body|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Key-set|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-key-set-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name to associate with the given key-set.|
|» created_at|integer|false|none|Unix epoch when the resource was last created.|
|» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» tags|[string]|false|none|The name to associate with the given key-set.|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## upsert-key-set

<a id="opIdupsert-key-set"></a>

`PUT /key-sets/{key-set_id_or_name}`

*Update a Key-set*

Update a Key-set using ID or name.

> Note: This API is not available in DB-less mode.

> Body parameter

```json
{
  "name": "my-key_set",
  "tags": [
    "string"
  ]
}
```

<h3 id="upsert-key-set-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» name|body|string|false|The name to associate with the given key-set.|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|
|key-set_id_or_name|path|string|true|The unique identifier or the `name` attribute of the Key Set that should be associated to the newly-created Key.|

> Example responses

> Key set object response body

```json
{
  "id": "4D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "name": "my-key_set",
  "created_at": 1422386534,
  "updated_at": 1422386534,
  "tags": [
    "string"
  ],
  "next": "http://localhost:8001/key-sets?offset=6378122c-a0a1-438d-a5c6-efabae9fb969"
}
```

> 400 Response

```json
{}
```

<h3 id="upsert-key-set-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Key set object response body|Inline|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Key-set|Inline|

<h3 id="upsert-key-set-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» id|string|false|none|none|
|» name|string|false|none|The name to associate with the given key-set.|
|» created_at|integer|false|none|Unix epoch when the resource was last created.|
|» updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|» tags|[string]|false|none|The name to associate with the given key-set.|
|» next|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-information">Information</h1>

Information routes

## get-endpoints

<a id="opIdget-endpoints"></a>

`GET /endpoints`

*List all endpoints*

List all available endpoints provided by the Admin API.

> Example responses

> Example response

```json
{
  "data": [
    "/",
    "/acls",
    "/acls/{acls}",
    "/acls/{acls}/consumer",
    "/acme",
    "/acme/certificates",
    "/acme/certificates/{certificates}",
    "/acme_storage",
    "/acme_storage/{acme_storage}",
    "/applications",
    "/applications/{applications}",
    "/applications/{applications}/application_instances",
    "/applications/{applications}/application_instances/{application_instances}",
    "/applications/{applications}/consumer",
    "/applications/{applications}/credentials/{plugin}",
    "/applications/{applications}/credentials/{plugin}/{credential_id}",
    "/applications/{applications}/developer",
    "/auth",
    "/basic-auths",
    "/basic-auths/{basicauth_credentials}",
    "/basic-auths/{basicauth_credentials}/consumer",
    "/ca_certificates",
    "/ca_certificates/{ca_certificates}",
    "/ca_certificates/{ca_certificates}/mtls_auth_credentials",
    "/ca_certificates/{ca_certificates}/mtls_auth_credentials/{mtls_auth_credentials}",
    "/cache",
    "/cache/{key}",
    "/certificates",
    "/certificates/{certificates}",
    "/certificates/{certificates}/services",
    "/certificates/{certificates}/services/{services}",
    "/certificates/{certificates}/snis",
    "/certificates/{certificates}/snis/{snis}",
    "/certificates/{certificates}/upstreams",
    "/certificates/{certificates}/upstreams/{upstreams}",
    "/clustering/data-planes",
    "/clustering/status",
    "/config",
    "/consumers",
    "/consumers/{consumers}",
    "/consumers/{consumers}/acls",
    "/consumers/{consumers}/acls/{acls}",
    "/consumers/{consumers}/admins",
    "/consumers/{consumers}/admins/{admins}",
    "/consumers/{consumers}/applications",
    "/consumers/{consumers}/applications/{applications}",
    "/consumers/{consumers}/basic-auth",
    "/consumers/{consumers}/basic-auth/{basicauth_credentials}",
    "/consumers/{consumers}/developers",
    "/consumers/{consumers}/developers/{developers}",
    "/consumers/{consumers}/hmac-auth",
    "/consumers/{consumers}/hmac-auth/{hmacauth_credentials}",
    "/consumers/{consumers}/jwt",
    "/consumers/{consumers}/jwt/{jwt_secrets}",
    "/consumers/{consumers}/key-auth",
    "/consumers/{consumers}/key-auth/{keyauth_credentials}",
    "/consumers/{consumers}/key-auth-enc",
    "/consumers/{consumers}/key-auth-enc/{keyauth_enc_credentials}",
    "/consumers/{consumers}/login_attempts",
    "/consumers/{consumers}/login_attempts/{login_attempts}",
    "/consumers/{consumers}/mtls-auth",
    "/consumers/{consumers}/mtls-auth/{mtls_auth_credentials}",
    "/consumers/{consumers}/mtls_auth_credentials",
    "/consumers/{consumers}/mtls_auth_credentials/{mtls_auth_credentials}",
    "/consumers/{consumers}/oauth2",
    "/consumers/{consumers}/oauth2/{oauth2_credentials}",
    "/consumers/{consumers}/plugins",
    "/consumers/{consumers}/plugins/{plugins}",
    "/debug/cluster/log-level/{log_level}",
    "/debug/node/log-level",
    "/debug/node/log-level/{log_level}",
    "/debug/profiling/cpu",
    "/debug/profiling/gc-snapshot",
    "/debug/profiling/memory",
    "/degraphql_routes",
    "/degraphql_routes/{degraphql_routes}",
    "/degraphql_routes/{degraphql_routes}/service",
    "/endpoints",
    "/entities/migrate",
    "/event-hooks",
    "/event-hooks/sources",
    "/event-hooks/sources/{source}",
    "/event-hooks/sources/{source}/{event}",
    "/event-hooks/{event_hooks}",
    "/event-hooks/{event_hooks}/ping",
    "/event-hooks/{event_hooks}/test",
    "/files",
    "/files/*",
    "/files/partials/*",
    "/files/{files}",
    "/filter-chains",
    "/filter-chains/{filter_chains}",
    "/filter-chains/{filter_chains}/route",
    "/filter-chains/{filter_chains}/service",
    "/graphql-proxy-cache-advanced",
    "/graphql-proxy-cache-advanced/{cache_key}",
    "/graphql-proxy-cache-advanced/{plugin_id}/caches/{cache_key}",
    "/graphql-rate-limiting-advanced/costs",
    "/graphql-rate-limiting-advanced/costs/{graphql_ratelimiting_advanced_cost_decoration}",
    "/graphql_ratelimiting_advanced_cost_decoration",
    "/graphql_ratelimiting_advanced_cost_decoration/{graphql_ratelimiting_advanced_cost_decoration}",
    "/graphql_ratelimiting_advanced_cost_decoration/{graphql_ratelimiting_advanced_cost_decoration}/service",
    "/groups",
    "/groups/{groups}",
    "/groups/{groups}/roles",
    "/hmac-auths",
    "/hmac-auths/{hmacauth_credentials}",
    "/hmac-auths/{hmacauth_credentials}/consumer",
    "/jwt-signer/jwks",
    "/jwt-signer/jwks/{jwt_signer_jwks}",
    "/jwt-signer/jwks/{jwt_signer_jwks}/rotate",
    "/jwts",
    "/jwts/{jwt_secrets}",
    "/jwts/{jwt_secrets}/consumer",
    "/key-auths",
    "/key-auths/{keyauth_credentials}",
    "/key-auths/{keyauth_credentials}/consumer",
    "/key-auths-enc",
    "/key-auths-enc/{keyauth_enc_credentials}",
    "/key-auths-enc/{keyauth_enc_credentials}/consumer",
    "/key-sets",
    "/key-sets/{key_sets}",
    "/key-sets/{key_sets}/keys",
    "/key-sets/{key_sets}/keys/{keys}",
    "/keys",
    "/keys/{keys}",
    "/keys/{keys}/set",
    "/konnect_applications",
    "/konnect_applications/{konnect_applications}",
    "/license/report",
    "/licenses",
    "/licenses/{licenses}",
    "/login_attempts",
    "/login_attempts/{login_attempts}",
    "/login_attempts/{login_attempts}/consumer",
    "/metrics",
    "/mtls-auths",
    "/mtls-auths/{mtls_auth_credentials}/consumer",
    "/mtls_auth_credentials",
    "/mtls_auth_credentials/{mtls_auth_credentials}",
    "/mtls_auth_credentials/{mtls_auth_credentials}/ca_certificate",
    "/mtls_auth_credentials/{mtls_auth_credentials}/consumer",
    "/oauth2",
    "/oauth2/{oauth2_credentials}",
    "/oauth2/{oauth2_credentials}/consumer",
    "/oauth2/{oauth2_credentials}/oauth2_tokens",
    "/oauth2/{oauth2_credentials}/oauth2_tokens/{oauth2_tokens}",
    "/oauth2_tokens",
    "/oauth2_tokens/{oauth2_tokens}",
    "/oauth2_tokens/{oauth2_tokens}/credential",
    "/oauth2_tokens/{oauth2_tokens}/service",
    "/openid-connect/issuers",
    "/openid-connect/issuers/{oic_issuers}",
    "/openid-connect/jwks",
    "/plugins",
    "/plugins/enabled",
    "/plugins/schema/{name}",
    "/plugins/{plugins}",
    "/plugins/{plugins}/consumer",
    "/plugins/{plugins}/route",
    "/plugins/{plugins}/service",
    "/proxy-cache",
    "/proxy-cache/{cache_key}",
    "/proxy-cache/{plugin_id}/caches/{cache_key}",
    "/proxy-cache-advanced",
    "/proxy-cache-advanced/{cache_key}",
    "/proxy-cache-advanced/{plugin_id}/caches/{cache_key}",
    "/routes",
    "/routes/{routes}",
    "/routes/{routes}/filter-chains",
    "/routes/{routes}/filter-chains/{filter_chains}",
    "/routes/{routes}/filters/all",
    "/routes/{routes}/filters/disabled",
    "/routes/{routes}/filters/enabled",
    "/routes/{routes}/plugins",
    "/routes/{routes}/plugins/{plugins}",
    "/routes/{routes}/service",
    "/schemas/plugins/validate",
    "/schemas/plugins/{name}",
    "/schemas/{db_entity_name}/validate",
    "/schemas/{name}",
    "/services",
    "/services/{services}",
    "/services/{services}/application_instances",
    "/services/{services}/application_instances/{application_instances}",
    "/services/{services}/applications",
    "/services/{services}/client_certificate",
    "/services/{services}/degraphql/routes",
    "/services/{services}/degraphql/routes/{degraphql_routes}",
    "/services/{services}/degraphql_routes",
    "/services/{services}/degraphql_routes/{degraphql_routes}",
    "/services/{services}/filter-chains",
    "/services/{services}/filter-chains/{filter_chains}",
    "/services/{services}/graphql-rate-limiting-advanced/costs",
    "/services/{services}/graphql_ratelimiting_advanced_cost_decoration",
    "/services/{services}/graphql_ratelimiting_advanced_cost_decoration/{graphql_ratelimiting_advanced_cost_decoration}",
    "/services/{services}/oauth2_tokens",
    "/services/{services}/oauth2_tokens/{oauth2_tokens}",
    "/services/{services}/plugins",
    "/services/{services}/plugins/{plugins}",
    "/services/{services}/routes",
    "/services/{services}/routes/{routes}",
    "/sessions",
    "/sessions/{sessions}",
    "/snis",
    "/snis/{snis}",
    "/snis/{snis}/certificate",
    "/status",
    "/tags",
    "/tags/{tags}",
    "/targets",
    "/targets/{targets}",
    "/targets/{targets}/upstream",
    "/timers",
    "/upstreams",
    "/upstreams/{upstreams}",
    "/upstreams/{upstreams}/client_certificate",
    "/upstreams/{upstreams}/health",
    "/upstreams/{upstreams}/targets",
    "/upstreams/{upstreams}/targets/all",
    "/upstreams/{upstreams}/targets/{targets}",
    "/upstreams/{upstreams}/targets/{targets}/healthy",
    "/upstreams/{upstreams}/targets/{targets}/unhealthy",
    "/upstreams/{upstreams}/targets/{targets}/{address}/healthy",
    "/upstreams/{upstreams}/targets/{targets}/{address}/unhealthy",
    "/userinfo",
    "/vault-auth",
    "/vault-auth/{vault_auth_vaults}",
    "/vault-auth/{vault}/credentials",
    "/vault-auth/{vault}/credentials/token/{access_token}",
    "/vault-auth/{vault}/credentials/{consumer}",
    "/vaults",
    "/vaults/{vaults}",
    "/vitals/",
    "/vitals/cluster",
    "/vitals/cluster/status_codes",
    "/vitals/consumers/{consumer_id}/cluster",
    "/vitals/nodes/",
    "/vitals/nodes/{node_id}",
    "/vitals/reports/{entity_type}",
    "/vitals/status_code_classes",
    "/vitals/status_codes/by_consumer",
    "/vitals/status_codes/by_consumer_and_route",
    "/vitals/status_codes/by_route",
    "/vitals/status_codes/by_service"
  ]
}
```

<h3 id="get-endpoints-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Example response|Inline|

<h3 id="get-endpoints-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## head-endpoints

<a id="opIdhead-endpoints"></a>

`HEAD /{endpoint}`

*Check endpoint or entity existence*

Similar to `HTTP` GET, but does not return the body. Returns HTTP 200 when the endpoint exits or HTTP 404 when it does not. Other status codes are possible.

<h3 id="head-endpoints-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|endpoint|path|string|true|Any available endpoint|

<h3 id="head-endpoints-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Endpoint does not exist|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|204|Date|string||The date and time at which the message was originated|
|204|Content-Type|string||The media type of the message content|
|204|Connection|string||Indicates whether the connection will be closed after the message is completed|
|204|Access-Control-Allow-Origin|string||Indicates whether the resource can be accessed by any origin|
|204|X-Kong-Admin-Request-ID|string||A unique identifier for the request, generated by Kong|
|204|X-Kong-Admin-Latency|integer||The time taken to process the request on the server, in milliseconds|
|204|Server|string||The software used by the origin server to handle the request|

<aside class="success">
This operation does not require authentication
</aside>

## options-endpoint

<a id="opIdoptions-endpoint"></a>

`OPTIONS /{endpoint}`

*List method by endpoint*

List all the supported HTTP methods by an endpoint. This can also be used with a CORS preflight request.

<h3 id="options-endpoint-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|endpoint|path|string|true|Any available endpoint|

<h3 id="options-endpoint-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|204|Date|string||The date and time at which the message was originated|
|204|Connection|string||Indicates whether the connection will be closed after the message is completed|
|204|Access-Control-Allow-Origin|string||Indicates whether the resource can be accessed by any origin|
|204|Access-Control-Allow-Headers|string||Used in response to a preflight request to indicate which HTTP headers can be used during the actual request|
|204|X-Kong-Admin-Request-ID|string||A unique identifier for the request, generated by Kong|
|204|Allow|string||Lists the HTTP methods that are supported for the resource|
|204|Access-Control-Allow-Methods|string||Indicates the methods allowed when accessing the resource in response to a preflight request|
|204|X-Kong-Admin-Latency|integer||The time taken to process the request on the server, in milliseconds|
|204|Server|string||The software used by the origin server to handle the request|

<aside class="success">
This operation does not require authentication
</aside>

## get-schemas-entity

<a id="opIdget-schemas-entity"></a>

`GET /schemas/{entity}/validate`

*Retrieve entity schema*

Retrieve the schema of an entity. This is useful to understand what fields an entity accepts, and can be used for building third-party integrations with Kong.

<h3 id="get-schemas-entity-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|entity|path|string|true|none|

> Example responses

> OK

```json
{
  "fields": [
    {
      "id": {
        "auto": true,
        "type": "string",
        "uuid": true
      }
    },
    {
      "created_at": {
        "auto": true,
        "timestamp": true,
        "type": "integer"
      }
    }
  ]
}
```

<h3 id="get-schemas-entity-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="get-schemas-entity-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» fields|[object]|false|none|A value of a schema|
|»» id|object|false|none|A value of a schema|
|»»» auto|boolean|false|none|A value of a schema|
|»»» type|string|false|none|A value of a schema|
|»»» uuid|boolean|false|none|A value of a schema|

<aside class="success">
This operation does not require authentication
</aside>

## post-schemas-entity-validate

<a id="opIdpost-schemas-entity-validate"></a>

`POST /schemas/{entity}/validate`

*Validate a configuration against a schema*

Check validity of a configuration against its entity schema. This allows you to test your input before submitting a request to the entity endpoints of the Admin API.

A requests to the entity endpoint using the given configuration may still fail due to other reasons, such as invalid foreign key relationships or uniqueness check failures against the contents of the data store.

<h3 id="post-schemas-entity-validate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|entity|path|string|true|none|

> Example responses

> OK

```json
{
  "Message": "schema validation successful"
}
```

<h3 id="post-schemas-entity-validate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="post-schemas-entity-validate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» Message|string|false|none|A success message|

<aside class="success">
This operation does not require authentication
</aside>

## get-schemas-filters-filter_name

<a id="opIdget-schemas-filters-filter_name"></a>

`GET /schemas/filters/{filter_name}`

*Retrieve Proxy-Wasm Filter JSON Schema*

Retrieve the JSON Schema of a Proxy-Wasm filter's configuration. This is useful to understand what fields a filter accepts, and can be used for building third-party integrations to the Kong's filter chain system.

<h3 id="get-schemas-filters-filter_name-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filter_name|path|string|true|The name of a Proxy-Wasm filter|

> Example responses

> OK

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "add": {
      "type": "object",
      "properties": {
        "headers": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      },
      "required": [
        "headers"
      ]
    }
  },
  "required": [
    "add"
  ]
}
```

<h3 id="get-schemas-filters-filter_name-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="get-schemas-filters-filter_name-responseschema">Response Schema</h3>

Status Code **200**

*JSON Schema object describing the expected configuration for the filter. This endpoints always returns a JSON Schema object describing the filter configuration, even if the filter does not include JSON Schema metadata: if the configuration is expected as a raw string, the JSON Schema configuration returned by this endpoint will indicate so.
*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|

<aside class="success">
This operation does not require authentication
</aside>

## get-schemas-plugins-plugin_name

<a id="opIdget-schemas-plugins-plugin_name"></a>

`GET /schemas/plugins/{plugin_name}`

*Retrieve Plugin Schema*

Retrieve the schema of a plugin's configuration. This is useful to understand what fields a plugin accepts, and can be used for building third-party integrations to the Kong's plugin system.

<h3 id="get-schemas-plugins-plugin_name-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|plugin_name|path|string|true|The name of a Kong plugin|

<h3 id="get-schemas-plugins-plugin_name-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|None|

<aside class="success">
This operation does not require authentication
</aside>

## post-schemas-plugins-validate

<a id="opIdpost-schemas-plugins-validate"></a>

`POST /schemas/plugins/validate`

*Validate plugin schema*

Check validity of a plugin configuration against the plugins entity schema. This allows you to test your input before submitting a request to the entity endpoints of the Admin API.

This only performs the schema validation checks, checking that the input configuration is well-formed. A requests to the entity endpoint using the given configuration may still fail due to other reasons, such as invalid foreign key relationships or uniqueness check failures against the contents of the data store.

> Example responses

> OK

```json
{
  "message": "schema validation successful"
}
```

<h3 id="post-schemas-plugins-validate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="post-schemas-plugins-validate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|A successful message|

<aside class="success">
This operation does not require authentication
</aside>

## get-timers

<a id="opIdget-timers"></a>

`GET /timers`

*Retrieve Runtime Debugging Info of Kong's Timers*

Retrieve runtime stats data from [lua-resty-timer-ng](https://github.com/Kong/lua-resty-timer-ng).

> Example responses

> 200 Response

```json
{
  "stats": {
    "sys": {
      "total": 7,
      "waiting": 7,
      "runs": 7,
      "pending": 0,
      "running": 0
    },
    "flamegraph": {
      "pending": "@./kong/init.lua:706:init_worker();@./kong/runloop/handler.lua:1086:before() 0\n",
      "running": "@./kong/init.lua:706:init_worker();@./kong/runloop/handler.lua:1086:before() 0\n",
      "elapsed_time": "@./kong/init.lua:706:init_worker();@./kong/runloop/handler.lua:1086:before() 17\n"
    },
    "timers": {
      "meta": {
        "name": "@/build/luarocks/share/lua/5.1/resty/counter.lua:71:new()"
      }
    }
  },
  "worker": {
    "id": 0,
    "count": 0
  }
}
```

<h3 id="get-timers-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="get-timers-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» stats|object|false|none|Statistics about the worker|
|»» sys|object|false|none|List of the number of different type of timers|
|»»» total|integer|false|none|The total number of timers (running + pending + waiting)|
|»»» waiting|integer|false|none|The number of unexpired timers|
|»»» runs|integer|false|none|The total number of runs for the timers|
|»»» pending|integer|false|none|The number of pending timers|
|»»» running|integer|false|none|The number of running timers|
|»» flamegraph|object|false|none|String-encoded timer-related flamegraph data|
|»»» pending|string|false|none|The number of pending timers for the flamegraph|
|»»» running|string|false|none|The number of running timers for the flamegraph|
|»»» elapsed_time|string|false|none|The elapsed time for the flamegraph|
|»» timers|object|false|none|Timer statistics for the worker|
|»»» meta|object|false|none|Program callstack of created timers|
|»»»» name|string|false|none|The name of the timer's metadata|
|» worker|object|false|none|none|
|»» id|integer|false|none|The ordinal number of the current Nginx worker processes (starting from number 0).|
|»» count|integer|false|none|The total number of the Nginx worker processes.|

<aside class="success">
This operation does not require authentication
</aside>

## get-status

<a id="opIdget-status"></a>

`GET /status`

*Health Routes*

Retrieve usage information about a node, with some basic information about the connections being processed by the underlying nginx process, the status of the database connection, and node's memory usage.

If you want to monitor the Kong process, since Kong is built on top of nginx, every existing nginx monitoring tool or agent can be used.

> Example responses

> OK

```json
{
  "memory": {
    "lua_shared_dicts": {
      "kong_core_db_cache": {
        "capacity": "128.00 MiB",
        "allocated_slabs": "0.76 MiB"
      },
      "kong_core_db_cache_miss": {
        "capacity": "12.00 MiB",
        "allocated_slabs": "0.08 MiB"
      },
      "kong_db_cache": {
        "capacity": "128.00 MiB",
        "allocated_slabs": "0.78 MiB"
      },
      "kong_db_cache_miss": {
        "capacity": "12.00 MiB",
        "allocated_slabs": "0.08 MiB"
      },
      "kong_vitals_counters": {
        "capacity": "50.00 MiB",
        "allocated_slabs": "0.30 MiB"
      },
      "kong_vitals_lists": {
        "capacity": "1.00 MiB",
        "allocated_slabs": "0.02 MiB"
      },
      "kong_vitals": {
        "capacity": "1.00 MiB",
        "allocated_slabs": "0.02 MiB"
      },
      "kong_counters": {
        "capacity": "1.00 MiB",
        "allocated_slabs": "0.02 MiB"
      },
      "kong_reports_consumers": {
        "capacity": "10.00 MiB",
        "allocated_slabs": "0.07 MiB"
      },
      "kong_reports_routes": {
        "capacity": "1.00 MiB",
        "allocated_slabs": "0.02 MiB"
      },
      "kong_reports_services": {
        "capacity": "1.00 MiB",
        "allocated_slabs": "0.02 MiB"
      },
      "kong_profiling_state": {
        "capacity": "1.50 MiB",
        "allocated_slabs": "0.02 MiB"
      },
      "prometheus_metrics": {
        "capacity": "5.00 MiB",
        "allocated_slabs": "0.04 MiB"
      },
      "kong": {
        "capacity": "5.00 MiB",
        "allocated_slabs": "0.04 MiB"
      },
      "kong_locks": {
        "capacity": "8.00 MiB",
        "allocated_slabs": "0.06 MiB"
      },
      "kong_healthchecks": {
        "capacity": "5.00 MiB",
        "allocated_slabs": "0.04 MiB"
      },
      "kong_process_events": {
        "capacity": "5.00 MiB",
        "allocated_slabs": "0.04 MiB"
      },
      "kong_cluster_events": {
        "capacity": "5.00 MiB",
        "allocated_slabs": "0.04 MiB"
      },
      "kong_rate_limiting_counters": {
        "capacity": "12.00 MiB",
        "allocated_slabs": "0.08 MiB"
      }
    },
    "workers_lua_vms": [
      {
        "http_allocated_gc": "51.92 MiB",
        "pid": 2323
      },
      {
        "http_allocated_gc": "51.48 MiB",
        "pid": 2324
      },
      {
        "http_allocated_gc": "51.48 MiB",
        "pid": 2325
      },
      {
        "http_allocated_gc": "51.48 MiB",
        "pid": 2326
      },
      {
        "http_allocated_gc": "51.48 MiB",
        "pid": 2327
      }
    ]
  },
  "database": {
    "reachable": true
  },
  "server": {
    "connections_reading": 0,
    "connections_writing": 6,
    "total_requests": 28,
    "connections_waiting": 0,
    "connections_handled": 15,
    "connections_active": 6,
    "connections_accepted": 15
  }
}
```

<h3 id="get-status-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="get-status-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» memory|object|false|none|Metrics about the memory usage.|
|»» lua_shared_dicts|object|false|none|An array of information about dictionaries that are shared with all workers in a Kong node, where each array node contains how much memory is dedicated for the specific shared dictionary (capacity) and how much of said memory is in use (allocated_slabs).|
|»»» kong_core_db_cache|object|false|none|none|
|»»»» capacity|string|false|none|Memory capacity|
|»»»» allocated_slabs|string|false|none|Total allocated memory|
|»» workers_lua_vms|[object]|false|none|An array with all workers of the Kong node, each entry contains a `http_allocated_gc` string and a `pid`.|
|»»» http_allocated_gc|string|false|none|HTTP submodule's Lua virtual machine's memory usage information, as reported by|
|»»» pid|integer|false|none|worker's process identification number.|
|» database|object|false|none|Metrics about the database|
|»» reachable|boolean|false|none|A boolean value reflecting the state of the database connection. Please note that this flag does not reflect the health of the database itself.|
|» server|object|false|none|Metrics about the nginx HTTP/S server|
|»» connections_reading|integer|false|none|The current number of connections where Kong is reading the request header.|
|»» connections_writing|integer|false|none|The current number of connections where nginx is writing the response back to the client.|
|»» total_requests|integer|false|none|The total number of client requests.|
|»» connections_waiting|integer|false|none|The current number of idle client connections waiting for a request.|
|»» connections_handled|integer|false|none|The total number of handled connections. Generally, the parameter value is the same as accepts unless some resource limits have been reached.|
|»» connections_active|integer|false|none|The current number of active client connections including Waiting connections.|
|»» connections_accepted|integer|false|none|The total number of accepted client connections.|

<aside class="success">
This operation does not require authentication
</aside>

## get-schemas-vaults-vault_name

<a id="opIdget-schemas-vaults-vault_name"></a>

`GET /schemas/vaults/{vault_name}`

*Retrieve Vault Schema*

Retrieve the schema of a vault.

<h3 id="get-schemas-vaults-vault_name-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|vault_name|path|string|true|The vault schema to be returned|

> Example responses

> 200 Response

```json
{
  "config": {
    "prefix": "vaults.config.resurrect_ttl"
  },
  "description": "environment variable based vault",
  "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
  "name": "env",
  "tags": [
    "foo",
    "bar"
  ]
}
```

<h3 id="get-schemas-vaults-vault_name-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[Vault](#schemavault)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|No vault named|None|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-debug">Debug</h1>

Debug routes

## put-debug-cluster-control-planes-nodes-log-level-log_level

<a id="opIdput-debug-cluster-control-planes-nodes-log-level-log_level"></a>

`PUT /debug/cluster/control-planes-nodes/log-level/{log_level}`

*Set Node Log Level of All Control Plane Nodes*

Change the log level of all Control Plane nodes deployed in Hybrid (CP/DP) cluster.

See the [NGINX docs](http://nginx.org/en/docs/ngx_core_module.html#error_log) for a list of accepted values.

Care must be taken when changing the log level of a node to `debug` in a production environment because the disk could fill up quickly. As soon as the debug logging finishes, revert back to a higher level such as notice.

It's currently not possible to change the log level of DP and DB-less nodes.

<h3 id="put-debug-cluster-control-planes-nodes-log-level-log_level-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|log_level|path|string|true|Log levels are set in Kong's configuration. Log levels increase in order of their severity|

#### Enumerated Values

|Parameter|Value|
|---|---|
|log_level|info|
|log_level|notice|
|log_level|warn|
|log_level|error|
|log_level|crit|

> Example responses

> OK

```json
{
  "message": "log level changed"
}
```

<h3 id="put-debug-cluster-control-planes-nodes-log-level-log_level-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="put-debug-cluster-control-planes-nodes-log-level-log_level-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|Response message|

<aside class="success">
This operation does not require authentication
</aside>

## put-debug-cluster-log-level-log_level

<a id="opIdput-debug-cluster-log-level-log_level"></a>

`PUT /debug/cluster/log-level/{log_level}`

*Set Node Log Level of All Nodes*

Change the log level of all nodes in a cluster.

See the [NGINX docs](http://nginx.org/en/docs/ngx_core_module.html#error_log) for a list of accepted values.

It's currently not possible to change the log level of DP and DB-less nodes.

Currently, when a user dynamically changes the log level for the entire cluster, if a new node joins a cluster the new node will run at the previous log level, not at the log level that was previously set dynamically for the entire cluster.

<h3 id="put-debug-cluster-log-level-log_level-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|log_level|path|string|true|Log levels are set in Kong's configuration. Log levels increase in order of their severity|

#### Enumerated Values

|Parameter|Value|
|---|---|
|log_level|info|
|log_level|notice|
|log_level|warn|
|log_level|error|
|log_level|crit|

> Example responses

> OK

```json
{
  "message": "log level changed"
}
```

<h3 id="put-debug-cluster-log-level-log_level-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="put-debug-cluster-log-level-log_level-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|A message containing information about the log level|

<aside class="success">
This operation does not require authentication
</aside>

## get-debug-node-log-level

<a id="opIdget-debug-node-log-level"></a>

`GET /debug/node/log-level`

*Retrieve Node Log Level of A Node*

Retrieve the current log level of a node.

See the [NGINX documentation](http://nginx.org/en/docs/ngx_core_module.html#error_log) for the list of possible return values.

> Example responses

> OK

```json
{
  "message": "log level: debug"
}
```

<h3 id="get-debug-node-log-level-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="get-debug-node-log-level-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|A message containing the current log level of the node.|

<aside class="success">
This operation does not require authentication
</aside>

## put-debug-node-log-level-log_level

<a id="opIdput-debug-node-log-level-log_level"></a>

`PUT /debug/node/log-level/{log_level}`

*Set log level of a single node*

Change the log level of a node.

See the [NGINX documentation](http://nginx.org/en/docs/ngx_core_module.html#error_log) for the list of possible return values.

<h3 id="put-debug-node-log-level-log_level-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|log_level|path|string|true|Log levels are set in Kong's configuration. Log levels increase in order of their severity|

#### Enumerated Values

|Parameter|Value|
|---|---|
|log_level|info|
|log_level|notice|
|log_level|warn|
|log_level|error|
|log_level|crit|

> Example responses

> OK

```json
{
  "message": "log level changed"
}
```

<h3 id="put-debug-node-log-level-log_level-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|Inline|

<h3 id="put-debug-node-log-level-log_level-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» message|string|false|none|A message confirming the log level change|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-targets">Targets</h1>

A target is an IP address or hostname with a port that identifies an instance of a backend service. Every upstream can have many targets, and the targets can be dynamically added, modified, or deleted. Changes take effect on the fly.
<br><br>
To disable a target, post a new one with `weight=0`, or use the `DELETE` method to accomplish the same.

## list-targets-for-upstream

<a id="opIdlist-targets-for-upstream"></a>

`GET /upstreams/{upstream_id_or_name}/targets`

*List all Targets associated with an Upstream*

List all Targets associated with a an Upstream

<h3 id="list-targets-for-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|upstream_id_or_name|path|string|true|ID or name of the related Upstream|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "089292a7-ba3d-4d88-acf0-97b4b2e2621a",
      "target": "203.0.113.42",
      "upstream": {
        "id": "5f1d7e76-2fed-4806-a6af-869984f025cb"
      },
      "weight": 100
    }
  ],
  "offset": "string"
}
```

<h3 id="list-targets-for-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Targets|Inline|

<h3 id="list-targets-for-upstream-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Target](#schematarget)]|false|none|[A target is an ip address/hostname with a port that identifies an instance of a backend service. Every upstream can have many targets, and the targets can be dynamically added, modified, or deleted. Changes take effect on the fly. To disable a target, post a new one with `weight=0`; alternatively, use the `DELETE` convenience method to accomplish the same. The current target object definition is the one with the latest `created_at`.]|
|»» created_at|number|false|none|Unix epoch when the resource was created.|
|»» id|string|false|none|The unique identifier or the name of the upstream for which to update the target.|
|»» tags|[string]|false|none|An optional set of strings associated with the Target for grouping and filtering.|
|»» target|string|false|none|The target address (ip or hostname) and port. If the hostname resolves to an SRV record, the `port` value will be overridden by the value from the DNS record.|
|»» upstream|object|false|none|The unique identifier or the name of the upstream for which to update the target.|
|»»» id|string|false|none|none|
|»» weight|integer|false|none|The weight this target gets within the upstream loadbalancer (`0`-`65535`). If the hostname resolves to an SRV record, the `weight` value will be overridden by the value from the DNS record.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-target-for-upstream

<a id="opIdcreate-target-for-upstream"></a>

`POST /upstreams/{upstream_id_or_name}/targets`

*Create a new Target associated with an Upstream*

Create a new Target associated with an Upstream

> Body parameter

```json
{
  "upstream": {
    "id": "173a6cee-90d1-40a7-89cf-0329eca780a6"
  },
  "weight": 100,
  "tags": [
    "string"
  ]
}
```

<h3 id="create-target-for-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|upstream_id_or_name|path|string|true|ID or name of the related Upstream|
|body|body|object|false|none|
|» upstream|body|object|false|The unique identifier or the name of the upstream for which to update the target.|
|»» id|body|string|false|The unique identifier or the name of the upstream for which to update the target.|
|» weight|body|integer|false|The weight this target gets within the upstream loadbalancer (`0`-`65535`). If the hostname resolves to an SRV record, the `weight` value will be overridden by the value from the DNS record.|
|» tags|body|[string]|false|An optional set of strings associated with the Target for grouping and filtering.|

> Example responses

> Successfully created Target

```json
{
  "id": "173a6cee-90d1-40a7-89cf-0329eca780a6",
  "created_at": 1422386534,
  "upstream": {
    "id": "bdab0e47-4e37-4f0b-8fd0-87d95cc4addc"
  },
  "target": "example.com:8000",
  "weight": 100,
  "tags": [
    "user-level",
    "low-priority"
  ]
}
```

> 400 Response

```json
{}
```

<h3 id="create-target-for-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Target|[Target](#schematarget)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Target|Inline|

<h3 id="create-target-for-upstream-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete-upstream-target

<a id="opIddelete-upstream-target"></a>

`DELETE /upstreams/{upstream_id_or_name}/targets/{target_id_or_target}`

*Delete a Target associated with a an Upstream*

Delete a Target associated with a an Upstream using ID or target.

<h3 id="delete-upstream-target-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|
|target_id_or_target|path|string|true|The host/port combination element of the target to set as unhealthy, or the `id` of an existing target entry.|

<h3 id="delete-upstream-target-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successfully deleted Target or the resource didn't exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## fetch-target-for-upstream

<a id="opIdfetch-target-for-upstream"></a>

`GET /upstreams/{upstream_id_or_name}/targets/{target_id_or_target}`

*Fetch a Target associated with an Upstream*

Get a Target associated with an Upstream using ID or target.

<h3 id="fetch-target-for-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|
|target_id_or_target|path|string|true|The host/port combination element of the target to set as unhealthy, or the `id` of an existing target entry.|

> Example responses

> 200 Response

```json
{
  "id": "089292a7-ba3d-4d88-acf0-97b4b2e2621a",
  "target": "203.0.113.42",
  "upstream": {
    "id": "5f1d7e76-2fed-4806-a6af-869984f025cb"
  },
  "weight": 100
}
```

<h3 id="fetch-target-for-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully fetched Target|[Target](#schematarget)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<aside class="success">
This operation does not require authentication
</aside>

## update-target-for-upstream

<a id="opIdupdate-target-for-upstream"></a>

`PATCH /upstreams/{upstream_id_or_name}/targets/{target_id_or_target}`

*Update a target associated with an Upstream*

Update a Target associated with a an Upstream using ID or target.

> Body parameter

```json
{
  "upstream": {
    "id": "173a6cee-90d1-40a7-89cf-0329eca780a6"
  },
  "weight": 100,
  "tags": [
    "string"
  ]
}
```

<h3 id="update-target-for-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» upstream|body|object|false|The unique identifier or the name of the upstream for which to update the target.|
|»» id|body|string|false|The unique identifier or the name of the upstream for which to update the target.|
|» weight|body|integer|false|The weight this target gets within the upstream loadbalancer (`0`-`65535`). If the hostname resolves to an SRV record, the `weight` value will be overridden by the value from the DNS record.|
|» tags|body|[string]|false|An optional set of strings associated with the Target for grouping and filtering.|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|
|target_id_or_target|path|string|true|The host/port combination element of the target to set as unhealthy, or the `id` of an existing target entry.|

> Example responses

> 200 Response

```json
{
  "id": "089292a7-ba3d-4d88-acf0-97b4b2e2621a",
  "target": "203.0.113.42",
  "upstream": {
    "id": "5f1d7e76-2fed-4806-a6af-869984f025cb"
  },
  "weight": 100
}
```

<h3 id="update-target-for-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully updated Target|[Target](#schematarget)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Target|Inline|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Resource does not exist|None|

<h3 id="update-target-for-upstream-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## upsert-target-for-upstream

<a id="opIdupsert-target-for-upstream"></a>

`PUT /upstreams/{upstream_id_or_name}/targets/{target_id_or_target}`

*Upsert a Target associated with an Upstream*

Create or Update a Target associated with an Upstream using ID or target.

> Body parameter

```json
{
  "upstream": {
    "id": "173a6cee-90d1-40a7-89cf-0329eca780a6"
  },
  "weight": 100,
  "tags": [
    "string"
  ]
}
```

<h3 id="upsert-target-for-upstream-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|none|
|» upstream|body|object|false|The unique identifier or the name of the upstream for which to update the target.|
|»» id|body|string|false|The unique identifier or the name of the upstream for which to update the target.|
|» weight|body|integer|false|The weight this target gets within the upstream loadbalancer (`0`-`65535`). If the hostname resolves to an SRV record, the `weight` value will be overridden by the value from the DNS record.|
|» tags|body|[string]|false|An optional set of strings associated with the Target for grouping and filtering.|
|upstream_id_or_name|path|string|true|The unique identifier or the name of the Upstream associated to the Certificate to be retrieved.|
|target_id_or_target|path|string|true|The host/port combination element of the target to set as unhealthy, or the `id` of an existing target entry.|

> Example responses

> 200 Response

```json
{
  "id": "089292a7-ba3d-4d88-acf0-97b4b2e2621a",
  "target": "203.0.113.42",
  "upstream": {
    "id": "5f1d7e76-2fed-4806-a6af-869984f025cb"
  },
  "weight": 100
}
```

<h3 id="upsert-target-for-upstream-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successfully upserted Target|[Target](#schematarget)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Target|Inline|

<h3 id="upsert-target-for-upstream-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-tags">Tags</h1>

Tags are strings associated to entities in Kong Gateway. Tags can contain almost all UTF-8 characters, with the following exceptions: `,`,`/`, and non-printable ASCII (for example, the space character). 
<br><br>
Most core entities can be tagged via the tags attribute upon creation or modification.

## get-tags

<a id="opIdget-tags"></a>

`GET /tags`

*List all tags*

Returns a paginated list of all the tags in the system.

The list of entities will not be restricted to a single entity type: all the entities tagged with tags will be present on this list.

If an entity is tagged with more than one tag, the entity_id for that entity will appear more than once in the resulting list. Similarly, if several entities have been tagged with the same tag, the tag will appear in several items of this list.

> Example responses

> Tags response body

```json
{
  "data": [
    {
      "entity_name": "services",
      "entity_id": "c87440e1-0496-420b-b06f-dac59544bb6c",
      "tag": "example"
    }
  ],
  "offset": "1fb491c4-f4a7-4bca-aeba-7f3bcee4d2f9",
  "next": "/tags/example?offset=1fb491c4-f4a7-4bca-aeba-7f3bcee4d2f9"
}
```

<h3 id="get-tags-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Tags response body|Inline|

<h3 id="get-tags-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|none|
|»» entity_name|string|false|none|The name of the entity that corresponds to a tag|
|»» entity_id|string|false|none|The unique ID for the entity that is attached to the tag|
|»» tag|string|false|none|The tag|
|» offset|string|false|none|Pagination information|
|» next|string|false|none|Pagination information|

<aside class="success">
This operation does not require authentication
</aside>

## get-tags-tags

<a id="opIdget-tags-tags"></a>

`GET /tags/{tags}`

*List entity by tag*

Returns the entities that have been tagged with the specified tag.

The list of entities will not be restricted to a single entity type: all the entities tagged with tags will be present on this list.

<h3 id="get-tags-tags-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|tags|path|string|true|Tags are strings associated to entities in Kong.|

> Example responses

> Tags response body

```json
{
  "data": [
    {
      "entity_name": "services",
      "entity_id": "c87440e1-0496-420b-b06f-dac59544bb6c",
      "tag": "example"
    }
  ],
  "offset": "1fb491c4-f4a7-4bca-aeba-7f3bcee4d2f9",
  "next": "/tags/example?offset=1fb491c4-f4a7-4bca-aeba-7f3bcee4d2f9"
}
```

<h3 id="get-tags-tags-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Tags response body|Inline|

<h3 id="get-tags-tags-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[object]|false|none|none|
|»» entity_name|string|false|none|The name of the entity that corresponds to a tag|
|»» entity_id|string|false|none|The unique ID for the entity that is attached to the tag|
|»» tag|string|false|none|The tag|
|» offset|string|false|none|Pagination information|
|» next|string|false|none|Pagination information|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-acls">ACLs</h1>

## list-acls-for-consumer

<a id="opIdlist-acls-for-consumer"></a>

`GET /consumers/{consumer_username_or_id}/acls`

*List all ACLs associated with a consumer*

Retrieve a list of all ACLs associated with a consumer.

<h3 id="list-acls-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "consumer": {
        "id": "string"
      },
      "created_at": 0,
      "id": "string",
      "group": "string",
      "tags": [
        "string"
      ]
    }
  ],
  "offset": "string"
}
```

<h3 id="list-acls-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Consumers|Inline|

<h3 id="list-acls-for-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[ACL](#schemaacl)]|false|none|[ACL entities are used to control access to a resource. An ACL can be applied to a Consumer]|
|»» ACL|[ACL](#schemaacl)|false|none|ACL entities are used to control access to a resource. An ACL can be applied to a Consumer|
|»»» consumer|object|false|none|The Consumer to which this ACL is associated. A Consumer can have many ACLs.|
|»»»» id|string|false|none|none|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» id|string|false|none|none|
|»»» group|string|false|none|The group that this ACL represents.|
|»»» tags|[string]|false|none|An optional set of strings associated with the ACL for grouping and filtering.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-acl-for-consumer

<a id="opIdcreate-acl-for-consumer"></a>

`POST /consumers/{consumer_username_or_id}/acls`

*Create a new ACL associated with a Consumer*

Create a new ACL associated with a Consumer

> Body parameter

```json
{
  "consumer": "string",
  "group": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-acl-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|ACL request body|
|» consumer|body|string¦null|false|The id/username of the consumer that this ACL will be associated with.|
|» group|body|string|true|The name of the group to associate with the given consumer.|
|» tags|body|[string]|false|An optional set of strings associated with the ACL for grouping and filtering.|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 201 Response

```json
{
  "consumer": {
    "id": "string"
  },
  "created_at": 0,
  "id": "string",
  "group": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-acl-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created ACL|[ACL](#schemaacl)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid ACL|Inline|

<h3 id="create-acl-for-consumer-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="kong-admin-api-key-auths">Key-auths</h1>

## list-key-auths-for-consumer

<a id="opIdlist-key-auths-for-consumer"></a>

`GET /consumers/{consumer_username_or_id}/key-auth`

*List all Key-auths associated with a consumer*

Retrieve a list of all Key-auths associated with a consumer.

<h3 id="list-key-auths-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|size|query|integer|false|Number of resources to be returned.|
|offset|query|string|false|Offset from which to return the next set of resources. Use the value of the 'offset' field from the response of a list operation as input here to paginate through all the resources|
|tags|query|string|false|A list of tags to filter the list of resources on. Multiple tags can be concatenated using ','' to mean AND or using ''/'' to mean OR.'|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "consumer": {
        "id": "string"
      },
      "created_at": 0,
      "id": "string",
      "key": "string",
      "tags": [
        "string"
      ]
    }
  ],
  "offset": "string"
}
```

<h3 id="list-key-auths-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A successful response listing Key-auths|Inline|

<h3 id="list-key-auths-for-consumer-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» data|[[Key-auth](#schemakey-auth)]|false|none|[A Key-auth entity represents a key used to authenticate consumers with the key-auth plugin. The key-auth plugin is used to protect API endpoints by requiring a secret key to be sent with the request.]|
|»» Key-auth|[Key-auth](#schemakey-auth)|false|none|A Key-auth entity represents a key used to authenticate consumers with the key-auth plugin. The key-auth plugin is used to protect API endpoints by requiring a secret key to be sent with the request.|
|»»» consumer|object|false|none|The Consumer to which this Key is associated. A Consumer can have many Key-auth credentials.|
|»»»» id|string|false|none|none|
|»»» created_at|integer|false|none|Unix epoch when the resource was created.|
|»»» id|string|false|none|none|
|»»» key|string|false|none|The key that will be used to authenticate the consumer. If this field is not specified, it will be auto-generated.|
|»»» tags|[string]|false|none|An optional set of strings associated with the Key for grouping and filtering.|
|» offset|[pagination-offset-response](#schemapagination-offset-response)|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

<aside class="success">
This operation does not require authentication
</aside>

## create-key-auth-for-consumer

<a id="opIdcreate-key-auth-for-consumer"></a>

`POST /consumers/{consumer_username_or_id}/key-auth`

*Create a new Key-auth associated with a Consumer*

Create a new Key-auth associated with a Consumer

> Body parameter

```json
{
  "consumer": "string",
  "key": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-key-auth-for-consumer-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|key-auth request body|
|» consumer|body|string¦null|false|The id/username of the consumer that this Key will be associated with.|
|» key|body|string¦null|false|The key to associate with the given consumer. If not specified, it will be auto-generated.|
|» tags|body|[string]|false|An optional set of strings associated with the Key for grouping and filtering.|
|consumer_username_or_id|path|string|true|The unique identifier or the username of the Consumer to retrieve.|

> Example responses

> 201 Response

```json
{
  "consumer": {
    "id": "string"
  },
  "created_at": 0,
  "id": "string",
  "key": "string",
  "tags": [
    "string"
  ]
}
```

<h3 id="create-key-auth-for-consumer-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Successfully created Key-auth|[Key-auth](#schemakey-auth)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid Key-auth|Inline|

<h3 id="create-key-auth-for-consumer-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ACL">ACL</h2>
<!-- backwards compatibility -->
<a id="schemaacl"></a>
<a id="schema_ACL"></a>
<a id="tocSacl"></a>
<a id="tocsacl"></a>

```json
{
  "consumer": {
    "id": "string"
  },
  "created_at": 0,
  "id": "string",
  "group": "string",
  "tags": [
    "string"
  ]
}

```

ACL

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|consumer|object|false|none|The Consumer to which this ACL is associated. A Consumer can have many ACLs.|
|» id|string|false|none|none|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string|false|none|none|
|group|string|false|none|The group that this ACL represents.|
|tags|[string]|false|none|An optional set of strings associated with the ACL for grouping and filtering.|

<h2 id="tocS_CA-Certificate">CA-Certificate</h2>
<!-- backwards compatibility -->
<a id="schemaca-certificate"></a>
<a id="schema_CA-Certificate"></a>
<a id="tocSca-certificate"></a>
<a id="tocsca-certificate"></a>

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f260"
}

```

CA-Certificate

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cert|string|false|none|PEM-encoded public certificate of the CA.|
|cert_digest|string|false|none|SHA256 hex digest of the public certificate.|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string(uuid)|false|none|none|
|tags|[string]|false|none|An optional set of strings associated with the Certificate for grouping and filtering.|

<h2 id="tocS_Certificate">Certificate</h2>
<!-- backwards compatibility -->
<a id="schemacertificate"></a>
<a id="schema_Certificate"></a>
<a id="tocScertificate"></a>
<a id="tocscertificate"></a>

```json
{
  "cert": "-----BEGIN CERTIFICATE-----\ncertificate-content\n-----END CERTIFICATE-----",
  "id": "b2f34145-0343-41a4-9602-4c69dec2f269",
  "key": "-----BEGIN PRIVATE KEY-----\nprivate-key-content\n-----END PRIVATE KEY-----"
}

```

Certificate

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cert|string|false|none|PEM-encoded public certificate chain of the SSL key This field is referenceable and can be stored in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|cert_alt|string|false|none|PEM-encoded public certificate chain of the alternate SSL key pair. This should only be set if you have both RSA and ECDSA types of certificate available and would like Kong to prefer serving using ECDSA certs.|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string(uuid)|false|none|The UUID representation of the certificate object.|
|key|string|false|none|PEM-encoded private key of the SSL key pair. This field is _referenceable_, which means it can be securely stored as a [secret](/gateway/latest/plan-and-deploy/security/secrets-management/getting-started) in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|key_alt|string|false|none|PEM-encoded private key of the alternate SSL key pair. This should only be set if you have both RSA and ECDSA types of certificate available and would like Kong to prefer serving using ECDSA certs when client advertises support for it. This field is _referenceable_, which means it can be securely stored as a [secret](/gateway/latest/plan-and-deploy/security/secrets-management/getting-started) in a vault. References must follow a [specific format](/gateway/latest/plan-and-deploy/security/secrets-management/reference-format).|
|tags|[string]|false|none|An optional set of strings associated with the Certificate for grouping and filtering.|

<h2 id="tocS_Consumer">Consumer</h2>
<!-- backwards compatibility -->
<a id="schemaconsumer"></a>
<a id="schema_Consumer"></a>
<a id="tocSconsumer"></a>
<a id="tocsconsumer"></a>

```json
{
  "custom_id": "4200",
  "id": "8a388226-80e8-4027-a486-25e4f7db5d21",
  "tags": [
    "silver-tier"
  ],
  "username": "bob-the-builder"
}

```

Consumer

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|custom_id|string|false|none|Field for storing an existing unique ID for the Consumer - useful for mapping Kong with users in your existing database. You must send either this field or `username` with the request.|
|id|string|false|none|none|
|tags|[string]|false|none|An optional set of strings associated with the Consumer for grouping and filtering.|
|username|string|false|none|The unique username of the Consumer. You must send either this field or `custom_id` with the request.|

<h2 id="tocS_Filter-chain">Filter-chain</h2>
<!-- backwards compatibility -->
<a id="schemafilter-chain"></a>
<a id="schema_Filter-chain"></a>
<a id="tocSfilter-chain"></a>
<a id="tocsfilter-chain"></a>

```json
{
  "id": "ce44eef5-41ed-47f6-baab-f725cecf98c7",
  "name": "my-filter-chain",
  "created_at": 1422386534,
  "updated_at": 1422386534,
  "enabled": true,
  "route": null,
  "service": "20487393-41ed-47f6-93a8-3407cade2002",
  "filters": [
    {
      "name": "go-rate-limiting",
      "enabled": true,
      "config": "{ \"minute\": 30 }"
    },
    {
      "name": "rust-response-transformer",
      "enabled": true,
      "config": "{ \"remove_header\": \"X-Example\" }"
    }
  ],
  "tags": [
    "my-tag"
  ]
}

```

Filter Chain

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|enabled|boolean|false|none|Whether the filter chain is applied.|
|filters|[object]|false|none|An array of filter definitions that will be executed in order.|
|» name|string|false|none|The name of the filter. This name matches the basename of the WebAssembly module file: for a filter file called `my-filter.wasm`, then filter name will be `my-filter`.|
|» config|any|false|none|The configuration for the filter. Proxy-Wasm does not define a configuration format, so this field accepts either a raw string, or a JSON object. A raw string is passed uninterpreted to the filter, to be validated at request time. If a JSON object is used, there must be a metadata file called `my-filter.meta.json` in the same folder as your `my-filter.wasm` file. The metadata file must contain an object with a field `"config_schema"`, and its value must the JSON Schema for the filter configuration. This schema will be used for validating the configuration upon insertion in the filter chain, ahead of execution.|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» enabled|boolean|false|none|Whether the filter is to be applied.|
|id|string|false|none|none|
|name|string|false|none|The name of the filter chain.|
|route|object|false|none|The route to which this chain is applied. A filter chain must be applied to either a single route or a single service.|
|» id|string|false|none|none|
|service|object|false|none|The service to which this chain is applied. A filter chain must be applied to either a single route or a single service.|
|» id|string|false|none|none|
|tags|[string]|false|none|An optional set of strings associated with the Filter Chain for grouping and filtering.|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|

<h2 id="tocS_Key">Key</h2>
<!-- backwards compatibility -->
<a id="schemakey"></a>
<a id="schema_Key"></a>
<a id="tocSkey"></a>
<a id="tocskey"></a>

```json
{
  "id": "d958f66b-8e99-44d2-b0b4-edd5bbf24658",
  "jwk": "{\"alg\":\"RSA\",  \"kid\": \"42\",  ...}",
  "kid": "42",
  "name": "a-key",
  "pem": {
    "private_key": "-----BEGIN",
    "public_key": "-----BEGIN"
  },
  "set": {
    "id": "b86b331c-dcd0-4b3e-97ce-47c5a9543031"
  }
}

```

Key

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string|false|none|The unique identifier or the prefix of the Vault to delete.|
|jwk|string|false|none|A JSON Web Key represented as a string.|
|kid|string|false|none|A unique identifier for a key.|
|name|string|false|none|The name to associate with the given keys.|
|pem|object|false|none|A keypair in PEM format.|
|» private_key|string|false|none|none|
|» public_key|string|false|none|none|
|set|object|false|none|The id (an UUID) of the key-set with which to associate the key.|
|» id|string|false|none|none|
|tags|[string]|false|none|An optional set of strings associated with the Key for grouping and filtering.|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|

<h2 id="tocS_Key-auth">Key-auth</h2>
<!-- backwards compatibility -->
<a id="schemakey-auth"></a>
<a id="schema_Key-auth"></a>
<a id="tocSkey-auth"></a>
<a id="tocskey-auth"></a>

```json
{
  "consumer": {
    "id": "string"
  },
  "created_at": 0,
  "id": "string",
  "key": "string",
  "tags": [
    "string"
  ]
}

```

Key-auth

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|consumer|object|false|none|The Consumer to which this Key is associated. A Consumer can have many Key-auth credentials.|
|» id|string|false|none|none|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string|false|none|none|
|key|string|false|none|The key that will be used to authenticate the consumer. If this field is not specified, it will be auto-generated.|
|tags|[string]|false|none|An optional set of strings associated with the Key for grouping and filtering.|

<h2 id="tocS_Key-set">Key-set</h2>
<!-- backwards compatibility -->
<a id="schemakey-set"></a>
<a id="schema_Key-set"></a>
<a id="tocSkey-set"></a>
<a id="tocskey-set"></a>

```json
{
  "created_at": 0,
  "id": "24D0DBDA-671C-11ED-BA0B-EF1DCCD3725F",
  "name": "\"example-key-set\"",
  "tags": [
    "[\"google-keys\", \"mozilla-keys\"]"
  ],
  "updated_at": 0
}

```

Key-set

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string|false|none|The unique identifier or the name of the Key to delete.|
|name|string|false|none|The name to associate with the given key-set.|
|tags|[string]|false|none|An optional set of strings associated with the Key for grouping and filtering|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|

<h2 id="tocS_Plugin">Plugin</h2>
<!-- backwards compatibility -->
<a id="schemaplugin"></a>
<a id="schema_Plugin"></a>
<a id="tocSplugin"></a>
<a id="tocsplugin"></a>

```json
{
  "config": {
    "anonymous": null,
    "hide_credentials": false,
    "key_in_body": false,
    "key_in_header": true,
    "key_in_query": true,
    "key_names": [
      "apikey"
    ],
    "run_on_preflight": true
  },
  "enabled": true,
  "id": "3fd1eea1-885a-4011-b986-289943ff8177",
  "name": "key-auth",
  "protocols": [
    "grpc",
    "grpcs",
    "http",
    "https"
  ]
}

```

Plugin

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|config|object|false|none|The configuration properties for the Plugin which can be found on the plugins documentation page in the [Kong Hub](https://docs.konghq.com/hub/).|
|consumer|object|false|none|If set, the plugin will activate only for requests where the specified has been authenticated. (Note that some plugins can not be restricted to consumers this way.). Leave unset for the plugin to activate regardless of the authenticated Consumer.|
|» id|string|false|none|none|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|enabled|boolean|false|none|Whether the plugin is applied.|
|id|string|false|none|none|
|instance_name|string|false|none|none|
|name|string|false|none|The name of the Plugin thats going to be added. Currently, the Plugin must be installed in every Kong instance separately.|
|ordering|object|false|none|none|
|protocols|[string]|false|none|A list of the request protocols that will trigger this plugin. The default value, as well as the possible values allowed on this field, may change depending on the plugin type. For example, plugins that only work in stream mode will only support `"tcp"` and `"tls"`.|
|route|object|false|none|If set, the plugin will only activate when receiving requests via the specified route. Leave unset for the plugin to activate regardless of the route being used.|
|» id|string|false|none|none|
|service|object|false|none|If set, the plugin will only activate when receiving requests via one of the routes belonging to the specified service. Leave unset for the plugin to activate regardless of the service being matched.|
|» id|string|false|none|none|
|tags|[string]|false|none|An optional set of strings associated with the Plugin for grouping and filtering.|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|

<h2 id="tocS_Route">Route</h2>
<!-- backwards compatibility -->
<a id="schemaroute"></a>
<a id="schema_Route"></a>
<a id="tocSroute"></a>
<a id="tocsroute"></a>

```json
{
  "hosts": [
    "foo.example.com",
    "bar.example.com"
  ],
  "id": "56c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "example-route",
  "paths": [
    "/v1",
    "/v2"
  ],
  "service": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  }
}

```

Route

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|destinations|[object]|false|none|A list of IP destinations of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|» default|any|false|none|none|
|headers|object|false|none|One or more lists of values indexed by header name that will cause this route to match if present in the request. The `Host` header cannot be used with this hosts should be specified using the `hosts` attribute. When `headers` contains only one value and that value starts with the special prefix `~*`, the value is interpreted as a regular expression.|
|hosts|[string]|false|none|A list of domain names that match this route. Note that the hosts value is case sensitive.|
|https_redirect_status_code|integer|false|none|The status code Kong responds with when all properties of a route match except the protocol i.e. if the protocol of the request is `HTTP` instead of `HTTPS`. `Location` header is injected by Kong if the field is set to 301, 302, 307 or 308. This config applies only if the route is configured to only accept the `https` protocol.|
|id|string|false|none|none|
|methods|[string]|false|none|A list of HTTP methods that match this route.|
|name|string|false|none|The name of the route. Route names must be unique, and they are case sensitive. For example, there can be two different routes named "test" and "Test".|
|path_handling|string|false|none|Controls how the service path, route path and requested path are combined when sending a request to the upstream. See above for a detailed description of each behavior.|
|paths|[string]|false|none|A list of paths that match this route.|
|preserve_host|boolean|false|none|When matching a route via one of the `hosts` domain names, use the request `Host` header in the upstream request headers. If set to `false`, the upstream `Host` header will be that of the services `host`.|
|protocols|[string]|false|none|An array of the protocols this route should allow. See the [route Object](#route-object) section for a list of accepted protocols. When set to only `"https"`, HTTP requests are answered with an upgrade error. When set to only `"http"`, HTTPS requests are answered with an error.|
|regex_priority|integer|false|none|A number used to choose which route resolves a given request when several routes match it using regexes simultaneously. When two routes match the path and have the same `regex_priority`, the older one (lowest `created_at`) is used. Note that the priority for non-regex routes is different (longer non-regex routes are matched before shorter ones).|
|request_buffering|boolean|false|none|Whether to enable request body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that receive data with chunked transfer encoding.|
|response_buffering|boolean|false|none|Whether to enable response body buffering or not. With HTTP 1.1, it may make sense to turn this off on services that send data with chunked transfer encoding.|
|service|object|false|none|The service this route is associated to. This is where the route proxies traffic to.|
|» id|string|false|none|none|
|snis|[string]|false|none|A list of SNIs that match this route when using stream routing.|
|sources|[object]|false|none|A list of IP sources of incoming connections that match this route when using stream routing. Each entry is an object with fields "ip" (optionally in CIDR range notation) and/or "port".|
|» default|any|false|none|none|
|strip_path|boolean|false|none|When matching a route via one of the `paths`, strip the matching prefix from the upstream request URL.|
|tags|[string]|false|none|An optional set of strings associated with the route for grouping and filtering.|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|

<h2 id="tocS_SNI">SNI</h2>
<!-- backwards compatibility -->
<a id="schemasni"></a>
<a id="schema_SNI"></a>
<a id="tocSsni"></a>
<a id="tocssni"></a>

```json
{
  "certificate": {
    "id": "bd380f99-659d-415e-b0e7-72ea05df3218"
  },
  "id": "36c4566c-14cc-4132-9011-4139fcbbe50a",
  "name": "some.example.org"
}

```

An SNI object represents a many-to-one mapping of hostnames to a certificate. That is, a certificate object can have many hostnames associated with it; when Kong receives an SSL request, it uses the SNI field in the Client Hello to lookup the certificate object based on the SNI associated with the certificate.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|certificate|object|false|none|The id (a UUID) of the certificate with which to associate the SNI hostname. The Certificate must have a valid private key associated with it to be used by the SNI object.|
|» id|string|false|none|none|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|id|string|false|none|none|
|name|string|false|none|The SNI name to associate with the given certificate.|
|tags|[string]|false|none|An optional set of strings associated with the SNIs for grouping and filtering.|

<h2 id="tocS_Service">Service</h2>
<!-- backwards compatibility -->
<a id="schemaservice"></a>
<a id="schema_Service"></a>
<a id="tocSservice"></a>
<a id="tocsservice"></a>

```json
{
  "host": "example.internal",
  "id": "49fd316e-c457-481c-9fc7-8079153e4f3c",
  "name": "example-service",
  "path": "/",
  "port": 80,
  "protocol": "http"
}

```

Service

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ca_certificates|[string]|false|none|Array of `CA Certificate` object UUIDs that are used to build the trust store while verifying upstream server's TLS certificate. If set to `null` when Nginx default is respected. If default CA list in Nginx are not specified and TLS verification is enabled, then handshake with upstream server will always fail (because no CA are trusted).|
|client_certificate|object|false|none|Certificate to be used as client certificate while TLS handshaking to the upstream server.|
|» id|string|false|none|none|
|connect_timeout|integer|false|none|The timeout in milliseconds for establishing a connection to the upstream server.|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|enabled|boolean|false|none|Whether the service is active. If set to `false`, the proxy behavior will be as if any routes attached to it do not exist (404).|
|host|string|false|none|The host of the upstream server. Note that the host value is case sensitive.|
|id|string|false|none|none|
|name|string|false|none|The service name.|
|path|string|false|none|The path to be used in requests to the upstream server.|
|port|integer|false|none|The upstream server port.|
|protocol|string|false|none|The protocol used to communicate with the upstream.|
|read_timeout|integer|false|none|The timeout in milliseconds between two successive read operations for transmitting a request to the upstream server.|
|retries|integer|false|none|The number of retries to execute upon failure to proxy.|
|tags|[string]|false|none|An optional set of strings associated with the service for grouping and filtering.|
|tls_verify|boolean¦null|false|none|Whether to enable verification of upstream server TLS certificate. If set to `null`, then the Nginx default is respected.|
|tls_verify_depth|integer|false|none|Maximum depth of chain while verifying Upstream server's TLS certificate. If set to `null`, then the Nginx default is respected.'|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|
|url|string|false|none|Helper field to set `protocol`, `host`, `port` and `path` using a URL. This field is write-only and is not returned in responses.|
|write_timeout|integer|false|none|The timeout in milliseconds between two successive write operations for transmitting a request to the upstream server.|

<h2 id="tocS_Target">Target</h2>
<!-- backwards compatibility -->
<a id="schematarget"></a>
<a id="schema_Target"></a>
<a id="tocStarget"></a>
<a id="tocstarget"></a>

```json
{
  "id": "089292a7-ba3d-4d88-acf0-97b4b2e2621a",
  "target": "203.0.113.42",
  "upstream": {
    "id": "5f1d7e76-2fed-4806-a6af-869984f025cb"
  },
  "weight": 100
}

```

A target is an ip address/hostname with a port that identifies an instance of a backend service. Every upstream can have many targets, and the targets can be dynamically added, modified, or deleted. Changes take effect on the fly. To disable a target, post a new one with `weight=0`; alternatively, use the `DELETE` convenience method to accomplish the same. The current target object definition is the one with the latest `created_at`.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|created_at|number|false|none|Unix epoch when the resource was created.|
|id|string|false|none|The unique identifier or the name of the upstream for which to update the target.|
|tags|[string]|false|none|An optional set of strings associated with the Target for grouping and filtering.|
|target|string|false|none|The target address (ip or hostname) and port. If the hostname resolves to an SRV record, the `port` value will be overridden by the value from the DNS record.|
|upstream|object|false|none|The unique identifier or the name of the upstream for which to update the target.|
|» id|string|false|none|none|
|weight|integer|false|none|The weight this target gets within the upstream loadbalancer (`0`-`65535`). If the hostname resolves to an SRV record, the `weight` value will be overridden by the value from the DNS record.|

<h2 id="tocS_Upstream">Upstream</h2>
<!-- backwards compatibility -->
<a id="schemaupstream"></a>
<a id="schema_Upstream"></a>
<a id="tocSupstream"></a>
<a id="tocsupstream"></a>

```json
{
  "algorithm": "round-robin",
  "hash_fallback": "none",
  "hash_on": "none",
  "hash_on_cookie_path": "/",
  "healthchecks": {
    "active": {
      "concurrency": 10,
      "healthy": {
        "http_statuses": [
          200,
          302
        ],
        "interval": 0,
        "successes": 0
      },
      "http_path": "/",
      "https_verify_certificate": true,
      "timeout": 1,
      "type": "http",
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          404,
          500,
          501,
          502,
          503,
          504,
          505
        ],
        "interval": 0,
        "tcp_failures": 0,
        "timeouts": 0
      }
    },
    "passive": {
      "healthy": {
        "http_statuses": [
          200,
          201,
          202,
          203,
          204,
          205,
          206,
          207,
          208,
          226,
          300,
          301,
          302,
          303,
          304,
          305,
          306,
          307,
          308
        ],
        "successes": 0
      },
      "type": "http",
      "unhealthy": {
        "http_failures": 0,
        "http_statuses": [
          429,
          500,
          503
        ],
        "tcp_failures": 0,
        "timeouts": 0
      }
    },
    "threshold": 0
  },
  "id": "6eed5e9c-5398-4026-9a4c-d48f18a2431e",
  "name": "api.example.internal",
  "slots": 10000
}

```

The upstream object represents a virtual hostname and can be used to loadbalance incoming requests over multiple services (targets). So for example an upstream named `service.v1.xyz` for a service object whose `host` is `service.v1.xyz`. Requests for this service would be proxied to the targets defined within the upstream. An upstream also includes a health check, which is able to enable and disable targets based on their ability or inability to serve requests. The configuration for the health checker is stored in the upstream object, and applies to all of its targets.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|algorithm|string|false|none|Which load balancing algorithm to use.|
|client_certificate|object|false|none|If set, the certificate to be used as client certificate while TLS handshaking to the upstream server.|
|» id|string|false|none|none|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|hash_fallback|string|false|none|What to use as hashing input if the primary `hash_on` does not return a hash (eg. header is missing, or no Consumer identified). Not available if `hash_on` is set to `cookie`.|
|hash_fallback_header|string|false|none|The header name to take the value from as hash input. Only required when `hash_fallback` is set to `header`.|
|hash_fallback_query_arg|string|false|none|The name of the query string argument to take the value from as hash input. Only required when `hash_fallback` is set to `query_arg`.|
|hash_fallback_uri_capture|string|false|none|The name of the route URI capture to take the value from as hash input. Only required when `hash_fallback` is set to `uri_capture`.|
|hash_on|string|false|none|What to use as hashing input. Using `none` results in a weighted-round-robin scheme with no hashing.|
|hash_on_cookie|string|false|none|The cookie name to take the value from as hash input. Only required when `hash_on` or `hash_fallback` is set to `cookie`. If the specified cookie is not in the request, Kong will generate a value and set the cookie in the response.|
|hash_on_cookie_path|string|false|none|The cookie path to set in the response headers. Only required when `hash_on` or `hash_fallback` is set to `cookie`.|
|hash_on_header|string|false|none|The header name to take the value from as hash input. Only required when `hash_on` is set to `header`.|
|hash_on_query_arg|string|false|none|The name of the query string argument to take the value from as hash input. Only required when `hash_on` is set to `query_arg`.|
|hash_on_uri_capture|string|false|none|The name of the route URI capture to take the value from as hash input. Only required when `hash_on` is set to `uri_capture`.|
|healthchecks|object|false|none|none|
|» active|object|false|none|none|
|»» concurrency|integer|false|none|none|
|»» headers|object|false|none|none|
|»» healthy|object|false|none|none|
|»»» http_statuses|[integer]|false|none|none|
|»»» interval|number|false|none|none|
|»»» successes|integer|false|none|none|
|»» http_path|string|false|none|none|
|»» https_sni|string|false|none|none|
|»» https_verify_certificate|boolean|false|none|none|
|»» timeout|number|false|none|none|
|»» type|string|false|none|none|
|»» unhealthy|object|false|none|none|
|»»» http_failures|integer|false|none|none|
|»»» http_statuses|[integer]|false|none|none|
|»»» interval|number|false|none|none|
|»»» tcp_failures|integer|false|none|none|
|»»» timeouts|integer|false|none|none|
|» passive|object|false|none|none|
|»» healthy|object|false|none|none|
|»»» http_statuses|[integer]|false|none|none|
|»»» successes|integer|false|none|none|
|»» type|string|false|none|none|
|»» unhealthy|object|false|none|none|
|»»» http_failures|integer|false|none|none|
|»»» http_statuses|[integer]|false|none|none|
|»»» tcp_failures|integer|false|none|none|
|»»» timeouts|integer|false|none|none|
|» threshold|number|false|none|none|
|host_header|string|false|none|The hostname to be used as `Host` header when proxying requests through Kong.|
|id|string|false|none|none|
|name|string|false|none|This is a hostname, which must be equal to the `host` of a service.|
|slots|integer|false|none|The number of slots in the load balancer algorithm. If `algorithm` is set to `round-robin`, this setting determines the maximum number of slots. If `algorithm` is set to `consistent-hashing`, this setting determines the actual number of slots in the algorithm. Accepts an integer in the range `10`-`65536`.|
|tags|[string]|false|none|An optional set of strings associated with the Upstream for grouping and filtering.|
|use_srv_name|boolean|false|none|If set, the balancer will use SRV hostname(if DNS Answer has SRV record) as the proxy upstream `Host`.|

<h2 id="tocS_Vault">Vault</h2>
<!-- backwards compatibility -->
<a id="schemavault"></a>
<a id="schema_Vault"></a>
<a id="tocSvault"></a>
<a id="tocsvault"></a>

```json
{
  "config": {
    "prefix": "vaults.config.resurrect_ttl"
  },
  "description": "environment variable based vault",
  "id": "2747d1e5-8246-4f65-a939-b392f1ee17f8",
  "name": "env",
  "tags": [
    "foo",
    "bar"
  ]
}

```

Vault entities are used to configure different Vault connectors. Examples of Vaults are Environment Variables, Hashicorp Vault and AWS Secrets Manager. Configuring a Vault allows referencing the secrets with other entities. For example a certificate entity can store a reference to a certificate and key, stored in a vault, instead of storing the certificate and key within the entity. This allows a proper separation of secrets and configuration and prevents secret sprawl.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|config|object|false|none|The configuration properties for the Vault which can be found on the [vaults' documentation page](https://docs.konghq.com/gateway/latest/kong-enterprise/secrets-management/advanced-usage/).|
|» prefix|string|false|none|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|created_at|integer|false|none|Unix epoch when the resource was created.|
|description|string|false|none|The description of the Vault entity.|
|id|string|false|none|none|
|name|string|false|none|The name of the Vault thats going to be added. Currently, the Vault implementation must be installed in every Kong instance.|
|prefix|string|false|none|The unique prefix (or identifier) for this Vault configuration. The prefix is used to load the right Vault configuration and implementation when referencing secrets with the other entities.|
|tags|[string]|false|none|An optional set of strings associated with the Vault for grouping and filtering.|
|updated_at|integer|false|none|Unix epoch when the resource was last updated.|

#### Enumerated Values

|Property|Value|
|---|---|
|prefix|vaults.config.resurrect_ttl|
|prefix|vaults.config.neg_ttl|
|prefix|vaults.config.ttl|

<h2 id="tocS_pagination-offset-response">pagination-offset-response</h2>
<!-- backwards compatibility -->
<a id="schemapagination-offset-response"></a>
<a id="schema_pagination-offset-response"></a>
<a id="tocSpagination-offset-response"></a>
<a id="tocspagination-offset-response"></a>

```json
"string"

```

Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Offset is used to paginate through the API. Provide this value to the next list operation to fetch the next page|

