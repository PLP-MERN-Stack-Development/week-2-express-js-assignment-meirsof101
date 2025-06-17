# Product API - Week 2 Express.js Assignment

A RESTful API for managing products with authentication, validation, and error handling.

## Table of Contents
- [Getting Started](#getting-started)
- [API Endpoints](#api-endpoints)
- [Authentication](#authentication)
- [Error Handling](#error-handling)
- [Testing with Postman](#testing-with-postman)

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- npm

### Installation

1. Clone the repository or download the files
2. Install dependencies:
```bash
npm install express body-parser uuid dotenv
```

3. Create a `.env` file in the root directory:
```env
API_KEY=your_secret_api_key_here
PORT=3000
```

4. Start the server:
```bash
node server.js
```

The server will start on `http://localhost:3000` (or the port specified in your .env file).

## Authentication

All API endpoints require authentication using a Bearer token.

**Header Required:**
```
Authorization: Bearer your_secret_api_key_here
```

**Error Responses:**
- `401 Unauthorized`: Missing or invalid Authorization header
- `403 Forbidden`: Invalid API token

## API Endpoints

### Root Route

#### GET /
Returns a welcome message.

**Request:**
```http
GET http://localhost:3000/
Authorization: Bearer your_api_key
```

**Response:**
```
Welcome to the Product API! Go to /api/products to see all products.
```

### Products

#### GET /api/products
Get all products with optional filtering and pagination.

**Query Parameters:**
- `category` (optional): Filter by category (e.g., "electronics", "kitchen")
- `page` (optional): Page number (default: 1)
- `limit` (optional): Number of items per page (default: 10)

**Request:**
```http
GET http://localhost:3000/api/products?category=electronics&page=1&limit=5
Authorization: Bearer your_api_key
```

**Response:**
```json
{
  "total": 2,
  "page": 1,
  "limit": 5,
  "products": [
    {
      "id": "1",
      "name": "Laptop",
      "description": "High-performance laptop with 16GB RAM",
      "price": 1200,
      "category": "electronics",
      "inStock": true
    },
    {
      "id": "2",
      "name": "Smartphone",
      "description": "Latest model with 128GB storage",
      "price": 800,
      "category": "electronics",
      "inStock": true
    }
  ]
}
```

#### GET /api/products/:id
Get a specific product by ID.

**Request:**
```http
GET http://localhost:3000/api/products/1
Authorization: Bearer your_api_key
```

**Response:**
```json
{
  "id": "1",
  "name": "Laptop",
  "description": "High-performance laptop with 16GB RAM",
  "price": 1200,
  "category": "electronics",
  "inStock": true
}
```

#### POST /api/products
Create a new product.

**Request:**
```http
POST http://localhost:3000/api/products
Authorization: Bearer your_api_key
Content-Type: application/json

{
  "name": "Wireless Headphones",
  "description": "Noise-cancelling wireless headphones",
  "price": 150,
  "category": "electronics",
  "inStock": true
}
```

**Response:**
```json
{
  "id": "generated-uuid-here",
  "name": "Wireless Headphones",
  "description": "Noise-cancelling wireless headphones",
  "price": 150,
  "category": "electronics",
  "inStock": true
}
```

#### PUT /api/products/:id
Update an existing product.

**Request:**
```http
PUT http://localhost:3000/api/products/1
Authorization: Bearer your_api_key
Content-Type: application/json

{
  "name": "Updated Laptop",
  "description": "High-performance laptop with 32GB RAM",
  "price": 1500,
  "category": "electronics",
  "inStock": true
}
```

**Response:**
```json
{
  "id": "1",
  "name": "Updated Laptop",
  "description": "High-performance laptop with 32GB RAM",
  "price": 1500,
  "category": "electronics",
  "inStock": true
}
```

#### DELETE /api/products/:id
Delete a product.

**Request:**
```http
DELETE http://localhost:3000/api/products/3
Authorization: Bearer your_api_key
```

**Response:**
```
Status: 204 No Content
```

### Search and Analytics

#### GET /api/products/search
Search products by name.

**Query Parameters:**
- `name` (required): Search term

**Request:**
```http
GET http://localhost:3000/api/products/search?name=laptop
Authorization: Bearer your_api_key
```

**Response:**
```json
[
  {
    "id": "1",
    "name": "Laptop",
    "description": "High-performance laptop with 16GB RAM",
    "price": 1200,
    "category": "electronics",
    "inStock": true
  }
]
```

#### GET /api/products/stats
Get product count statistics by category.

**Request:**
```http
GET http://localhost:3000/api/products/stats
Authorization: Bearer your_api_key
```

**Response:**
```json
{
  "electronics": 2,
  "kitchen": 1
}
```

## Data Validation

### Product Schema
All product data must include:
- `name`: string (required)
- `description`: string (required)
- `price`: number (required)
- `category`: string (required)
- `inStock`: boolean (required)

### Validation Errors
Invalid data will return a `400 Bad Request` with error details:
```json
{
  "message": "Invalid product data. Check all fields."
}
```

## Error Handling

The API returns appropriate HTTP status codes:

- `200 OK`: Successful GET/PUT requests
- `201 Created`: Successful POST requests
- `204 No Content`: Successful DELETE requests
- `400 Bad Request`: Invalid data or missing required fields
- `401 Unauthorized`: Missing or invalid Authorization header
- `403 Forbidden`: Invalid API token
- `404 Not Found`: Resource not found
- `500 Internal Server Error`: Server errors

### Error Response Format
```json
{
  "message": "Error description"
}
```

## Testing with Postman

### Quick Setup
1. Create a new collection in Postman
2. Set up environment variables:
   - `base_url`: `http://localhost:3000`
   - `api_token`: `your_secret_api_key_here`

3. Add authorization header to collection:
   - Key: `Authorization`
   - Value: `Bearer {{api_token}}`

### Test Sequence
1. GET `/` - Welcome message
2. GET `/api/products` - All products
3. GET `/api/products/1` - Single product
4. POST `/api/products` - Create new product
5. PUT `/api/products/1` - Update product
6. GET `/api/products/search?name=laptop` - Search
7. GET `/api/products/stats` - Statistics
8. DELETE `/api/products/3` - Delete product

## Sample Data

The server starts with 3 sample products:
1. **Laptop** - High-performance laptop ($1200, electronics)
2. **Smartphone** - Latest model with storage ($800, electronics)  
3. **Coffee Maker** - Programmable with timer ($50, kitchen, out of stock)

## Features

- ✅ RESTful API design
- ✅ JWT-style Bearer token authentication
- ✅ Request logging middleware
- ✅ Data validation middleware
- ✅ Custom error handling
- ✅ Pagination support
- ✅ Category filtering
- ✅ Product search
- ✅ Analytics endpoint
- ✅ In-memory data storage

## License

This project is for educational purposes as part of a Week 2 Express.js assignment.