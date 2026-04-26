# Orders SAPI Implementation

## Overview
This project implements the Orders System API (SAPI) version 1.0.1, providing order management functionality for the Online Books system. The implementation follows MuleSoft API-led connectivity patterns and includes comprehensive business validation logic.

## Features

### API Endpoints
- **GET /orders** - Retrieve all orders with pagination support
  - Query parameters: `limit` (default: 50), `offset` (default: 0)
- **GET /orders/{id}** - Retrieve a specific order by ID
- **POST /orders** - Create a new order with comprehensive validation

### Business Validations
The implementation includes sophisticated business validation for order creation:

1. **Customer Validation**: Verifies customer exists in the customers-sapi
2. **Book Validation**: Validates all books exist in the books-catalog-sapi and calculates total amount
3. **Inventory Check**: Ensures sufficient stock is available in the inventory-sapi

### Error Handling
Comprehensive error handling with specific error codes as per RAML specification:
- `CLIENT_INVALID`: Customer ID doesn't exist
- `BOOK_INVALID`: Book ID doesn't exist in catalog
- `OUT_OF_STOCK`: Insufficient inventory for requested books

## Architecture

### System Dependencies
- **customers-sapi** (localhost:8081) - Customer validation
- **books-catalog-sapi** (localhost:8081) - Book validation and pricing
- **inventory-sapi** (localhost:8082) - Stock availability checking

### Data Storage
Uses ObjectStore V2 for order persistence with the following structure:
- Orders stored by orderId as key
- Automatic order ID generation (ORD001, ORD002, etc.)
- Last order ID counter management

### Port Configuration
- **Main API**: Port 8083
- **API Console**: Port 8083/console/

## Project Structure
```
src/
├── main/
│   ├── mule/
│   │   └── orders-sapi-impl.xml    # Main implementation
│   └── resources/
│       └── log4j2.xml              # Logging configuration
└── test/
    ├── munit/                      # MUnit test cases
    └── resources/                  # Test resources
```

## Configuration Files

### Main Flows
1. **orders-sapi-main** - Main API entry point with APIKit router
2. **orders-sapi-console** - API console for testing
3. **init-orders** - Data initialization endpoint (/init)

### Implementation Flows
- **get:\orders:orders-sapi-config** - List orders with pagination
- **get:\orders\(id):orders-sapi-config** - Get order by ID
- **post:\orders:application\json:orders-sapi-config** - Create new order

### Validation Sub-flows
- **validate-customer-flow** - Customer existence validation
- **validate-books-and-calculate-total-flow** - Book validation and total calculation
- **check-inventory-flow** - Stock availability verification
- **create-order-flow** - Order creation and storage

## Dependencies
```xml
<!-- Core MuleSoft Dependencies -->
<dependency>
    <groupId>org.mule.modules</groupId>
    <artifactId>mule-apikit-module</artifactId>
    <version>1.11.16</version>
</dependency>

<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-http-connector</artifactId>
    <version>1.11.1</version>
</dependency>

<dependency>
    <groupId>org.mule.connectors</groupId>
    <artifactId>mule-objectstore-connector</artifactId>
    <version>1.2.1</version>
</dependency>

<!-- API Specifications -->
<dependency>
    <groupId>bcb991d0-dc5b-4ff7-b0d9-bda707983f40</groupId>
    <artifactId>orders-sapi</artifactId>
    <version>1.0.1</version>
</dependency>

<dependency>
    <groupId>bcb991d0-dc5b-4ff7-b0d9-bda707983f40</groupId>
    <artifactId>onlinebooks-types</artifactId>
    <version>1.3.0</version>
</dependency>
```

## Getting Started

### Prerequisites
- Anypoint Studio or Mule Runtime 4.11.2+
- Maven 3.6+
- Running instances of:
  - customers-sapi (port 8081)
  - books-catalog-sapi (port 8081)
  - inventory-sapi (port 8082)

### Running the Application
1. Build the project:
   ```bash
   mvn clean compile
   ```

2. Deploy to Mule Runtime or run in Anypoint Studio

3. Initialize sample data:
   ```bash
   curl -X GET http://localhost:8083/init
   ```

4. Access API Console:
   ```
   http://localhost:8083/console/
   ```

### Testing

#### Sample Order Creation Request
```json
{
  "customerId": 1,
  "items": [
    {
      "bookId": 1,
      "quantity": 2
    },
    {
      "bookId": 2,
      "quantity": 1
    }
  ]
}
```

#### Sample Response
```json
{
  "orderId": "ORD004",
  "customerId": 1,
  "status": "PENDING",
  "items": [
    {
      "bookId": 1,
      "quantity": 2
    },
    {
      "bookId": 2,
      "quantity": 1
    }
  ],
  "totalAmount": 89.97
}
```

## Monitoring and Logging
- Comprehensive logging at INFO level for all major operations
- Error tracking with detailed error messages
- Request/response logging for debugging

## Security Considerations
- Input validation through RAML specification
- Business rule validation before data persistence
- Proper error handling to prevent information leakage

## Future Enhancements
- Database persistence instead of ObjectStore
- Authentication and authorization
- Rate limiting and throttling
- Enhanced monitoring and metrics
- Order status update endpoints
- Order cancellation functionality

## API Documentation
For complete API documentation, refer to:
- RAML specification: `orders-sapi.raml`
- API Console: `http://localhost:8083/console/`
- Exchange documentation for orders-sapi v1.0.1