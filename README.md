# API Documentation: TaskFlow SDK

**Version:** 1.0  
**Author:** TaskFlow Inc.  
**Last Updated:** December 2024  

---

## Overview  
The TaskFlow SDK is designed to help developers seamlessly integrate task management features into their applications. Whether you're developing a web or mobile app, this SDK enables real-time task creation, assignment, status updates, and user collaboration. The SDK supports a variety of features from task categorization to notifications, offering an intuitive solution for managing tasks across teams and organizations. It’s perfect for project management systems, collaborative tools, or productivity apps that require task tracking and team collaboration.

---

## Getting Started

### Prerequisites  
Before integrating the TaskFlow SDK, ensure that your development environment meets the following prerequisites:

- Node.js (for web applications) or the required mobile SDKs (for iOS/Android applications).
- A valid **API Key** from the TaskFlow platform, which can be obtained through the platform's dashboard.
- Access to the **TaskFlow admin console** to configure tasks, assign roles, and set up permissions for users.

### Installation

**For Web Applications:**  
To begin using the SDK for web apps, install it via npm:
```bash
npm install taskflow-sdk
```

**For Mobile Applications (React Native):**  
For mobile apps built with React Native, use the following npm package:
```bash
npm install react-native-taskflow-sdk
```

### Authentication  
Authentication is required for secure access to TaskFlow’s services. Initialize the SDK by passing your API Key to authenticate your requests.

```javascript
const taskFlow = new TaskFlowSDK({ apiKey: 'YOUR_API_KEY' });
```

---

## API Endpoints

### 1. Create a Task  
This endpoint creates a new task within a specified project.

**Endpoint:**  
`POST /api/v1/tasks/create`

**Request Parameters:**

- `projectId` (string): The ID of the project to which the task belongs.
- `title` (string): The title of the task.
- `description` (string): A detailed description of the task.
- `assignedTo` (string, optional): The user ID of the person to whom the task is assigned.
- `dueDate` (string, optional): The due date for the task (ISO 8601 format).

**Example Request:**
```javascript
taskFlow.tasks.create({
  projectId: 'proj_001',
  title: 'Implement Authentication Feature',
  description: 'Create authentication endpoints using JWT.',
  assignedTo: 'user_001',
  dueDate: '2024-12-25'
});
```

**Response:**
```json
{
  "status": "success",
  "taskId": "task_001",
  "message": "Task created successfully."
}
```

### 2. Update Task Status  
This endpoint allows you to update the status of an existing task.

**Endpoint:**  
`PATCH /api/v1/tasks/update_status`

**Request Parameters:**

- `taskId` (string): The ID of the task whose status needs to be updated.
- `status` (string): The new status of the task (e.g., `In Progress`, `Completed`, `On Hold`).

**Example Request:**
```javascript
taskFlow.tasks.updateStatus({
  taskId: 'task_001',
  status: 'In Progress'
});
```

**Response:**
```json
{
  "status": "success",
  "message": "Task status updated successfully."
}
```

---

### Real-Time Notifications with WebSockets  
The TaskFlow SDK provides real-time updates through WebSockets. This feature ensures that your app stays synchronized with changes in task status, user activity, and other events.

#### Establishing a WebSocket Connection
You can establish a WebSocket connection by providing your API key and the project ID. This will allow your application to listen for real-time updates like task status changes, new task assignments, and more.

```javascript
const socket = taskFlow.socket.connect({
  apiKey: 'YOUR_API_KEY',
  projectId: 'proj_001'
});

socket.on('task_updated', (task) => {
  console.log('Task updated:', task);
});

socket.on('task_assigned', (task) => {
  console.log('New task assigned:', task);
});
```

---

## User Management

### 1. Add a User to a Project  
This endpoint allows you to add a user to a project, giving them access to its tasks.

**Endpoint:**  
`POST /api/v1/projects/add_user`

**Request Parameters:**

- `projectId` (string): The ID of the project to which the user is being added.
- `userId` (string): The ID of the user being added.

**Example Request:**
```javascript
taskFlow.projects.addUser({
  projectId: 'proj_001',
  userId: 'user_002'
});
```

**Response:**
```json
{
  "status": "success",
  "message": "User added to project successfully."
}
```

---

## Features

- **Task Assignment & Tracking:** Assign tasks to specific team members and track their progress in real-time.
- **Real-Time Notifications:** Receive real-time updates on task status changes, user activity, and project milestones.
- **Customizable Task Categories:** Group tasks into customizable categories based on the project’s needs (e.g., `Bug`, `Feature`, `Enhancement`).
- **Team Collaboration:** Enable teams to collaborate through comments, file uploads, and task dependencies.

---

## Troubleshooting

### 1. Invalid API Key  
If you encounter a `401 Unauthorized` error, verify that your API key is valid and that it has the necessary permissions. You can regenerate the key from the TaskFlow dashboard if necessary.

### 2. WebSocket Connection Issues  
If you're having trouble establishing a WebSocket connection, ensure that your server supports WebSocket connections and that the API key and project ID are correct. If the connection still fails, use fallback mechanisms like long-polling to handle updates.

---

## Conclusion  
The TaskFlow SDK simplifies the process of building task management and project tracking features into your applications. With real-time updates, detailed task tracking, and seamless user collaboration tools, the SDK is a versatile solution for enhancing productivity and streamlining team workflows.  

For more information, in-depth examples, and troubleshooting, refer to the full documentation or contact our support team.
