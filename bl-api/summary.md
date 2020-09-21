[\< home](../README.md)

# bl-api

This is the backend system for boklisten. It runs as a web server on [api.boklisten.no](https://api.boklisten.no)

* [Settings](./settings.md)
* [Authorization](./authorization.md)

## Introduction

Every action a user does is done through `bl-api`. If it is ordering books or
registering her name.

This application is used by both `bl-web` and `bl-admin`.

## Concept

A simple to use API for fetching, updating and create information that are needed to
ordering books.

## Functionality

`bl-api` has a simple to use API with easy to understand endpoints. Some examples:

- `HTTP GET api.boklisten.no/items?title=Signatur 3`
  - will retrieve all items that match the title specified
- `HTTP PATCH api.boklisten.no/orders/5b69e4f1abfa02002f8ade83/place`
  - places the order, creates customer-items if any and sends email to customer
- `HTTP DELETE api.boklisten.no/customeritems/5b71ea72820949002f322f4e`
  - deletes a customer-item from the database
- `HTTP POST api.boklisten.no/order`
  - creates a order if payload (body) is of correct format

## Goal

To be as easy as possible to use for the consumers of the API. Every endpoint
should be easy to understand and do exactly what it looks like it does. It
should be as fast as possible.

## Development

To read the spec and how to develop and test the application you must visit [bl-api's github
page](https://github.com/holskil/bl-api)


