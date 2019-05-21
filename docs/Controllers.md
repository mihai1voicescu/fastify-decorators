<h1 align="center">Fastify decorators</h1>

## Bootstrap controllers
Let's imagine that:
- We already have the directory named `handlers` which contains all our handlers
- Each handler contains `.handler.` in it's name.

To make it works without manual loading we can use `bootstrap` method:
```typescript
import { bootstrap } from 'fastify-decorators';
import { join } from 'path';

// Require the framework and instantiate it
const instance = require('fastify')();

// Define bootstrap options
const bootstrapOptions = {
    // This option defines which directory should be scanned for handlers
    controllersDirectory: join(__dirname, `controllers`),

    // This option defines which pattern should file match
    controllersMask: /\.controller\./
};

// Register our bootstrap with options
instance.register(bootstrap, bootstrapOptions);
```

## Writing controllers

The Fastify decorators module exports set of decorators to implement controllers with multiple handlers and hooks.

### Base class

Every controller should be decorated with `@Controller` decorator and exported:
```typescript
import { Controller } from 'fastify-decorators';

@Controller({route: '/'})
class SimpleController {
}

export = SimpleController;
```

### Handlers

To mark controller method as handler you have to use one of the following decorators:
- `ALL`
- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`
- `OPTIONS`
- `HEAD`

*example*:
```typescript
import { Controller, GET } from 'fastify-decorators';

@Controller({route: '/'})
class SimpleController {
    @GET({url: '/'})
    async getHandler(request, reply) {
        return 'Hello world!'
    }
}

export = SimpleController;
```

Decorators accept `RouteConfig` with follow fields:

| name    | type            | required | description                                      |
|---------|-----------------|:--------:|--------------------------------------------------|
| url     | `string`        | yes      | Route url which will be passed to Fastify        |
| options | [`RouteConfig`] | no       | Config for route which will be passed to Fastify |

**NOTE**: This decorators can't be mixed and you can use only one decorator per method.

### Hooks

There are also decorator which allows to use [Fastify Hooks]:
```typescript
import { Controller, Hook } from 'fastify-decorators';

@Controller({route: '/'})
class SimpleController {
    @Hook('onSend')
    async (request, reply) {
        reply.removeHeader('X-Powered-By');
    }
}

export = SimpleController;
```

[Fastify Hooks]: https://github.com/fastify/fastify/blob/master/docs/Hooks.md