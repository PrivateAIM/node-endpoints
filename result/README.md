---
title: FLAME Node Result Service v0.1.0
language_tabs: []
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="flame-node-result-service">FLAME Node Result Service v0.1.0</h1>

> Scroll down for example requests and responses.

The FLAME Node Result Service is responsible for handling result files for federated analyses within FLAME.
It uses a local object storage to store intermediate files, as well as to enqueue files for upload to the FLAME Hub.

# Setup

You will need access to a MinIO instance and an identification provider that offers a JWKS endpoint for the access
tokens it issues.

For manual installation, you will need Python 3.10 or higher and [Poetry](https://python-poetry.org/) installed.
Clone the repository and run `poetry install` in the root directory.
Create a copy of `.env.example`, name it `.env` and configure to your needs.
Finally, use the command line tool `flame-result` to start the service.

```
$ git clone https://github.com/PrivateAIM/node-result-service.git
$ cd node-result-service
$ poetry install
$ cp .env.example .env
$ poetry shell
$ flame-result server
```

Alternatively, if you're using
Docker, [pull a recent image from the GitHub container registry](https://github.com/PrivateAIM/node-result-service/pkgs/container/node-result-service).
Pass in the configuration options using `-e` flags and forward port 8080 from your host to the container.

```
$ docker run --rm -p 8080:8080 -e HUB__AUTH_USERNAME=admin \
    -e HUB__AUTH_PASSWORD=super_secret \
    -e MINIO__ENDPOINT=localhost:9000 \
    -e MINIO__ACCESS_KEY=admin \
    -e MINIO__SECRET_KEY=super_secret \
    -e MINIO__BUCKET=flame \
    -e MINIO__USE_SSL=false \
    -e OIDC__CERTS_URL="http://my.idp.org/realms/flame/protocol/openid-connect/certs" \
    ghcr.io/privateaim/node-result-service:sha-c1970cf
```

# Configuration

The following table shows all available configuration options.

| **Environment variable**   | **Description**                                          | **Default**                 | **Required** |
|----------------------------|----------------------------------------------------------|-----------------------------|:------------:|
| HUB__API_BASE_URL          | Base URL for the FLAME Hub API                           | https://api.privateaim.net  |              |
| HUB__AUTH_BASE_URL         | Base URL for the FLAME Auth API                          | https://auth.privateaim.net |              |
| HUB__AUTH_USERNAME         | Username to use for obtaining access tokens              |                             |      x       |
| HUB__AUTH_PASSWORD         | Password to use for obtaining access tokens              |                             |      x       |
| MINIO__ENDPOINT            | MinIO S3 API endpoint (without scheme)                   |                             |      x       |
| MINIO__ACCESS_KEY          | Access key for interacting with MinIO S3 API             |                             |      x       |
| MINIO__SECRET_KEY          | Secret key for interacting with MinIO S3 API             |                             |      x       |
| MINIO__BUCKET              | Name of S3 bucket to store result files in               |                             |      x       |
| MINIO__REGION              | Region of S3 bucket to store result files in             | us-east-1                   |              |
| MINIO__USE_SSL             | Flag for en-/disabling encrypted traffic to MinIO S3 API | 0                           |              |
| OIDC__CERTS_URL            | URL to OIDC-complaint JWKS endpoint for validating JWTs  |                             |      x       |
| OIDC__CLIENT_ID_CLAIM_NAME | JWT claim to identify authenticated requests with        | client_id                   |              |

## Note on running tests

When running tests, environment variables must be overwritten by prefixing them with `PYTEST__`.
OIDC does not need to be configured, since an OIDC-compatible endpoint will be spawned alongside the tests that are
being run.
A [pre-generated keypair](tests/assets/keypair.pem) is used for this purpose.
This allows all tests to generate valid JWTs as well as the service to validate them.
The keypair is for development purposes only and should not be used in a productive setting.
Therefore `pytest` should be invoked as follows.

```
$ PYTEST__MINIO__ENDPOINT="localhost:9000" \
    PYTEST__MINIO__ACCESS_KEY="admin" \
    PYTEST__MINIO__SECRET_KEY="s3cr3t_p4ssw0rd" \
    PYTEST__MINIO__BUCKET="flame" \
    PYTEST__HUB__AUTH_USERNAME="XXXXXXXX" \
    PYTEST__HUB__AUTH_PASSWORD="XXXXXXXX" pytest
```

Some tests need to be run against live infrastructure.
Since a proper test instance is not available yet, these tests are hidden behind a flag and are not explicitly run in
CI.
To run these tests, append `-m live` to the command above.

# License

The FLAME Node Result Service is released under the Apache 2.0 license.

License: <a href="https://www.apache.org/licenses/LICENSE-2.0.html">Apache 2.0</a>

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="flame-node-result-service-default">Default</h1>

## getHealth

<a id="opIdgetHealth"></a>

`GET /healthz`

*Check service readiness*

Check whether the service is ready to process requests. Responds with a 200 on success.

> Example responses

> 200 Response

```json
null
```

<h3 id="gethealth-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|

<h3 id="gethealth-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="flame-node-result-service-upload">upload</h1>

Upload files for submission to FLAME hub

## putResultFile

<a id="opIdputResultFile"></a>

`PUT /upload/`

*Upload file to submit to Hub*

Upload a file to the local S3 instance and send it to FLAME Hub in the background.
The request is successful if the file was uploaded to the local S3 instance.
Responds with a 204 on success.

This endpoint is to be used for submitting final results of a federated analysis.

Currently, there is no way of determining the status or progress of the upload to the FLAME Hub.

> Body parameter

```yaml
file: string

```

<h3 id="putresultfile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Body_putResultFile](#schemabody_putresultfile)|true|none|

> Example responses

> 422 Response

```json
{
  "detail": [
    {
      "loc": [
        "string"
      ],
      "msg": "string",
      "type": "string"
    }
  ]
}
```

<h3 id="putresultfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successful Response|None|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
HTTPBearer
</aside>

<h1 id="flame-node-result-service-scratch">scratch</h1>

Upload files to local object storage

## putIntermediateFile

<a id="opIdputIntermediateFile"></a>

`PUT /scratch/`

*Upload file to local object storage*

Upload a file to the local S3 instance.
The file is not forwarded to the FLAME hub.
Responds with a 200 on success and a link to the endpoint for fetching the uploaded file.

This endpoint is to be used for submitting intermediate results of a federated analysis.

> Body parameter

```yaml
file: string

```

<h3 id="putintermediatefile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Body_putIntermediateFile](#schemabody_putintermediatefile)|true|none|

> Example responses

> 200 Response

```json
{
  "url": "http://example.com"
}
```

<h3 id="putintermediatefile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|[ScratchUploadResponse](#schemascratchuploadresponse)|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
HTTPBearer
</aside>

## getIntermediateFile

<a id="opIdgetIntermediateFile"></a>

`GET /scratch/{object_id}`

*Get file from local object storage*

Get a file from the local S3 instance.
The file must have previously been uploaded using the PUT method of this endpoint.
Responds with a 200 on success and the requested file in the response body.

This endpoint is to be used for retrieving intermediate results of a federated analysis.

<h3 id="getintermediatefile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|object_id|path|string(uuid)|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="getintermediatefile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="getintermediatefile-responseschema">Response Schema</h3>

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
HTTPBearer
</aside>

# Schemas

<h2 id="tocS_Body_putIntermediateFile">Body_putIntermediateFile</h2>
<!-- backwards compatibility -->
<a id="schemabody_putintermediatefile"></a>
<a id="schema_Body_putIntermediateFile"></a>
<a id="tocSbody_putintermediatefile"></a>
<a id="tocsbody_putintermediatefile"></a>

```json
{
  "file": "string"
}

```

Body_putIntermediateFile

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|file|string(binary)|true|none|none|

<h2 id="tocS_Body_putResultFile">Body_putResultFile</h2>
<!-- backwards compatibility -->
<a id="schemabody_putresultfile"></a>
<a id="schema_Body_putResultFile"></a>
<a id="tocSbody_putresultfile"></a>
<a id="tocsbody_putresultfile"></a>

```json
{
  "file": "string"
}

```

Body_putResultFile

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|file|string(binary)|true|none|none|

<h2 id="tocS_HTTPValidationError">HTTPValidationError</h2>
<!-- backwards compatibility -->
<a id="schemahttpvalidationerror"></a>
<a id="schema_HTTPValidationError"></a>
<a id="tocShttpvalidationerror"></a>
<a id="tocshttpvalidationerror"></a>

```json
{
  "detail": [
    {
      "loc": [
        "string"
      ],
      "msg": "string",
      "type": "string"
    }
  ]
}

```

HTTPValidationError

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|detail|[[ValidationError](#schemavalidationerror)]|false|none|none|

<h2 id="tocS_ScratchUploadResponse">ScratchUploadResponse</h2>
<!-- backwards compatibility -->
<a id="schemascratchuploadresponse"></a>
<a id="schema_ScratchUploadResponse"></a>
<a id="tocSscratchuploadresponse"></a>
<a id="tocsscratchuploadresponse"></a>

```json
{
  "url": "http://example.com"
}

```

ScratchUploadResponse

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|url|string(uri)|true|none|none|

<h2 id="tocS_ValidationError">ValidationError</h2>
<!-- backwards compatibility -->
<a id="schemavalidationerror"></a>
<a id="schema_ValidationError"></a>
<a id="tocSvalidationerror"></a>
<a id="tocsvalidationerror"></a>

```json
{
  "loc": [
    "string"
  ],
  "msg": "string",
  "type": "string"
}

```

ValidationError

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|loc|[anyOf]|true|none|none|

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|integer|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|msg|string|true|none|none|
|type|string|true|none|none|

