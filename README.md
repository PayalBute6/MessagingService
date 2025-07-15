#Real-Time Messaging Service
A lightweight real-time chat system built with Spring Boot, WebSocket (STOMP/SockJS), JWT authentication, and in-memory message queuing for reliable offline message delivery.

🚀 Features

🔐 JWT-based authentication with OTP verification
🧵 WebSocket communication using STOMP over SockJS
📥 Instant message delivery to online users
🕒 Message queuing for offline users with automatic delivery on reconnect
📜 In-memory storage for users and messages (no database required)
📦 REST APIs for user registration, OTP verification, and login
🌐 Frontend chat client using plain HTML + JavaScript
🔄 Online/offline detection via WebSocket connection tracking

📁 Project Structure
messaging/
├── src/main/java/com/example/messaging/
│   ├── auth/                     # JWT utilities and filters
│   │   ├── JwtUtil.java         # JWT token generation/validation
│   │   └── JwtAuthFilter.java   # JWT authentication filter
│   ├── config/                   # Configuration classes
│   │   ├── SecurityConfig.java  # Spring Security configuration
│   │   └── WebSocketConfig.java # WebSocket and STOMP configuration
│   ├── controller/               # REST and WebSocket endpoints
│   │   ├── AuthController.java  # Registration, login, OTP verification
│   │   └── MessageController.java # WebSocket message handling
│   ├── model/                    # Data models
│   │   ├── User.java            # User model
│   │   ├── ChatMessage.java     # Message model
│   │   └── LoginRequest.java    # Login request DTO
│   ├── service/                  # Business logic
│   │   ├── UserService.java     # User management
│   │   └── MessageService.java  # Message handling and queuing
│   ├── storage/                  # In-memory storage
│   │   └── UserStorage.java     # User storage management
│   └── MessagingApplication.java # Main Spring Boot application
├── src/main/resources/static/
│   ├── chat.html                 # Main chat interface
│   ├── login.html                # Login interface
│   └── favicon.ico               # Application icon
└── pom.xml                       # Maven dependencies
⚙️ Prerequisites

Java 17+
Maven 3.6+
IDE (IntelliJ IDEA, VS Code, or Eclipse)
Modern browser (Chrome, Firefox, Safari)

🔧 Setup Instructions

Clone the repository
bashgit clone <repository-url>
cd messaging

Build the project
bashmvn clean install

Run the application
bashmvn spring-boot:run

Access the application

Server: http://localhost:8080
Login page: http://localhost:8080/login.html
Chat page: http://localhost:8080/chat.html



🧪 API Usage
1. User Registration
httpPOST http://localhost:8080/register
Content-Type: application/json

{
  "username": "user1",
  "password": "password123"
}

2. OTP Verification
httpPOST http://localhost:8080/verify?username=user1&otp=123456

3. User Login
httpPOST http://localhost:8080/login
Content-Type: application/json

{
  "username": "user1",
  "password": "password123"
}
Response:
json{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
🔄 WebSocket Message Flow
Connection Process:

Client connects to /ws endpoint using SockJS
Authenticates with JWT token in Authorization header
Subscribes to /user/queue/messages for receiving messages
Sends connection notification to /app/connect

Message Sending:
javascript// Send message to another user
stompClient.send("/app/send", {}, JSON.stringify({
    receiver: "user2",
    text: "Hello there!"
}));
Message Format:
json{
  "sender": "user1",
  "receiver": "user2",
  "text": "Hello there!",
  "timestamp": "2025-07-15T10:30:00Z"
}
📊 Message Delivery Logic
Online User:

Message delivered immediately via WebSocket
Real-time notification to receiver

Offline User:

Message queued in memory grouped by receiver
Automatic delivery when user reconnects
Messages delivered in correct order

Reconnection:

All queued messages delivered instantly
User marked as online
Live message delivery resumed

🛠️ Key Components
Authentication

JWT tokens for stateless authentication
OTP verification for enhanced security
WebSocket authentication via STOMP headers

Message Storage

In-memory queuing using ConcurrentHashMap<String, Queue<ChatMessage>>
Thread-safe operations for concurrent users
Automatic cleanup of empty queues

Connection Management

Online user tracking with ConcurrentHashMap.newKeySet()
WebSocket session management via Spring STOMP
Graceful disconnection handling

🧠 Testing Scenarios
1. Real-time Messaging

Open chat in two browsers with different users
Send messages between them
Verify instant delivery

2. Offline Message Queuing

User A sends message to offline User B
User B connects later
Verify message is delivered on connection

3. Multiple Message Delivery

Multiple users send messages to offline user
Verify all messages delivered in correct order
Check message timestamps

4. Connection Recovery

Disconnect and reconnect user
Verify queued messages are delivered
Confirm real-time messaging resumes

🔒 Security Features

JWT token validation on every WebSocket message
CORS configuration for cross-origin requests
CSRF protection disabled for WebSocket endpoints
Stateless session management
Authorization header authentication

🌐 Frontend Features

Real-time chat interface with message history
Online/offline status indicators
Message timestamps and sender identification
Auto-scroll for new messages
Enter key support for sending messages
JWT token storage in localStorage

🛡️ Technologies Used

Spring Boot 3.x - Application framework
Spring Security - Authentication and authorization
Spring WebSocket - Real-time communication
STOMP - WebSocket protocol
SockJS - WebSocket fallback support
JWT - JSON Web Tokens for authentication
Maven - Dependency management
HTML/CSS/JavaScript - Frontend implementation

📝 Configuration
WebSocket Configuration

Endpoint: /ws with SockJS fallback
Message broker: /queue and /topic prefixes
Application prefix: /app
User prefix: /user

Security Configuration

Public endpoints: Login, registration, static files, WebSocket
Protected endpoints: All other REST APIs
JWT filter: Validates tokens on protected endpoints
CORS: Allows all origins (configure for production)

✅ Future Enhancements

 Database persistence (H2, PostgreSQL, MongoDB)
 Message history with pagination
 User registration UI with form validation
 Group chat functionality
 Message read receipts
 File sharing capabilities
 Message encryption
 User presence indicators
 Message search functionality
 Push notifications

🚀 Deployment
Development
bashmvn spring-boot:run
Production
bashmvn clean package
java -jar target/messaging-1.0.0.jar
📞 Support
For issues or questions, please check the application logs and ensure:

JWT tokens are properly formatted
WebSocket connection is established
User authentication is successful
Message format follows the expected schema


Built with using Spring Boot and WebSocket technology
