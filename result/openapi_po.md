---
title: FLAME PO v0.1.0
language_tabs: []
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="flame-po">FLAME PO v0.1.0</h1>

> Scroll down for example requests and responses.

<h1 id="flame-po-default">Default</h1>

## create_analysis_po__analysis_id__post

<a id="opIdcreate_analysis_po__analysis_id__post"></a>

`POST /po/{analysis_id}`

*Create Analysis*

<h3 id="create_analysis_po__analysis_id__post-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|analysis_id|path|string|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="create_analysis_po__analysis_id__post-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="create_analysis_po__analysis_id__post-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## retrieve_logs_po__analysis_id__logs_get

<a id="opIdretrieve_logs_po__analysis_id__logs_get"></a>

`GET /po/{analysis_id}/logs`

*Retrieve Logs*

<h3 id="retrieve_logs_po__analysis_id__logs_get-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|analysis_id|path|string|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="retrieve_logs_po__analysis_id__logs_get-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="retrieve_logs_po__analysis_id__logs_get-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## get_status_po__analysis_id__status_get

<a id="opIdget_status_po__analysis_id__status_get"></a>

`GET /po/{analysis_id}/status`

*Get Status*

<h3 id="get_status_po__analysis_id__status_get-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|analysis_id|path|string|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="get_status_po__analysis_id__status_get-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="get_status_po__analysis_id__status_get-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## get_pods_po__analysis_id__pods_get

<a id="opIdget_pods_po__analysis_id__pods_get"></a>

`GET /po/{analysis_id}/pods`

*Get Pods*

<h3 id="get_pods_po__analysis_id__pods_get-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|analysis_id|path|string|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="get_pods_po__analysis_id__pods_get-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="get_pods_po__analysis_id__pods_get-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## stop_analysis_po__analysis_id__stop_put

<a id="opIdstop_analysis_po__analysis_id__stop_put"></a>

`PUT /po/{analysis_id}/stop`

*Stop Analysis*

<h3 id="stop_analysis_po__analysis_id__stop_put-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|analysis_id|path|string|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="stop_analysis_po__analysis_id__stop_put-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="stop_analysis_po__analysis_id__stop_put-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## delete_analysis_po__analysis_id__delete_delete

<a id="opIddelete_analysis_po__analysis_id__delete_delete"></a>

`DELETE /po/{analysis_id}/delete`

*Delete Analysis*

<h3 id="delete_analysis_po__analysis_id__delete_delete-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|analysis_id|path|string|true|none|

> Example responses

> 200 Response

```json
null
```

<h3 id="delete_analysis_po__analysis_id__delete_delete-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Validation Error|[HTTPValidationError](#schemahttpvalidationerror)|

<h3 id="delete_analysis_po__analysis_id__delete_delete-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

## health_po_healthz_get

<a id="opIdhealth_po_healthz_get"></a>

`GET /po/healthz`

*Health*

> Example responses

> 200 Response

```json
null
```

<h3 id="health_po_healthz_get-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Successful Response|Inline|

<h3 id="health_po_healthz_get-responseschema">Response Schema</h3>

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

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

