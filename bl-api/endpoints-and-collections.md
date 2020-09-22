[\< bl-api](./summary.md)

# Endpoints and Collections

Each Collection in the `MongoDB` database has a corresponding `Collection` in
`bl-api` under `src/collections`. A `Collection` describes what actions are
valid and what permission you need to access them. All actions are then
translated to a `Endpoint` that are accessable through HTTP.

* [What is a Collection?](#what-is-a-collection)
* [Collection settings](#collection-settings)
  * [Collection class](#collection-class)
  * [Endpoint class](#endpoint-class)
  * [Endpoint methods](#endpoint-methods)
  * [Endpoint hooks](#enpoint-hooks)
  * [Endpoint restrictions](#endpoint-restrictions)
  * [Endpoint operations](#enpoint-operations)


# What is a `Collection`?

To explain `Endpoint` and `Collection` in detail we can take the `bl-model`
class `Order` as an example. 

We want users to be able to: 
* Create orders
* Update orders
* Get orders
* Delete orders

To do this we could write everyting manually using `express`. Like so:
```
router.get('/order/:id', (req, res, next) => {
  // authentication code
  // database handling code
  // error handling code
  // response handling
});

router.delete('/order/:id', (req, res, next) => {
  // authentication code
  // database handling code
  // error handling code
  // response handling
});

router.patch('/order/:id', (req, res, next) => {
  // authentication code
  // database handling code
  // error handling code
  // response handling
});

router.post('/orders', (req, res, next) => {
  // authentication code
  // database handling code
  // error handling code
  // response handling
});

```
But this soon becomes repetative. And if you add one more class, let say `Item`
or `Customer-item` and `Booking` this code will be repetative and hard to
maintain.

### Keep it [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)
We want to keep the code as maintainable as possible. It should be easy to spin
up a new class like `Manager` or `Dog` without repeating yourself. That's why
we have `Collection`. 

### The solution is a simple spec file called `Collection` 

Instead of writing everything manually we can have a file named
`order.collection.ts` with the following contents:

```typescript
{
  collectionName: "orders",
  mongooseSchema: orderSchema,
  endpoints: [
    {
      method: "getId",
      restriction: {
        permissions: ["customer", "employee", "manager", "admin"],
        restricted: true
      }
    },
    {
      method: "delete",
      restriction: {
        permissions: ["admin"]
      }
    },
    {
      method: "patch",
      restriction: {
        permissions: ["customer", "employee", "manager", "admin"],
        restricted: true
      }
    },
    {
      method: "post",
      restriction: {
        permissions: ["customer", "employee", "manager", "admin"],
      }
    }
  ]
}
```

This little spec describes the `Order` `Collection`. Everything is now created
for you, endpoints are generated, authentication set and database is handled.

`bl-api` now magically has four new HTTP endpoints that you can access:
* `HTTP GET /orders/:id`
* `HTTP DELETE /orders/:id`
* `HTTP PATCH /orders/:id`
* `HTTP POST /orders`

You can now post data via `HTTP POST /orders` if the data conforms to the set
mongoose schema, you can retrieve a order with a specific mongodb-id or update
a order. It checks if you have the right permissions to access it and sends
error messages if you dont. **No need to write every endpoint manually.**

# Collection settings
The collection file can be simple or you can expand it with more advanced functions.


## Collection class
```typescript
export interface BlCollection {
  collectionName: string,
  mongooseSchema: any,
  documentPermission?: BlDocumentPermission,
  endpoints: BlEndpoint[]
}
```
* `collectionName` is the name of the collection, example: `orders`, `items` or `branches`.
* `mongooseSchema` is the mongoose schema specifying how the document should look like in the database.
* `documentPermission`
* `endpoints` a list of [BlEndpoint](#endpoints) for this collection

#### Minimal `Collection` class
```typescript
export class DogCollection implements BlCollection {
  collectionName: "dogs",
  mongooseSchema: dogSchema,
  endpoints: [
    {
      method: "getAll"
    }
  ]
}
```
This is a valid `Collection`. It has only one endpoint `HTTP GET /dogs`.

## Endpoints
Endpoints are a description of the valid methods on a collection. It contains
four fields that can be edited.

#### Endpoint class
```typescript
{
  method: 'post' | 'delete' | 'patch' | 'getAll' | 'getId',
  hook?: Hook,
  restriction?: BlEndpointRestriction,
  operations?: BlEndpointOperation[]
}
```

#### Endpoint methods
| Method | Description |
| ---- | ---- |
| post | Adds a mongodb-document to the database if it conforms to the mongoose schema |
| delete | Deletes a specific mongodb-document from the database |
| patch | Updates a specific mongodb-document in the database |
| getAll | Gets all mongodb-documents in that collection, can be filtered with a query | 
| getId | Gets a specific mongodb-document corresponding to the provided id |

_Document and Collection is also a core part of MongoDB, [Read more](https://mongodb.org)._

#### Endpoint hooks
```typescript
class SomeHook extends Hook {
  before(body?: any): Promise<boolean>;
  after(docs: BlDocument[]): Promise<BlDocument[]>
}
```
Hooks can be added to a endpoint if you want to execute some code either before
the action is done or after the action is done. If before or after fails, the
endpoint rejects with a HTTP error.

#### Endpoint restriction
```typescript
{
  permissions: 'customer' | 'employee' | 'manager' | 'admin' | 'super' [], 
  restricted?: boolean
}
```
* `permission` is the access you want the user to have to access this endpoint. 
* `restricted` can be set if the document in question is only viewable/editable for the customer who created it or the permission set in `Collection.viewableFor`.
  * For example in `UserDetail`, you don't want another customer to view your details.


#### Endpoint operations
`Operation` are not the same as `Hook`. A `Operation` is a code file that is
callable by hitting a cetain url.

Example: `PATCH orders/:id/place` is a `Operation` that places a order. This
could have been done through regular `PATCH` on `/orders/:id` but by having a
`Operation` it is much easier to understand what the intentions are. 

```typescript
{
  name: 'place',
  operation: {
    run: (blApiRequest?: BlApiRequest, req?: Request, res?: Response, next?: NextFunction) => { 
      // ... code ... 
    }
  },
  restriction: BlEndpointRestriction
}
```
* `name` is the name of the operation, the same name becomes the ending of the url
* `operation` is a object with the function `run` that should be called when visiting the URL
* `restriction` is the same as [Endpoint restriction](#endpoint-restriction) mentioned before.



