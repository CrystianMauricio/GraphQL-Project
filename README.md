# GraphQL.js

### Using GraphQL.js

Install GraphQL.js from npm

With `npm`:

```sh
npm install --save graphql
```

With `yarn`:

```sh
yarn add graphql
```

With `bun`:

```sh
bun add graphql
```

GraphQL.js provides two important capabilities: building a type schema and
serving queries against that type schema.

First, build a GraphQL type schema which maps to your codebase.

```js
import {
  graphql,
  GraphQLSchema,
  GraphQLObjectType,
  GraphQLString,
} from 'graphql';

var schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
      hello: {
        type: GraphQLString,
        resolve() {
          return 'world';
        },
      },
    },
  }),
});
```

This defines a simple schema, with one type and one field, that resolves
to a fixed value. The `resolve` function can return a value, a promise,
or an array of promises. A more complex example is included in the top-level [tests](src/__tests__) directory.

Then, serve the result of a query against that type schema.

```js
var source = '{ hello }';

graphql({ schema, source }).then((result) => {
  // Prints
  // {
  //   data: { hello: "world" }
  // }
  console.log(result);
});
```

This runs a query fetching the one field defined. The `graphql` function will
first ensure the query is syntactically and semantically valid before executing
it, reporting errors otherwise.

```js
var source = '{ BoyHowdy }';

graphql({ schema, source }).then((result) => {
  // Prints
  // {
  //   errors: [
  //     { message: 'Cannot query field BoyHowdy on RootQueryType',
  //       locations: [ { line: 1, column: 3 } ] }
  //   ]
  // }
  console.log(result);
});
```

**Note**: Please don't forget to set `NODE_ENV=production` if you are running a production server. It will disable some checks that can be useful during development but will significantly improve performance.

### Want to ride the bleeding edge?

The `npm` branch in this repository is automatically maintained to be the last
commit to `main` to pass all tests, in the same form found on npm. It is
recommended to use builds deployed to npm for many reasons, but if you want to use
the latest not-yet-released version of graphql-js, you can do so by depending
directly on this branch:

```
npm install graphql@git://github.com/graphql/graphql-js.git#npm
```
