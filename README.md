# 🛒 E-commerce Product Catalog Microservice (Node.js, TypeScript, Docker)

This project is a mid-level implementation of an e-commerce backend architecture split into two decoupled microservices, demonstrating **Role-Based Access Control (RBAC)** via inter-service HTTP communication and **Asynchronous Stock Management** via a **RabbitMQ** message broker.

## 🛠️ Technology Stack

* **Runtime:** Node.js
* **Language:** TypeScript
* **Web Framework:** Express.js
* **Containerization:** Docker & Docker Compose
* **Databases:** PostgreSQL (User Auth), MongoDB (Product Catalog)
* **Messaging:** RabbitMQ (AMQP)
* **Security:** JSON Web Tokens (JWT)

## 🚀 Services Overview

| Service | Port (External) | Primary Function | Database |
| :--- | :--- | :--- | :--- |
| **`user-auth-service`** | `3001` | Handles Registration, Login, and **JWT Validation**. | PostgreSQL |
| **`product-service`** | `3000` | Manages the Product Catalog, Filtering, and Stock. | MongoDB |

## ⚙️ Setup and Installation

### Prerequisites

* Node.js (v18+)
* Docker & Docker Compose

### Steps

1.  **Clone the Repository:**
    ```bash
    git clone [your_repo_url]
    cd microservice-ecomm-rbc
    ```

2.  **Configure Environment Variables:**
    Create a `.env` file in the root of *both* `user-auth-service` and `product-service` directories based on the examples provided above.

3.  **Build and Run Containers:**
    The Docker Compose file handles building the TypeScript code, installing dependencies, and starting all services (Auth, Products, Postgres, Mongo, RabbitMQ) on a shared network.
    ```bash
    docker-compose up --build
    ```
    *Services will be accessible via `http://localhost:3000` and `http://localhost:3001`.*

## 🧪 API Usage and Testing

Use a tool like Postman or Insomnia to test the API endpoints.

### Step 1: Authentication (User & Auth Service - Port 3001)

| Action | Method | Endpoint | Note |
| :--- | :--- | :--- | :--- |
| Register | `POST` | `/api/auth/register` | Create a new user (default role: `basic`). |
| Login | `POST` | `/api/auth/login` | Get the **JWT Token**. |

### Step 2: Product Creation (Product Service - Port 3000)

**A. Protected Endpoint Test (RBAC)**

| Endpoint | Method | Required Role | Headers |
| :--- | :--- | :--- | :--- |
| `/api/products` | `POST` | `admin` | `Authorization: Bearer <JWT>` |

* *Note: If you are testing, ensure your logged-in user's role is manually updated in the PostgreSQL database to `admin` before attempting to create a product.*

**B. Advanced Querying (Public)**

| Action | Method | Endpoint Example |
| :--- | :--- | :--- |
| Filter & Sort | `GET` | `/api/products?category=Electronics&sort=-price` |
| Pagination | `GET` | `/api/products?page=2&limit=10` |

### Step 3: Asynchronous Stock Update (RabbitMQ Test)

This demonstrates the decoupled messaging workflow.

| Action | Method | Endpoint | Headers | Body (Example) |
| :--- | :--- | :--- | :--- | :--- |
| Mock Order | `POST` | `/api/products/mock-order` | `Authorization: Bearer <ADMIN_JWT>` | `{"productId": "...", "quantity": 3}` |

* This request sends an order event to RabbitMQ. The `product-service` consumer will pick up the event and update the stock *asynchronously*. Check the service logs to see the stock change happen in the background.

## 🤝 Contribution

Feel free to fork the repository and submit pull requests.