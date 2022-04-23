# Cloud Computing Project - Phase 1
This documentation describes a microservice architecture for [JPetStore](https://github.com/mybatis/jpetstore-6) project and plans to implement it in a cloud environment.

## Overview
JPetStore is a simple pet store web application written in Java using Spring. A sample demo of the original monolith application is available [here](https://petstore.octoperf.com/actions/Catalog.action).

The application has a hierarchical catalogue, a user registration and authentication system, and a purchase system with a shopping cart. In compliance with this, the monolith has three separate services:
- **Catalog**: provides a catalogue of products in hierarchical categorisation.
- **Account**: provides user authentication system using token-based authentication.
- **Order**: handles the purchase process and shopping carts.

The original database structure has some flaws and redundancies which are addressed and fixed in the proposed architecture.

In the following sections, a three-part architecture is proposed that will divide the monolith into the three aforementioned services, each with their own database and gRPC service. The architecture, if approved, will then be implemented from scratch using Node.js and MongoDB in phase 2.

## Proposed Design
### Microservices
The three microservices that best divide the monolith are the ones described above, as they are already somewhat well-defined. Other reasons for splitting the monolith into these three services are:
1. The user authentication system only interacts with the rest of the application through the order mechanism.
2. The catalogue is mostly static and is updated either by admin to update or add new products or through the order system to update the stock.
3. The order system is the most dynamic and is updated frequently through purchases and cart management.
4. Each of these microservices has a clearly defined database structure that only interacts with the other two through gRPC.

### Database Structure
The database, from the prespective of each service has the following models:
* **User**:
  * users: user accounts and information
  * favourite categories: favourite category as chosen by the user
* **Order**:
  * carts: shopping cart details, user_id, items, total, status, etc.
  * orders: order details (i.e. finalized carts with purchase information)
* **Catalog**:
  * categories: categories and subcategories, include parent_id for hierarchy
  * products: product details, category_id, stock, price, etc.
  * suppliers: supplier information, name, address, etc.

<!-- insert access diagram 1 (db objects in services) -->
| ![access diagram 1](https://github.com/fum-cloud-project/documentation/blob/main/access_diagram_1.png) |
|:--:|
|*Access diagram 1*|


### Actors
The actors in the scenario are the following:
- **Customer**: the user of the application, can register, login, view catalogue, and place orders (create carts and confirm orders). It can also pick a favourite category to display in profile.
- **Admin**: the administrator of the application, can add, update, delete products and categories, manage users, and view orders.

<!-- insert access diagram 2 (user-service and service-service interactions) -->
| ![access diagram 2](https://github.com/fum-cloud-project/documentation/blob/main/access_diagram_2.png) |
|:--:|
|*Access diagram 2*|

### Internal Communication
The specific internal communications are as follows:
* `user — catalog`: getting category information for favourite categroy selection
* `user — order`: validate auth token
* `order — catalog`: getting product information
* `catalog — order`: update product stock


<!-- add roles -->
## Authors
<!-- + Tooraj Taraz ([@ToorajTaraz](https://github.com/ToorajTaraz))
+ Hamed Ghoochanian ([@HamedGhoochanian](https://github.com/HamedGhoochanian))
+ Afarin Zamanian ([@Af4rinz](https://github.com/Af4rinz)) -->

| [<img class="avatar avatar-user" src="https://avatars.githubusercontent.com/u/64916254?v=4" width="75px;"/><br/>Tooraj Taraz](https://github.com/ToorajTaraz) (Team Lead) <br/>user - auth | [<img class="avatar avatar-user" src="https://avatars.githubusercontent.com/u/67141954?v=4" width="75px;"/><br/>Hamed Ghoochanian](https://github.com/HamedGhoochanian)<br/>catalogue - order| [<img class="avatar avatar-user" src="https://avatars.githubusercontent.com/u/23293389?v=4" width="75px;" /><br/>Afarin Zamanian](https://github.com/Af4rinz)<br/>design & doc|
| :---: | :---: | :---: |
