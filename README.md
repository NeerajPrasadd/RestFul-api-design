# RestFul-api-design
This is Api design principal page
# RESTful API URL Design Best Practices

A well-designed RESTful API URL structure improves **clarity, usability, and scalability**. Below are best practices for designing clean and intuitive API endpoints.

---

## **1. General Structure**
```
/{resource}/{resource_id}/{sub-resource}/{sub-resource_id}
```
- Use **nouns** for resources (not verbs).
- Use **plural** form for collections.
- Keep URLs **predictable and hierarchical**.

---

## **2. Basic CRUD Operations**

| HTTP Method | Operation | Example URL | Description |
|-------------|------------|------------------|-------------|
| `GET` | Fetch all | `/users` | Retrieve a list of users |
| `GET` | Fetch one | `/users/123` | Retrieve user with ID 123 |
| `POST` | Create | `/users` | Create a new user |
| `PUT` | Update | `/users/123` | Update user with ID 123 |
| `PATCH` | Partial Update | `/users/123` | Partially update user |
| `DELETE` | Delete | `/users/123` | Delete user with ID 123 |

---

## **3. Handling Sub-Resources**
When a resource has a related sub-resource, structure the URL **hierarchically**.

### **Example: Managing Posts and Comments**

| Operation | Example URL | Description |
|-----------|------------|-------------|
| `GET` all comments | `/posts/5/comments` | Get all comments for post ID 5 |
| `GET` single comment | `/posts/5/comments/42` | Get comment ID 42 on post ID 5 |
| `POST` new comment | `/posts/5/comments` | Add a comment to post ID 5 |
| `DELETE` comment | `/posts/5/comments/42` | Delete comment ID 42 |

---

## **4. Filtering, Sorting, and Pagination**
Use **query parameters** for optional modifications.

### **Filtering**
```
GET /users?role=admin
```
Retrieve all users with the `admin` role.

### **Sorting**
```
GET /products?sort=price_asc
```
Sort products by price in ascending order.

### **Pagination**
```
GET /products?page=2&per_page=10
```
Retrieve page 2 with 10 items per page.

---

## **5. Search Endpoint**
Use a dedicated search endpoint instead of overloading `GET` with complex filters.
```
GET /search?q=keyword&type=user
```
- `q` → Search query
- `type=user` → Search only users

---

## **6. Authentication & Authorization**
- Use **JWT tokens** in headers (preferred):
  ```
  Authorization: Bearer <token>
  ```
- Use `/auth` for authentication actions:
  ```
  POST /auth/login
  POST /auth/logout
  POST /auth/refresh
  ```

---

## **7. Status & Health Check**
Expose a simple health check endpoint to monitor API availability.
```
GET /health
```
Response:
```json
{
  "status": "ok",
  "uptime": "24h"
}
```

---

## **8. Versioning**
Version your API to **avoid breaking changes**.

### **Approaches:**
1. **URL Versioning (Recommended)**
   ```
   /v1/users
   ```
2. **Header Versioning**
   ```
   Accept: application/vnd.myapi.v1+json
   ```

---

## **9. Error Handling & Responses**
Use meaningful HTTP status codes and a structured error response.

### **Example Error Response**
```
GET /users/9999
```
Response:
```json
{
  "status": 404,
  "success": false,
  "message": "User not found",
  "errors": []
}
```

---

## **10. Avoid These Common Mistakes**
❌ **Using Verbs in URLs**
```
GET /getUsers  ❌ (Bad)
GET /users ✅ (Good)
```

❌ **Using Singular Form for Collections**
```
GET /user  ❌ (Bad)
GET /users ✅ (Good)
```

❌ **Deeply Nested Resources**
```
/users/123/posts/5/comments/42/replies/8 ❌ (Bad)
```
Instead, use:
```
/comments/42/replies ✅ (Good)
```

---

## **Final Thoughts**
✅ Use **plural nouns** for resources  
✅ Keep URLs **simple and hierarchical**  
✅ Use **query parameters** for filtering & sorting  
✅ Use **meaningful status codes**  
✅ Implement **versioning** early  



