# Request and Response Structure of RESTful API (Hyperlocal Grocery Example)

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
X-Correlation-ID: <unique-id>
```
- `Content-Type`: Specifies the request body format (usually `application/json`).
- `Accept`: Defines the expected response format.
- `Authorization`: Includes authentication credentials (JWT, API key, etc.).
- `X-Correlation-ID`: A unique identifier for tracing API requests across services and logs.

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
  "errors": [],
  "correlation_id": "<same-x-correlation-id>"
}
```
- `correlation_id`: The same value sent in `X-Correlation-ID` to enable tracing of the request-response cycle.

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
  "errors": [],
  "correlation_id": "abc-123"
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
  "errors": [],
  "correlation_id": "xyz-789"
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
  "errors": [],
  "correlation_id": "def-456"
}
```

---

## **3. Asynchronous API Calls**
### **3.1 When to Use Asynchronous APIs**
Asynchronous API calls should be used when:
- **Long-running processes** (e.g., order processing, bulk data uploads, background tasks).
- **Real-time updates** (e.g., notifications, order tracking, stock updates).
- **Third-party integrations** where response times may vary.
- **Avoiding blocking UI or backend** to ensure responsiveness.

### **3.2 Asynchronous Request-Response Pattern**
#### **Step 1: Client Makes a Request (POST /orders)**
```json
{
  "user_id": 123,
  "items": [
    { "product_id": 567, "quantity": 2 }
  ]
}
```
#### **Step 2: API Responds Immediately with a Processing Status**
```json
{
  "status": 202,
  "success": true,
  "message": "Order is being processed",
  "task_id": "xyz-123",
  "correlation_id": "task-123"
}
```

---

## **4. Status Codes and Their Meaning**
| Status Code | Meaning |
|-------------|------------------------------------------------|
| `200 OK` | Successful GET request |
| `201 Created` | Resource successfully created (POST) |
| `202 Accepted` | Request accepted for asynchronous processing |
| `204 No Content` | Successful request with no response body |
| `400 Bad Request` | Client-side input validation error |
| `401 Unauthorized` | Authentication failed or missing token |
| `403 Forbidden` | Client has no access rights |
| `404 Not Found` | Requested resource does not exist |
| `409 Conflict` | Resource conflict (e.g., duplicate email) |
| `500 Internal Server Error` | Unexpected server error |



