---
description: >-
  Learn how to start a new GraphQL API project from scratch using Node.js and
  JavaScript.
---

# Getting Started

## Step 1: Create Node.js project

During this step, you want to ensure that at least the following things are taken care of:

* You can use the latest \(modern\) JavaScript syntax \(most likely using [Babel](https://babeljs.io/) \)
* You can launch the app by running `yarn start`  which is a commonly used convention.
* When you make changes to the source code, the app \(API\) automatically restarts \(see [Nodemon](https://github.com/remy/nodemon)\).

![Basic Node.js project in VS Code ](.gitbook/assets/graphql-example-01.png)

Assuming that you already have [**Node.js**](https://nodejs.org) and [**Yarn**](https://yarnpkg.com) \(or, NPM\) installed,  bootstrap a new Node.js project by running:

```bash
# Creates package.json file in the root of the project's folder
$ yarn init

# Installs Express.js and GraphQL.js NPM modules (runtime dependencies)
$ yarn add graphql express express-graphql

# Installs Nodemon and Babel (dev dependencies)
$ yarn add nodemon --dev
$ yarn add @babel/core @babel/cli @babel/node @babel/preset-env --dev --exact
```

Create `.babelrc` file containing Babel configuration. Add `"start"` script to the `package.json` file:

{% code-tabs %}
{% code-tabs-item title="package.json" %}
```yaml
{
  "name": "api",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "express": "^4.17.1",
    "express-graphql": "^0.9.0",
    "graphql": "^14.5.4"
  },
  "devDependencies": {
    "@babel/cli": "7.6.0",
    "@babel/core": "7.6.0",
    "@babel/node": "7.6.1",
    "@babel/preset-env": "7.6.0",
    "nodemon": "1.19.2"
  },
  "scripts": {
    "start": "nodemon src/index.js localhost 8080 --exec ./node_modules/.bin/babel-node"
  }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title=".babelrc" %}
```yaml
{
  "presets": [
    "@babel/preset-env"
  ]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
Note that the actual versions of all the listed dependencies above may differ from what you would end up having in your `package.json` file, but it shouldn't  be a problem.
{% endhint %}

Finally, create `src/index.js` file that will serve as an entry point to the \([Express.js](http://expressjs.com/)\) app:

```javascript
import express from 'express';
import graphql from 'express-graphql';

const app = express();

app.use('/', graphql({
  schema: null, // TODO: Implement the GraphQL schema
  graphiql: process.env.NODE_ENV !== 'production',
}));

app.listen(process.env.PORT || 8080);
```

At this point, when you launch the app by running `yarn start` and navigate to `http://localhost:8080/` in the browser's window, you must be able to see the following: 

![](.gitbook/assets/graphql-example-02.png)

{% hint style="success" %}
**Congratulations!** This means that the step \#1 is complete and we can move on to the next one — creating our first GraphQL schema required by the **`express-graphql`** middleware.
{% endhint %}

## Step 2: Create GraphQL schema

Basically, you create an GraphQL API by describing the hierarchy \(schema\) of all the \(query\) fields and mutations that the API must support.

Imagine that we need to fetch some OS-related info from the server using this query:

{% tabs %}
{% tab title="GraphQL Query" %}
```graphql
query {
  environment {
    arch
    platform
    uptime
  }
}
```
{% endtab %}

{% tab title="GraphQL API Response" %}
```javascript
{
  data: {
    environment: {
      arch: 'x64',
      platform: 'linux',
      uptime: 11.256
    }
  }
}
```
{% endtab %}
{% endtabs %}

It's asking for `arch`, `platform`, and `uptime` values grouped under the top-level `envrionment` field.

For this particular API we would need to implement just one GraphQL type \("Environment"\), and one top-level query field "environment". But since, we're planning to add more types and query fields later on, it would be good idea to group them under `src/types` and `src/queires`  folders, plus you would add `src/schema.js` file with the "schema" GraphQL object type.

![](.gitbook/assets/graphql-example-03.png)

...

