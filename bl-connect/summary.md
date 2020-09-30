[\< home](../README.md)

# bl-connect

`bl-connect` is a module for front-end applications that want to connect to the
`bl-api`-backend. It is used by `bl-admin`, `bl-web`, `bl-reporter` and
`bl-login`.

- [Each class has its own service](#each-class-has-its-own-service)
- [Simple to use](#simple-to-use)
- [Authorization](#authorization)
  - [Login](#login)
  - [Register](#register)
- [Collection functionalities](#collection-functionalities)

# Each class has its own service

Every class defined in `bl-model` that also are used as a data-endpoint in
`bl-api` has its own service in `bl-connect`.

### Example classes that have a coresponing bl-connect service

- Item
  - ItemService
  - `import { ItemService } from 'bl-connect';`
- Branch
  - BranchService
  - `import { BranchService } from 'bl-connect';`
- CustomerItem
  - CustomerItemService
  - `import { CustomerItemService } from 'bl-connect';`

# Simple to use

```typescript
import { ItemService } from "@wizardcoder/bl-connect";

class ItemComponent {
  constructor(private itemService: ItemService) {}

  public getItem(itemId: string) {
    this.itemService
      .getById(itemId)
      .then(item => {
        console.log("the item from bl-api", item);
      })
      .catch(e => {
        console.log(
          "something went wrong when retrieving item ffrom bl-api",
          e
        );
      });
  }
}
```

# Authorization

Token handling and authorization headers are manages by bl-connect. No need for
the consumer of `bl-connect` to think about token reauthentication and
authorization in general. All the user needs to do before using the collection
functions provided are login.

## Login

Login is needed to obtain the access and refresh tokens for accessing `bl-api`.
[Read more about bl-api's authorization](../bl-api/authorization.md).

```typescript
import { LoginService } from "bl-connect";
```

#### `loginService.login(username: string, password: string)`

User can login with username and password. The username must already be
registered for login to work.

#### `loginService.facebookLogin()`

User will be redirected to a facebook login page.

#### `loginService.googleLogin()`

User will be redirected to a google login page.

#### `loginService.feideLogin()`

User will be redirected to a feide login page.

## Register

To obtain access tokens you need a user. This user is registered using the `registerService`.

[Read more about bl-api's authorization](../bl-api/authorization.md).

```typescript
import { RegisterService } from "bl-connect";
```

#### `registerService.localRegister(username: string, password: string)`

The user can register with usename and password as long as the username is not
already taken and the password is long enough.

#### `registerService.facebookRegister()`

User will be redirected to a facebook login page.

#### `registerService.googleRegister()`

User will be redirected to a google login page.

#### `registerService.feideRegister()`

User will be redirected to a feide login page.

# Collection functionalities

Each document-service (ItemService, CustomerItemService etc.) has several
functions. Each function returns a `Promise<T[]>` type.

#### `getById(id: string, options?: Options)`

Returns the document that matches the id.

#### `getWithOperation(id: string, operation: string, options?: Options)`

Runs the operation for a document with the specified id and returns the value.
[Read more about
bl-api's Operations](../bl-api/endpoints-and-collections.md#endpoint-operations)

#### `getManyByIds(ids: string[], options?: Options)`

Retrieves multiple documents from the backend.

> Important: this method fetches documents one by one and may be very slow. Try
> using `get()` with a set `query` instead.

#### `update(id: string, data: any)`

Updates a document with the provided data. The data must conform to the set
mongoose schema or the definition in `bl-model`

#### `updateWithOperation(id: string, operation: string, data: any)`

Updates the document with a operation.
[Read more about
bl-api's Operations](../bl-api/endpoints-and-collections.md#endpoint-operations)

#### `add(data: any)`

Adds a document to the database.

#### `remove(id: string`

Removes the document with the specified id.
