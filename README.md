# MessagingService
This real-time messaging service uses Java, Spring Boot, WebSocket, and STOMP, with REST APIs for user registration, OTP verification, and JWT-based authentication. It supports real-time communication, online/offline detection, reliable message delivery, and a lightweight HTML-based chat client for testing.

📬 Real-Time Messaging Service (Spring Boot + WebSocket + JWT)

A lightweight real-time chat system built with Spring Boot, WebSocket (STOMP/SockJS), JWT authentication, and in-memory message queuing for offline message delivery.

🚀 Features
- 🔐 JWT-based authentication
- 🧵 WebSocket communication using STOMP over SockJS
- 📥 Message delivery to online users
- 🕒 Message queuing for offline users
- 📜 In-memory message storage (no database)
- 📦 REST APIs for registration & login
- 🌐 Frontend using plain HTML + JavaScript

📁 Project Structure
messaging/
├── src/main/java/com/example/messaging/
│   ├── auth/                     # JWT filter & utility
│   ├── config/                   # Security and WebSocket config
│   ├── controller/               # REST and WebSocket endpoints
│   ├── listener/                 # WebSocket connection listener
│   ├── model/                    # User and Message models
│   ├── service/                  # Messaging logic
│   ├── storage/                  # In-memory user/message storage
│   └── MessagingApplication.java # Main Spring Boot app
├── src/main/resources/static/
│   ├── chat.html                 # Chat UI
│   ├── login.html                # Login UI
│   └── chat.js                   # WebSocket frontend logic
└── README.txt

⚙️ Prerequisites
- Java 17+
- Maven
- IDE (e.g., IntelliJ, VS Code)
- Browser (for testing the frontend)

🔧 Setup Instructions

1. Clone the project
git clone https://github.com/your-repo/messaging.git
cd messaging

2. Build the project
mvn clean install

3. Run the server
mvn spring-boot:run

Spring Boot will start on: http://localhost:8080

🧪 How to Use

1. Register a user on postman
POST http://localhost:8080/register
{
  "username": "user1",
  "password": "pass1"
}

2. Verify
POST http://localhost:8080/verify?username=user1&otp=123456

3. Login and obtain JWT
POST http://localhost:8080/login
{
  "username": "user1",
  "password": "pass1"
}

Response:
{
  "token": "eyJhbGciOiJIUzI1NiIsInR..."
}

The token is stored in localStorage on successful login.

3. Start chatting
- Open http://localhost:8080/login.html
- Open http://localhost:8080/chat.html
- Enter a message and recipient username
- Messages to offline users are queued and delivered when they reconnect

🔄 WebSocket Flow
- Client connects to /ws using SockJS
- Authenticated with JWT via Authorization header
- Subscribes to /user/queue/messages
- Messages sent via /app/chat

🧠 Where Are Messages Stored?
Messages are stored in-memory using a ConcurrentHashMap<String, Queue<Message>> in MessagingService.java.
Offline messages are delivered automatically when the recipient connects.

🛠️ Technologies Used
- Spring Boot
- Spring Security
- JWT (JSON Web Token)
- STOMP over WebSocket + SockJS
- HTML, CSS, JavaScript
- No database (in-memory only)

✅ Todo
- Add persistent DB support (e.g., H2 or PostgreSQL)
- User registration UI
- Message history

