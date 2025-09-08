## Project Overview

This project demonstrates a simple microservices architecture using Flask, consisting of two independent services that communicate via HTTP APIs:

- **User Service**: Manages user data and operations
- **Order Service**: Manages orders and integrates with the User Service

## Architecture

```
┌─────────────────┐    HTTP Request     ┌─────────────────┐
│   Order Service │ ──────────────────> │   User Service  │
│   (Port 5002)   │                     │   (Port 5001)   │
└─────────────────┘                     └─────────────────┘
```

The Order Service makes HTTP requests to the User Service to fetch user details when retrieving order information, demonstrating inter-service communication in a microservices architecture.

## Services

### User Service (`user_service.py`)
- **Port**: 5001
- **Endpoints**:
  - `GET /users/<user_id>` - Retrieve user information
  - `POST /users` - Create a new user
- **Sample Data**: Pre-loaded with Alice and Bob

### Order Service (`order_service.py`)
- **Port**: 5002
- **Endpoints**:
  - `GET /orders/<order_id>` - Retrieve order information (includes user details)
  - `POST /orders` - Create a new order
- **Sample Data**: Pre-loaded with orders for Laptop and Smartphone

## Setup and Installation

### Prerequisites
- Python 3.11+
- pip package manager

### 1. Clone and Navigate to Project
```bash
cd /path/to/assn-w02-seth
```

### 2. Set up Virtual Environment
A virtual environment is already configured in the project. Activate it:

```bash
source assn-w02-seth/bin/activate
```

### 3. Install Dependencies
```bash
pip install flask requests
```

```bash
pip install requests
```

### 4. Create requirements.txt (Optional)
```bash
pip freeze > requirements.txt
```

## Running the Services

### Start User Service
```bash
python user_service.py
```
The User Service will start on `http://localhost:5001`

### Start Order Service (in a new terminal)
```bash
python order_service.py
```
The Order Service will start on `http://localhost:5002`

**Note**: Both services must be running simultaneously for full functionality, as the Order Service depends on the User Service.

## API Usage Examples

### User Service Examples

**Get a user:**
```bash
curl http://localhost:5001/users/1
```
Response:
```json
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

**Create a new user:**
```bash
curl -X POST http://localhost:5001/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Charlie", "email": "charlie@example.com"}'
```
Response:
```json
{
  "user_id": 3
}
```

### Order Service Examples

**Get an order (includes user details):**
```bash
curl http://localhost:5002/orders/1
```
Response:
```json
{
  "user_id": 1,
  "product": "Laptop",
  "user": {
    "name": "Alice",
    "email": "alice@example.com"
  }
}
```

**Create a new order:**
```bash
curl -X POST http://localhost:5002/orders \
  -H "Content-Type: application/json" \
  -d '{"user_id": 2, "product": "Tablet"}'
```
Response:
```json
{
  "order_id": 3
}
```

## Testing

### Manual Testing
1. Start both services
2. Test individual endpoints using curl commands above
3. Verify that order retrieval includes user information from the User Service

### Verify Inter-Service Communication
1. Get an order: `curl http://localhost:5002/orders/1`
2. Check that the response includes both order and user information
3. This confirms the Order Service successfully communicates with the User Service

## Project Structure

```
assn-w02-seth/
├── assn-w02-seth/          # Virtual environment
├── user_service.py         # User microservice
├── order_service.py        # Order microservice
└── README.md              # This file
```

## Key Learning Objectives

This project demonstrates:

1. **Microservices Architecture**: Separation of concerns into independent services
2. **Inter-Service Communication**: HTTP-based communication between services
3. **Service Independence**: Each service can be developed, deployed, and scaled independently
4. **RESTful API Design**: Implementation of REST principles for API endpoints
5. **Error Handling**: Proper HTTP status codes and error responses

## Technologies Used

- **Flask**: Lightweight Python web framework
- **Requests**: HTTP library for inter-service communication
- **Python**: Programming language
- **JSON**: Data exchange format
