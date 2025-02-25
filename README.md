# Software Pattern Architecture

## Introduction
Software architecture patterns define the structure and interaction of components in an application. They help in organizing code efficiently, improving scalability, maintainability, and performance.

## Common Software Architecture Patterns

### 1. Layered Architecture (N-Tier)
#### Overview:
A hierarchical approach where the application is divided into separate layers, each with a specific responsibility.

#### Layers:
- **Presentation Layer**: Handles UI (React)
- **Business Logic Layer**: Implements business rules (Express.js/Node.js)
- **Data Access Layer**: Interacts with the database (MongoDB)

#### When to Use:
- Suitable for enterprise applications
- When separation of concerns is needed
- Easy to test and maintain

#### Example (MERN Stack):
```javascript
// Controller Layer (Express.js)
const Product = require("../models/Product");
exports.getProducts = async (req, res) => {
  const products = await Product.find();
  res.json(products);
};

// Service Layer (Business Logic)
exports.addProduct = async (productData) => {
  return await Product.create(productData);
};

// Data Layer (MongoDB Model)
const mongoose = require("mongoose");
const productSchema = new mongoose.Schema({
  name: String,
  price: Number,
});
module.exports = mongoose.model("Product", productSchema);
```

### 2. MVC (Model-View-Controller)
#### Overview:
Separates an application into three interconnected parts:
- **Model**: Manages data (MongoDB, Mongoose models)
- **View**: UI representation (React components)
- **Controller**: Handles requests and responses (Express.js)

#### When to Use:
- Web applications needing separation of UI and logic
- Scalable and maintainable projects

#### Example (MERN Stack):
```javascript
// Model (MongoDB)
const mongoose = require("mongoose");
const UserSchema = new mongoose.Schema({
  username: String,
  email: String,
});
module.exports = mongoose.model("User", UserSchema);

// Controller (Express.js)
const User = require("../models/User");
exports.getUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};

// View (React)
const UserList = ({ users }) => (
  <ul>
    {users.map((user) => (
      <li key={user._id}>{user.username}</li>
    ))}
  </ul>
);
```

### 3. Microservices Architecture
#### Overview:
Breaks an application into small, loosely coupled services that communicate via APIs.

#### When to Use:
- Large-scale applications
- Systems requiring independent deployment and scaling

#### Example (MERN Stack):
```javascript
// Product Service (Express.js)
const express = require("express");
const app = express();
app.get("/products", async (req, res) => {
  res.json(await Product.find());
});
app.listen(5001, () => console.log("Product Service running"));

// User Service (Express.js)
const userApp = express();
userApp.get("/users", async (req, res) => {
  res.json(await User.find());
});
userApp.listen(5002, () => console.log("User Service running"));
```

### 4. Event-Driven Architecture
#### Overview:
Components communicate asynchronously using events and message queues.

#### When to Use:
- Real-time applications (chat, notifications)
- Systems needing decoupled communication

#### Example (MERN Stack - Using RabbitMQ for Messaging):
```javascript
// Publisher (Node.js)
const amqp = require("amqplib/callback_api");
amqp.connect("amqp://localhost", (err, connection) => {
  connection.createChannel((err, channel) => {
    const queue = "orderQueue";
    channel.assertQueue(queue, { durable: false });
    channel.sendToQueue(queue, Buffer.from("New order placed"));
  });
});

// Subscriber (Node.js)
amqp.connect("amqp://localhost", (err, connection) => {
  connection.createChannel((err, channel) => {
    const queue = "orderQueue";
    channel.assertQueue(queue, { durable: false });
    channel.consume(queue, (msg) => {
      console.log("Received:", msg.content.toString());
    }, { noAck: true });
  });
});
```

### 5. Serverless Architecture
#### Overview:
Applications run on cloud functions without managing servers.

#### When to Use:
- Cost-efficient for sporadic workloads
- Scalable backend without infrastructure management

#### Example (AWS Lambda + Express):
```javascript
const serverless = require("serverless-http");
const express = require("express");
const app = express();
app.get("/hello", (req, res) => {
  res.json({ message: "Hello from AWS Lambda!" });
});
module.exports.handler = serverless(app);
```

### 6. Repository Pattern
#### Overview:
Separates business logic from data access, ensuring decoupled and testable code.

#### When to Use:
- When reusing data access logic
- When simplifying data management

#### Example (MERN Stack):
```javascript
class UserRepository {
  async getAllUsers() {
    return await User.find();
  }
  async createUser(data) {
    return await User.create(data);
  }
}
```

### 7. Singleton Pattern
#### Overview:
Ensures a class has only one instance and provides a global access point.

#### When to Use:
- Managing configurations
- Database connections

#### Example (MERN Stack):
```javascript
class Database {
  constructor() {
    if (!Database.instance) {
      this.connection = mongoose.connect("mongodb://localhost:27017/mydb");
      Database.instance = this;
    }
    return Database.instance;
  }
}
module.exports = new Database();
```

## Additional Patterns
- **Factory Pattern**
- **Adapter Pattern**
- **Decorator Pattern**
- **Observer Pattern**
- **Strategy Pattern**
- **Proxy Pattern**
- **Builder Pattern**
- **Mediator Pattern**
- **Command Pattern**

## Most Used Patterns in Companies and Projects
- **Microservices Architecture**
- **MVC Pattern**
- **Singleton Pattern**
- **Factory Pattern**
- **Repository Pattern**
- **Event-Driven Architecture**
- **Serverless Architecture**

## Conclusion
Choosing the right architecture pattern depends on the project requirements, scalability, and maintainability needs. Each pattern has its benefits, and using the right one ensures a robust and efficient system.
