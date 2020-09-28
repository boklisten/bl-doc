![](../imgs/boklisten-logo-long.png)

Documentation for all of Boklisten.no's systems.

- [Our applications and modules](#our-applications-and-modules)
- [Introduction](#introductions)
- [How our main applications connect to each other](#how-our-main-applications-connect-to-each-other)
- [Knowledge you need before you code](#knowledge-you-need-before-you-code)

# Our applications and modules

- Applications
  - [bl-api](bl-api/summary.md)
  - [bl-admin](bl-admin/summary.md)
  - [bl-web](bl-web/summary.md)
- Modules
  - [bl-model](bl-model/summary.md)
  - [bl-login](bl-login/summary.md)
  - [bl-connect](bl-connect/summary.md)
  - [bl-post-office](bl-post-office/summary.md)
  - [bl-email](bl-email/summary.md)

# Introduction

At boklisten we sell, loan and rent out books. Customers comes to us either by
visiting our [website](https://boklisten.no) or by going to our multiple pop-up stands. Boklisten can be
translated to "the book list". We pride ourself by claiming to "Always have the
right book".

Our customers are people at school that need books to their classes.

To do this we have buildt a system from the ground up. This ensures that we
have full controll over the codebase, and can create specialized tools for our
business processes.

We have three main applications: `bl-api`, `bl-web` and `bl-admin`. They are
all equally important to the daily work at boklisten.

- `bl-api` is a backend service that controls the database and is used as a broker
  between `bl-web` and `bl-admin`.

- `bl-web` is the frontend application the customers uses. Here the customer can
  either rent, buy or loan items. They can also use it to view information
  provided to each branch and to extend a deadline.

- `bl-admin` is the frontend application for employees. Here the employee can
  view status of a item, rent, sell and buy items and take out reports.

# How our main applications connect to each other

![](imgs/bl_systems.png)

# Knowledge you need before you code

- [Typescript](https://www.typescriptlang.org)
  - Every module and application is written in typescript.
- [NodeJS](https://nodejs.org/en/)
  - The backend is written in typescript/javascript and to run this code on the server you need the node environment.
  - [Express](http://expressjs.com)
    - Express is a framework buildt on top of Node for ease of use regarding http endpoints
- [Angular](https://angular.io)
  - Angular 2+ is used for all frontend applications and modules
- [MongoDB](https://www.mongodb.com)
  - MongoDB is the backend database we use to store all of our data
- [NPM](https://www.npmjs.com)
  - NPM is used to publish and consume our modules
- [Heroku](https://www.heroku.com)
  - Where our development and productions servers are located
- [Git](https://git-scm.com)
  - For version control
