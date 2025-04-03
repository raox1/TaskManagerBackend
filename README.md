# Task Manager Server

A backend server for the Task Manager app, designed to store and manage tasks in the cloud. Built with Node.js and MongoDB, this server provides RESTful APIs to create, read, update, and delete tasks, enabling synchronization with the Task Manager Flutter app.

Developed by **Lalit Kumar**.

---

## Features

- **RESTful APIs**: Endpoints for CRUD operations on tasks (Create, Read, Update, Delete).
- **MongoDB Storage**: Persistent storage of tasks in a NoSQL database.
- **Task Synchronization**: Supports syncing tasks with the Task Manager Flutter app.
- **Scalable**: Lightweight and extendable for additional features (e.g., user authentication).
- **Error Handling**: Basic error responses for invalid requests.

---

## Project Structure

### Database Structure

The server uses **MongoDB**, a NoSQL database, to store tasks. Here’s how it’s structured:

- **Collection**: A single MongoDB collection named `tasks` stores all task data.
- **Schema**: Each task document follows this structure:
  - `_id` (ObjectId): Auto-generated unique identifier for each task.
  - `title` (String): The task’s title (required).
  - `description` (String, optional): An optional description.
  - `dueDate` (Date): The task’s due date, stored as an ISO date string.
  - `priority` (String): Priority level ("Low", "Medium", "High").
- **Storage**: Tasks are stored as JSON-like documents in the `tasks` collection.

**Why MongoDB?**
- Flexible schema, ideal for evolving app requirements.
- Scalable for cloud deployment.
- Easy integration with Node.js via Mongoose.

### File Structure

```
task_manager_server/
├── config/
│   └── db.js               # MongoDB connection configuration
├── models/
│   └── Task.js             # Mongoose schema for tasks
├── routes/
│   └── tasks.js            # API routes for task operations
├── .env                    # Environment variables (e.g., MongoDB URI)
├── server.js               # Main server entry point
├── package.json            # Dependencies and scripts
└── README.md               # This file
```

---

## Dependencies

Here are the dependencies used in `package.json`, along with their purposes:

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^8.0.0",
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

### Reasons for Each Dependency

- **express**: Provides a minimal framework to build RESTful APIs quickly and efficiently.
- **mongoose**: Simplifies MongoDB interactions with schemas and models, ensuring data consistency.
- **dotenv**: Loads sensitive configuration (e.g., MongoDB URI) from a `.env` file, keeping it out of version control.
- **nodemon**: Restarts the server automatically on file changes during development, improving workflow.

---

## How to Run the Server

### Prerequisites

- Node.js (version 16.x or higher recommended)
- MongoDB (local instance or cloud service like MongoDB Atlas)
- npm (included with Node.js)

### Setup

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/task_manager_server.git
   cd task_manager_server
   ```

2. **Install Dependencies**:
   ```bash
   npm install
   ```

3. **Configure Environment**:
   - Create a `.env` file in the root directory:
     ```
     MONGODB_URI=mongodb://localhost:27017/task_manager
     PORT=3000
     ```
   - Replace `MONGODB_URI` with your MongoDB connection string (e.g., from MongoDB Atlas if using cloud).

4. **Run the Server**:
   - Development mode (with auto-restart):
     ```bash
     npm run dev
     ```
   - Production mode:
     ```bash
     npm start
     ```

5. **Verify**:
   - Open a browser or tool like Postman and visit `http://localhost:3000/tasks`.
   - You should see an empty array `[]` if no tasks exist yet.

---

## API Endpoints

- **GET /tasks**: Retrieve all tasks.
- **POST /tasks**: Create a new task.
- **GET /tasks/:id**: Retrieve a specific task by ID.
- **PUT /tasks/:id**: Update a task by ID.
- **DELETE /tasks/:id**: Delete a task by ID.

---

## Connecting to the Flutter App

To integrate this server with the Task Manager Flutter app (currently using Hive), follow these steps:

1. **Add HTTP Dependency**:
   ```yaml
   dependencies:
     http: ^1.1.0
   ```
   Run:
   ```bash
   flutter pub get
   ```

2. **Update `TaskProvider`**:
   Replace Hive with HTTP calls in `lib/providers/task_provider.dart`.

3. **Update `Task` Model**:
   Ensure `toMap` and `fromMap` methods exist in `lib/models/task.dart`.

4. **Test the Integration**:
   - Start the server (`npm run dev`).
   - Update `_baseUrl` in `TaskProvider` to your server’s address.
   - Run the Flutter app (`flutter run`) and verify tasks sync with the server.

---

## Sample Server Code

### `server.js`
```javascript
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const taskRoutes = require('./routes/tasks');

const app = express();
app.use(express.json());

mongoose.connect(process.env.MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('MongoDB connection error:', err));

app.use('/tasks', taskRoutes);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

## Contributing

Feel free to fork this repository, submit pull requests, or open issues for bugs and feature requests.

---

## License

This project is licensed under the MIT License.
