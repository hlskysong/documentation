# Handle Error
Elysia catches all the errors thrown in the handler, classifies the error code, and pipes them to `onError` middleware.

```typescript
new Elysia()
    .get('/', () => {
        throw new Error('Server is during maintainance')
        
        return 'unreachable'
    })
    .onError((code, error) => {
        return new Response(error.toString())
    })
```

With `onError` you can catch and transform the error into your custom error message.

For example, returning custom 404 messages:
```typescript
# Custom 404
You can define custom 404 using `onError` hook:
```typescript
import { Elysia } from 'elysia'

new Elysia()
    .onError((code, error) => {
        if (code === 'NOT_FOUND')
            return new Response('Not Found :(', {
                status: 404
            })
    })
    .listen(8080)
```

## Error Code
Elysia error code consists of:
'NOT_FOUND'
'INTERNAL_SERVER_ERROR'
'BODY_LIMIT'
'VALIDATION'
'UNKNOWN'

By default, user thrown error code is `unknown`.

::: tip
If no error response is returned, the error will be returned using `error.name`.
:::