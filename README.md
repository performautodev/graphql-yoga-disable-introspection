# graphql-disable-introspection fro graphql-yoga
Disable introspection queries in GraphQL with a simple validation rule. Queries that contain `__schema` or `__type` will fail validation with this rule. For example, the following queries will be rejected:

QUERY

```graphql
query {
  __schema {
    queryType {
      name
    }
  }
}
```

[BEFORE]  RESULT QUERY :

```graphql
{
    "data": {
        "__schema": {
            "types": [
                {
                    "name": "Query"
                },
                {
                    "name": "String"
                },
               ...
            ]
        }
    }
}
```

[AFTER] RESULT QUERY :

```graphql
{
    "errors": [
        {
            "message": "GraphQL introspection is not allowed, but the query contained __schema or __type",
            "locations": [
                {
                    "line": 2,
                    "column": 3
                }
            ]
        }
    ]
}
```


## Usage

The package can be installed from npm

```sh
npm install -save graphql-disable-introspection
```

It exports a single validation rule which you can pass to your node GraphQL server with the `validationRules` argument. 

Here's an example for `graphql-yoga`:

```diff
import express from 'express';
+ const NoIntrospection = require("graphql-disable-introspection");


// Start server
server.start(
  {
+   validationRules: [NoIntrospection],
    playground: false,
    port: 4000,
...

+ i changed the message 'GraphQL introspection is not allowed'
this repo moded | Forked from https://www.npmjs.com/package/graphql-disable-introspection
