# Real-Time Chat Application

## Overview
This project is a **real-time chat application** built using **React.js** for the frontend and **Node.js with WebSockets** for the backend. The app enables instant messaging between users with a clean and responsive UI.

## Features
- **Real-time communication** using WebSockets
- **Beautiful and responsive UI** with CSS styling
- **Instant message updates** without page refresh
- **Multiple users can chat simultaneously**

## Technologies Used
### Frontend:
- React.js
- Socket.io-client
- HTML & CSS

### Backend:
- Node.js
- Express.js
- Socket.io
- CORS

## Installation and Setup

### 1️⃣ Setting Up the React Frontend
#### **Step 1: Create a React Project**
```sh
npx create-react-app chat-app
cd chat-app
```
#### **Step 2: Install Dependencies**
```sh
npm install socket.io-client
```
#### **Step 3: Add the React Chat Component**
Inside `src/`, create a file **`ChatApp.js`** and paste:
```javascript
import { useState, useEffect } from "react";
import io from "socket.io-client";
import "./ChatApp.css";

const socket = io("http://localhost:5000");

export default function ChatApp() {
  const [message, setMessage] = useState("");
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    socket.on("message", (data) => {
      setMessages((prev) => [...prev, data]);
    });
    return () => socket.off("message");
  }, []);

  const sendMessage = () => {
    if (message.trim()) {
      socket.emit("message", message);
      setMessage("");
    }
  };

  return (
    <div className="chat-container">
      <div className="chat-box">
        <h2 className="chat-header">Real-Time Chat</h2>
        <div className="chat-messages">
          {messages.map((msg, index) => (
            <div key={index} className="chat-message">{msg}</div>
          ))}
        </div>
        <div className="chat-input-container">
          <input
            className="chat-input"
            type="text"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            placeholder="Type a message..."
          />
          <button className="chat-send-btn" onClick={sendMessage}>Send</button>
        </div>
      </div>
    </div>
  );
}
```

#### **Step 4: Create a CSS File for Styling**
Inside `src/`, create **`ChatApp.css`** and add:
```css
.chat-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f4f4f4;
}

.chat-box {
  width: 400px;
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}

.chat-header {
  text-align: center;
  color: #4CAF50;
}

.chat-messages {
  height: 300px;
  overflow-y: auto;
  border-bottom: 1px solid #ddd;
  margin-bottom: 10px;
  padding: 10px;
}

.chat-message {
  background: #e1ffc7;
  padding: 8px;
  border-radius: 5px;
  margin-bottom: 5px;
}

.chat-input-container {
  display: flex;
  gap: 10px;
}

.chat-input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.chat-send-btn {
  background: #4CAF50;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.3s;
}

.chat-send-btn:hover {
  background: #45a049;
}
```

#### **Step 5: Add the Component to `App.js`**
Replace the content in `src/App.js` with:
```javascript
import ChatApp from "./ChatApp";

function App() {
  return (
    <div>
      <ChatApp />
    </div>
  );
}

export default App;
```

### 2️⃣ Setting Up the Node.js Backend

#### **Step 1: Create the Backend Folder**
```sh
mkdir server
cd server
```
#### **Step 2: Install Dependencies**
```sh
npm init -y
npm install express socket.io cors
```
#### **Step 3: Create `server.js` and Add This Code**
```javascript
const express = require("express");
const http = require("http");
const { Server } = require("socket.io");
const cors = require("cors");

const app = express();
const server = http.createServer(app);
const io = new Server(server, {
  cors: {
    origin: "http://localhost:3000",
    methods: ["GET", "POST"],
  },
});

app.use(cors());

io.on("connection", (socket) => {
  console.log("New user connected:", socket.id);

  socket.on("message", (msg) => {
    io.emit("message", msg);
  });

  socket.on("disconnect", () => {
    console.log("User disconnected:", socket.id);
  });
});

server.listen(5000, () => {
  console.log("Server running on port 5000");
});
```

### 3️⃣ Running the Application
#### **Step 1: Start the Backend**
```sh
node server.js
```
#### **Step 2: Start the Frontend**
```sh
cd ../chat-app
npm start
```

### 4️⃣ Expected Output
- A clean **chat UI** with instant message updates.
- Messages will appear in **real-time** between multiple users.
