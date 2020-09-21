
[\< bl-api](./summary.md)

# Orders
Orders are a key part of `bl-api`. It is the functionality that is most used
and are the main reason for `bl-api` to exist.

Think of orders as a list of actions done to a item. From the first placement to
the return of the item. Every action (extend, return, buyout etc.) for a
`customer-item` is done through a order. 


* [Order anatomy](#order-anatomy)
* [Order flow](#order-flow)
* [Order-item types](#order-item-types)
* [Order-item type permission](#order-item-type-permission)

# Order anatomy
The order is simply a object holding information about what items the customer
wants to get/update, what payment should be used and what delivery method. 

### Simplified order
Underneat you can see a simplified order. It includes the amount and the type
of order-item. To read more about the order object you must visit the
`bl-model` repo.

```json
{
  "id": "order1",
  "amount": 220,
  "delivery": "delivery1",
  "payments": [
    "payment1", 
    "payment2"
  ],
  "orderItems": [
    {
      "title": "Catch-22",
      "type": "rent",
      "item": "item1",
      "info": {
        "deadline": "2020-01-01T00:00.000Z"
      },
      "amount": 100
    }, 
    {
      "title": "Animal farm",
      "type": "buy",
      "item": "item2",
      "amount": 120
    }
  ],
}
```
As you can see, a order is concisting of some key fields. The `amount` is the
total of all order-item amounts. `delivery` is a id of the delivery object.
`payments` is a list of ids to payment objects, it can be one, multiple or
none.

The `order-items` are a list of actions to be done to either a item or
customer-item. Here the `type` directs what to do with it. `amount` is what the
order-item type costs.

# Order flow
_This flow assumes the user is logged in._

* Create order
  * `HTTP POST /orders { order object }`
  * after the order is posted it goes through a validation process. Some key points: 
    * checks if the amounts add up
    * checks if the items are actually available 
    * checks if the user can do the operation.
    * checks if the object posted is a valid order
* Updating order
  * `HTTP PATCH /orders/:id { patch object }`
  * The order can be updated several times before placing it. This can be to:
    * edit delivery option
    * add payment
* Placing order
  * For customers: `HTTP PATCH /orders/:id/confirm`
  * For employees: `HTTP PATCH /orders/:id/place`
  * when placing the order a new validation process begins:
    * validates the order
    * validates the payments
    * validates the delivery
  * after validation:
    * add `customer-items` (if order-items describes this, and the user is `employee` or higher)
    * send email confirmation
    * update order with `{placed: true}`

# Order item types
Each order consists of one or more order-items. These order-items each has a
type. The type is the action that should be done to that item or customer-item. 

### List of order-item types
- rent
  - rent item to a set deadline
- buy
  - buy the item
- loan
  - loan item to a certain deadline
- partly-payment
  - buy the item but with one payment now and one later at a set deadline
- sell
  - sell the item to the company 
- cancel
  - cancel a order-item
- extend
  - extend a customer-item's deadline to a later deadline 
- buyout
  - buyout a customer-item of type `loan` or `rent` instead of returning it
- return 
  - return a customer-item whoes type was either `loan` or `rent`
- buyback
  - if the customer-item is of type `partly-payment`, instead of paying second payment he can delivery it back to the company

# Order-item type permission 
| order-item type | customer | employee | manager | admin | super |
| ---- | ---- | ---- | ---- | ---- | ---- |
| rent | x | x | x | x | x |
| buy | x | x | x | x | x |
| loan | x | x | x | x | x |
| partly-payment | x | x | x | x | x |
| buyout | x | x | x | x | x |
| extend | x | x | x | x | x |
| sell |  | x | x | x | x |
| return |  | x | x | x | x |
| buyback |  | x | x | x | x |
| cancel |  | |  | x | x |


