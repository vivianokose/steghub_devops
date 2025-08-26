# Understanding RESTful APIs using a story

## Introduction
Picture this: you walk into a restaurant. You don’t go to the kitchen yourself — instead, you ask the waiter. The waiter takes your order to the kitchen and then brings back your food.  
That waiter is like an **API**. Specifically, in web development, we use **RESTful APIs**.

---

## How REST Works
- **Resources**: The dishes on the menu (e.g., `users`, `orders`, `products`).  
- **Endpoints**: The menu sections or item numbers (`/api/users`).  
- **HTTP Methods**: The actions you can take:  
  - `GET` → Read (look at the menu)  
  - `POST` → Create (place a new order)  
  - `PUT/PATCH` → Update (change your order)  
  - `DELETE` → Cancel/remove an order  

---

## Example
```http
GET /api/books       → fetch all books
POST /api/books      → add a new book
GET /api/books/1     → fetch book with ID=1
PUT /api/books/1     → update book with ID=1
DELETE /api/books/1  → delete book with ID=1
````

---

## Why RESTful APIs Are Loved

* They’re simple and predictable.
* They connect frontend and backend easily.
* They’re language-independent.
* They power everything from mobile apps to microservices.

---

## Conclusion

So next time you order food online, remember: somewhere, a RESTful API just played the waiter between you and the kitchen!

````