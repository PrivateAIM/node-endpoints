# Node endpoint collection

Collection of all HTTP endpoints for every service hosted on the Node side.

## With FastAPI

Create a new directory in this repository for your project.
Start your service.
FastAPI will automatically provide an OpenAPI spec that you can download into your directory.

```
$ mkdir my-project
$ cd my-project
$ curl -o openapi.json http://0.0.0.0:8000/openapi.json
```

If you haven't already, install [widdershins](https://www.npmjs.com/package/widdershins).
You need a Node.js and NPM installation on your system.
Once that's complete, run `npm i -g widdershins@4.0.1`.
In your directory, run the following command to generate a Markdown file from your OpenAPI spec.

```
$ widdershins openapi.json -c -l -o openapi.md
```

Commit your changes and push them to this repository.
