---
title: FastAPI v0.1.0
language_tabs: []
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="fastapi">FastAPI v0.1.0</h1>

> Scroll down for example requests and responses.

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="fastapi-default">Default</h1>

## do_healthcheck_healthz_get

<a id="opIddo_healthcheck_healthz_get"></a>

`GET /healthz`

*Do Healthcheck*

> Example responses

> 200 Response

```json
null
```

<h3 id="do_healthcheck_healthz_get-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|

<h3 id="do_healthcheck_healthz_get-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="fastapi-upload">upload</h1>

## upload_to_remote_upload__put

<a id="opIdupload_to_remote_upload__put"></a>

`PUT /upload/`

*Upload To Remote*

> Body parameter

```yaml
file: string

```

<h3 id="upload_to_remote_upload__put-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Body_upload_to_remote_upload__put](#schemabody_upload_to_remote_upload__put)|true|none|

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

<h3 id="upload_to_remote_upload__put-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Successful Response|None|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
HTTPBearer
</aside>

<h1 id="fastapi-scratch">scratch</h1>

## upload_to_scratch_scratch__put

<a id="opIdupload_to_scratch_scratch__put"></a>

`PUT /scratch/`

*Upload To Scratch*

> Body parameter

```yaml
file: string

```

<h3 id="upload_to_scratch_scratch__put-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Body_upload_to_scratch_scratch__put](#schemabody_upload_to_scratch_scratch__put)|true|none|

> Example responses

> 200 Response

```json
{
  "url": "http://example.com"
}
```

<h3 id="upload_to_scratch_scratch__put-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|[ScratchUploadResponse](#schemascratchuploadresponse)|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
HTTPBearer
</aside>

## read_from_scratch_scratch__object_id__get

<a id="opIdread_from_scratch_scratch__object_id__get"></a>

`GET /scratch/{object_id}`

*Read From Scratch*

<h3 id="read_from_scratch_scratch__object_id__get-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|object_id|path|string(uuid)|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="read_from_scratch_scratch__object_id__get-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="read_from_scratch_scratch__object_id__get-responseschema">Response Schema</h3>

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
HTTPBearer
</aside>

# Schemas

<h2 id="tocS_Body_upload_to_remote_upload__put">Body_upload_to_remote_upload__put</h2>
<!-- backwards compatibility -->
<a id="schemabody_upload_to_remote_upload__put"></a>
<a id="schema_Body_upload_to_remote_upload__put"></a>
<a id="tocSbody_upload_to_remote_upload__put"></a>
<a id="tocsbody_upload_to_remote_upload__put"></a>

```json
{
  "file": "string"
}

```

Body_upload_to_remote_upload__put

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|file|string(binary)|true|none|none|

<h2 id="tocS_Body_upload_to_scratch_scratch__put">Body_upload_to_scratch_scratch__put</h2>
<!-- backwards compatibility -->
<a id="schemabody_upload_to_scratch_scratch__put"></a>
<a id="schema_Body_upload_to_scratch_scratch__put"></a>
<a id="tocSbody_upload_to_scratch_scratch__put"></a>
<a id="tocsbody_upload_to_scratch_scratch__put"></a>

```json
{
  "file": "string"
}

```

Body_upload_to_scratch_scratch__put

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

