[\< bl-api](./summary.md)

# Customer-items
Think of a customer-item as the digital representation of the physical item the
customer holds in her hands. This item can be returned, extended or bought out.

* [Different types](#different-types)
* [Customer item anatomy](#customer-item-anatomy)
* [Deadline](#deadline)
* [When is the customer-item active?](#when-is-the-customer-item-active)
* [Available actions](#available-actions)


# Customer item anatomy
## Simplified customer-item

```json
{
  "item": "item1",
  "blid": "blid1", 
  "type": "rent",
  "customer": "customer1",
  "deadline": "2020-01-01T00:00.000Z",
  "returned": false, 
  "buyout": false, 
  "orders": [
    "order1", 
    "order2"
  ],
}
```

`item` is the id of the Item object this customer-item is a representation of.
`blid` is the [BL-ID]() of the physical item. `type` is one of [three
types](#different-types) currently supported. `customer` is the id if the customer object.
`deadline` is the final date a return or buyout can happen. `returned` is a
boolean flag to indicate if this item is returned or not. `buyout` is a boolean
flag to indicate if this item is bought out or not. `orders` is a list of ids
of the orders this customer-item has been apart of.  

# Different types 
The customer-item can be of three types: `rent`, `loan` or `partly-payment`. 
| Type | Description |
| ---- | ---- |
| `rent` | When the customer has payed for keeping the item for a set period. |
| `loan` | When the customer only borrows  the item for a set period  (free, like the library). |
| `partly-payment` | When the customer has bought the item, but only part of it. The second payment should be done before the deadline. |


# Deadline
The customer-item has a deadline, this deadline is the final date for either
paying the last payment, buyout the item or returning the item.

The deadline is a core part of a `customer-item`. It is used for determine if
certain actions is allowed. For example: The customer can not buyout the
customer-item after the deadline.

Deadline is also used for holding control over who shall be returning items for
a set date. The system can then send reminders to the customers who holds
customer-items with a specific deadline. Invoices are also based on who has
active customer-items after the deadline.

## Deadline is determined by branch
Each branch has settings that determine the deadline. Under `rentPeriods` or `partlyPaymentPeriods` in
branch the admin sets the period type to either `semester` or `year` and a date
for the deadline. Based on this setting the deadline is set in customer-item
  upon creation.

# When is the customer-item active?

The customer-item is active if none of the listed flags are set:
* Returned
  * if the `returned` flag is set and `returnInfo` is populated with data the customer-item is no longer active.
* Bought out
  * if the `buyout` flag is set and `buyoutInfo` is populated with data the customer-item is no longer active.
* Canceled
  * if the `cancel` flag is set and `cancelInfo` is populated with data the customer-item is no longer active.
* Bought back
  * if the `buyback` flag is set and `buybackInfo` is populated with data the customer-item is no longer active.

# Available actions
In the `bl-api` system today there are several actions that is available as http endpoints

* `HTTP GET /customeritems`
  * Gets all customer-items in the db. Can be queried.
* `HTTP GET /customeritems/:id`
  * Gets a specific customer-item.
* `HTTP PATCH /customeritems/:id`
  * Updates a specific customer-item.
  * Can do multiple actions _([Read more about actions](./orders.md#order-item-types))_:
    * Return
    * Buyout
    * Cancel
    * Buyback
    

