# Athena GC Purchase Portal APIs

## Overview
The GC APIs allow external merchants to browse and purchase digital gift cards. The API follows RESTful principles and returns JSON responses.

## Base URL
`https://apiv3.lysto.io/api/v1`

## Authentication
External merchants need to authenticate using their API key and partnerid:

1. Send an email to sunil.kumar@lysto.io asking for test & production API keys and an partnerid
2. Store the keys securely

## Authentication Headers
For all API requests, include these headers:
- `Authorization: Bearer <your_api_key>`
- `partnerid: <partnerid>`

## Endpoints

### 1. List Available Gift Cards
**GET /giftcards**

Description: Retrieves a list of available gift cards.

Query Parameters:
- `brand` (optional) - Filter by brand name
- `pageNumber` (optional) - Page number for pagination (default: 0)
- `pageSize` (optional) - Number of items per page (default: 20, max: 100)

Example Request:
```
GET /giftcards?brand=Steam&pageNumber=0&pageSize=20 HTTP/1.1
Host: apiv3.lysto.io/api/v1
Authorization: Bearer YOUR_API_KEY
partnerid: partnerid
```

Example Response:
```json
{
  "status": 200,
  "giftcards": [
    {
      "id": "345",
      "name": "Steam Gift Card",
      "brand": "Steam"
    },
    {
      "id": "378",
      "name": "Valorant Gift Card",
      "brand": "Valorant"
    }
  ]
}
```

### 2. Get Gift Card SKU details
**GET /giftcards/{giftcard_id}/skus**

Description: Retrieves details of a specific gift card's available SKUs.

Example Request:
```
GET /giftcards/345/skus HTTP/1.1
Host: apiv3.lysto.io/api/v1
Authorization: Bearer YOUR_API_KEY
partnerid: partnerid
```

Example Response:
```json
{
  "status": 200,
  "skus": [
    {
      "sku_id": "10012",
      "name": "Steam Gift Card",
      "brand": "Steam",
      "denomination": "99",
      "quantity": "340",
      "discount": "5%"
    },
    {
      "sku_id": "10013",
      "name": "Steam Gift Card",
      "brand": "Steam",
      "denomination": "150",
      "quantity": "150",
      "discount": "10%"
    }
  ]
}
```

### 3. Purchase a Gift Card
**POST /giftcard/purchase**

Description: Initiates a purchase of a gift card.

Example Request:
```
POST /giftcard/purchase HTTP/1.1
Host: apiv3.lysto.io/api/v1
Authorization: Bearer YOUR_API_KEY
partnerid: partnerid
Content-Type: application/json

{
  "order_request_id": "5412",
  "brand_id": "345",
  "sku_id": "10013",
  "quantity": 34,
  "currency": "INR"
}
```

Example Response:
```json
{
  "order_request_id": "5412",
  "order_response_id": "50083",
  "status": "processed",
  "giftcodes": [
    {
      "brand_id": "345",
      "skuId": "10013",
      "currency": "INR",
      "name": "Steam Gift Card",
      "brand": "Steam",
      "denomination": "150",
      "codes": ["code1", "code2", "..."]
    }
  ]
}
```

### 4. Check Order Status
**GET /orders/{order_id}**

Description: Retrieves the status of a gift card purchase, including delivery details.

Example Request:
```
GET /orders/5412 HTTP/1.1
Host: apiv3.lysto.io/api/v1
Authorization: Bearer YOUR_API_KEY
partnerid: partnerid
```

Example Response:
```json
{
  "order_request_id": "5412",
  "order_response_id": "50083",
  "status": "processed",
  "giftcodes": [
    {
      "brand_id": "345",
      "skuId": "10013",
      "currency": "INR",
      "name": "Steam Gift Card",
      "brand": "Steam",
      "denomination": "150",
      "codes": ["code1", "code2", "..."]
    }
  ]
}
```

### 5. Get Wallet Balance
**GET /wallet-balance**

Description: Retrieves the current wallet balance of the merchant.

Example Request:
```
GET /wallet-balance HTTP/1.1
Host: apiv3.lysto.io/api/v1
Authorization: Bearer YOUR_API_KEY
partnerid: partnerid
```

Example Response:
```json
{
  "status": 200,
  "balance": "2000.00"
}
```

## Error Handling
Responses will include HTTP status codes:
- 200 OK - Successful request
- 400 Bad Request - Invalid input
- 401 Unauthorized - Missing or invalid API key
- 404 Not Found - Resource not found
- 500 Internal Server Error - Unexpected server issue

Example Error Response:
```json
{
  "error": "BadRequestError",
  "message": "Invalid gift card ID",
  "description": "Gift card with ID 999 not found"
}
```

## Support
For further assistance, contact support at sunil.kumar@lysto.io. 
