### **Project Overview**

**Chatify** is a real-time communication app that supports text messaging, audio calls, and video calls. Initially built using Next.js and Convex, I rebuilt the entire backend with Golang and PostgreSQL to overcome performance bottlenecks and gain more control over database operations. The project demonstrates my ability to optimize systems, handle scalability challenges, and integrate modern web technologies.

### **Motivation**

This project started as a course project, but I saw an opportunity to take it further. The original version, while functional, suffered from significant performance issues—mainly due to inefficient database queries and limitations of using a third-party backend-as-a-service. I wanted to challenge myself by improving its architecture, making it more robust, performant, and production-ready.

### **My Role**

I was responsible for the full-stack development of the application. This included:

* Rewriting the backend from scratch using Golang and PostgreSQL.
* Designing and implementing raw SQL queries for optimal performance.
* Managing Docker-based deployment and environment configurations.
* Building real-time chat functionality with custom WebSocket logic in Go.
* Integrating third-party services like Clerk for authentication.
* Maintaining and enhancing the existing frontend using Next.js and Tailwind CSS.

### **Development Process**

1. **Identifying Bottlenecks**: Noticed database bandwidth consumption hitting 2GB in one week and over 400,000 query calls.
2. **Root Cause Analysis**: Discovered the N+1 query problem caused by the Convex backend which doesn't support SQL joins.
3. **System Redesign**: Decided to switch to a custom backend using Golang and PostgreSQL.
4. **Backend Development**:
   * Used raw SQL with `sqlc` to generate type-safe database access code.
   * Managed schema migrations with `golang-migrate`.
   * Leveraged `air` for live-reloading during development.
   * Protected environment variables using `direnv`.
5. **Deployment Setup**: Created a `Dockerfile` and used `docker-compose` to unify the app's services.
6. **Authentication Integration**: Used Clerk and implemented webhook support to store user authentication data in the backend.
7. **Real-Time Communication**: Wrote custom WebSocket logic using `gorilla/websocket` and `goRoutines`.

### **Tech Stack**

* **Frontend**: Next.js, Tailwind CSS
* **Backend**: Golang (with `sqlc`, `air`, `direnv`)
* **Database**: PostgreSQL (schema managed via `golang-migrate`)
* **WebSocket Library**: `gorilla/websocket`
* **Authentication**: Clerk + Webhooks
* **Containerization & Deployment**: Docker, docker-compose, and Caddy

### **Key Features**

* Real-time chat with message persistence
* Audio and video calling support
* Custom WebSocket implementation using Go
* Secure user authentication via Clerk
* Type-safe database access using raw SQL and `sqlc`
* Docker-based local and production deployment setup

### **Challenges & Solutions**

* **Convex's Limitations**: Unable to solve N+1 queries due to lack of SQL join support → Solved by switching to PostgreSQL and using raw SQL.
* **WebSocket Integration in Go**: Socket.io is Node.js-centric → Solved by writing my own WebSocket logic using `gorilla/websocket` and `goRoutines`.
* **Efficient Development Workflow**: Needed quick iteration during backend development → Solved with tools like `air`, `direnv`, and Docker.

### **What I Learned**

* How to diagnose and resolve N+1 query issues in real-world applications.
* Writing and optimizing raw SQL queries for high-performance APIs.
* Managing backend schema migrations and maintaining environment configurations securely.
* Creating a custom WebSocket server from scratch in Golang.
* End-to-end application deployment with Docker, docker-compose and Caddy.

### **Future Improvements**

1. Add unit and integration testing for backend services.
2. Implement Redis caching to reduce database load.
3. Set up message broker (e.g., NATS or Kafka) for decoupled real-time messaging.
4. Add rate limiting and load balancing for production readiness.
5. Improve error handling and refactor code for better readability.
6. Introduce CI/CD pipeline using Jenkins or GitHub Actions.
7. Add internationalization (i18n) support.
8. Integrate monitoring tools (e.g., Prometheus, Grafana) for system health checks.

### **Summary**

Chatify reflects my journey from identifying critical backend limitations to building a robust and scalable chat system. It showcases my backend development skills, ability to optimize performance, and knowledge of full-stack deployment practices. This project not only met the initial course requirements but also evolved into a professional-grade application through thoughtful redesign and execution.