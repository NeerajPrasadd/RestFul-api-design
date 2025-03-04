# Request and Response Structure of RESTful API (Grocery Example)

A well-structured RESTful API ensures consistency, scalability, and ease of use. Below are the best practices for designing request and response structures in a hyperlocal grocery API.

---

## **1. Request Structure**
### **1.1 HTTP Methods**
Use appropriate HTTP methods for different operations:

| Method   | Usage |
|----------|------------------------------------------------|
| `GET`    | Retrieve a resource or list of resources |
| `POST`   | Create a new resource |
| `PUT`    | Update an existing resource (full update) |
| `PATCH`  | Partially update an existing resource |
| `DELETE` | Remove a resource |

### **1.2 Request Headers**
Common headers for RESTful APIs:
```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
```
- `Content-Type`: Specifies the request body format (usually `application/json`).
- `Accept`: Defines the expected response format.
- `Authorization`: Includes authentication credentials (JWT, API key, etc.).

### **1.3 Request Body**
Use a structured JSON object for `POST`, `PUT`, and `PATCH` requests.

#### **Example Request (POST /orders)**
```json
{
  "user_id": 123,
  "items": [
    {
      "product_id": 567,
      "quantity": 2
    },
    {
      "product_id": 890,
      "quantity": 1
    }
  ],
  "delivery_address": "123 Main Street, City, Zip Code",
  "payment_method": "credit_card"
}
```

### **1.4 Query Parameters (For Filtering & Pagination)**
Use query parameters for optional modifications like filtering, sorting, and pagination.

#### **Example Request (GET /products?category=fruits&sort=price_asc&page=2&per_page=10)**
```http
GET /products?category=fruits&sort=price_asc&page=2&per_page=10
```

---

## **2. Response Structure**
### **2.1 General Response Format**
A consistent JSON response format helps maintain clarity across all endpoints.

```json
{
  "status": 200,
  "success": true,
  "message": "Request was successful",
  "data": {},
  "meta": {},
  "errors": []
}
```

### **2.2 Success Response Examples**
#### **Single Resource (GET /products/567)**
```json
{
  "status": 200,
  "success": true,
  "message": "Product retrieved successfully",
  "data": {
    "id": 567,
    "name": "Fresh Apples",
    "category": "Fruits",
    "price": 2.5,
    "availability": "In Stock"
  },
  "errors": []
}
```

#### **Paginated List Response (GET /orders?page=1&per_page=10)**
```json
{
  "status": 200,
  "success": true,
  "message": "Orders retrieved successfully",
  "data": [
    { "id": 101, "user_id": 123, "total_price": 15.0, "status": "Delivered" },
    { "id": 102, "user_id": 456, "total_price": 20.5, "status": "Pending" }
  ],
  "meta": {
    "total": 100,
    "page": 1,
    "per_page": 10,
    "total_pages": 10,
    "has_next": true
  },
  "errors": []
}
```

---

### **2.3 Error Response Structure**
Use meaningful status codes and detailed error messages.

#### **Example Error Response (404 Not Found - Product Not Available)**
```json
{
  "status": 404,
  "success": false,
  "message": "Product not found",
  "data": null,
  "errors": []
}
```

#### **Example Validation Error Response (400 Bad Request - Order Validation Error)**
```json
{
  "status": 400,
  "success": false,
  "message": "Validation Error",
  "data": null,
  "errors": [
    { "field": "delivery_address", "message": "Address is required" },
    { "field": "items", "message": "At least one item must be included in the order" }
  ]
}
```

---

## **3. Status Codes and Their Meaning**
| Status Code | Meaning |
|-------------|------------------------------------------------|
| `200 OK` | Successful GET request |
| `201 Created` | Resource successfully created (POST) |
| `204 No Content` | Successful request with no response body |
| `400 Bad Request` | Client-side input validation error |
| `401 Unauthorized` | Authentication failed or missing token |
| `403 Forbidden` | Client has no access rights |
| `404 Not Found` | Requested resource does not exist |
| `409 Conflict` | Resource conflict (e.g., duplicate email) |
| `500 Internal Server Error` | Unexpected server error |

---

## **4. Best Practices**
✅ Use **consistent response format** across all endpoints.  
✅ Always include **status, success flag, message, and errors** fields.  
✅ Use **appropriate status codes** to indicate success or failure.  
✅ Provide **detailed error messages** to help clients debug issues.  
✅ Support **pagination and filtering** for large datasets.  



