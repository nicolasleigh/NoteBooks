### Project Overview

**Petify** is a full-stack e-commerce platform designed for buying and selling pets online. It offers a user-friendly shopping experience with features like smart filtering, real-time chat, and detailed order tracking. The platform is divided into three independent parts: a user-facing web app, an admin dashboard, and a backend API server. Each part was built with scalability, usability, and performance in mind. Petify supports real-time communication, dynamic product management, and secure checkout, making it a comprehensive solution for pet commerce.

### Motivation

The main motivation behind building **Petify** was to deepen my understanding of full-stack development and gain hands-on experience with building a complete, real-world web application from scratch. I wanted to challenge myself to work across the entire development stack — from crafting interactive and responsive user interfaces, to building a scalable backend API, and finally deploying the application in a production-ready environment using Docker and Caddy.

By choosing an e-commerce platform as the project type, I was able to explore a wide range of features such as authentication, product filtering, shopping cart logic, real-time messaging, admin tools, and data visualization. This project gave me the opportunity to not only reinforce what I already knew, but also to explore new tools, best practices, and architectural patterns across frontend, backend, and deployment.

### My Role

I was solely responsible for the entire development of **Petify**, from concept to deployment. This included:

* **Frontend Development:** Building responsive user and admin interfaces using React, Redux, and Tailwind CSS.
* **Backend Development:** Designing and implementing a RESTful API with Node.js and Express, handling authentication, file uploads, and real-time chat with Socket.IO.
* **Database Design:** Structuring and managing data using MongoDB, including schema design and data relationships.
* **Deployment & DevOps:** Containerizing the application with Docker and docker-compose, configuring reverse proxy and HTTPS with Caddy, and deploying all services in a unified stack.

This project gave me end-to-end ownership, allowing me to practice problem-solving at every layer of the application and gain valuable experience in full-stack development.

### Tech Stack

**Frontend:**

* **React** – For building dynamic and interactive user interfaces.
* **Redux** – For managing global application state across both the user and admin apps.
* **Tailwind CSS** – For utility-first styling and responsive UI design.

**Backend:**

* **Node.js & Express** – For building the RESTful API and handling server-side logic.
* **MongoDB** – As the NoSQL database for storing users, products, orders, reviews, and chat data.
* **Formidable** – For handling file uploads such as product images.
* **Socket.IO** – For implementing real-time chat between users and admins.

**DevOps & Deployment:**

* **Docker** – To containerize the frontend and backend applications for consistent and portable deployment.
* **docker-compose** – To orchestrate multi-container setups during development and deployment.
* **Caddy** – As a reverse proxy and web server that automatically provides HTTPS and routes requests to the correct services.

### Key Features

#### User App

* **Product Browsing:** Users can explore pets by category and view detailed product information.
* **Smart Search & Filtering:**
  * Search by product name or category.
  * Filter by category, price range, and rating.
  * Sort products by price (ascending/descending).
* **Wishlist & Cart:** Add pets to a wishlist or shopping cart for later viewing or checkout.
* **Order Management:** Place orders, view order details, and track status.
* **Product Reviews:** Leave star ratings and written reviews on product pages.
* **Real-Time Chat:** Communicate instantly with the seller through a real-time chat system powered by Socket.IO.

#### Admin App

* **Dashboard Overview:**
  * View sales-related statistics including total sales, orders, customers, and products.
  * Visual bar chart showing sales and order count per category.
  * Live updates of recent orders and customer messages.
* **Product & Category Management:** Add, edit, and remove categories and product listings.
* **Order Tracking:** Access detailed order and payment information.
* **Customer Chat:** Respond to customer messages in real time.
* **Profile Management:** Update admin profile and change password securely.
* **Advanced Search Tools:**
  * Search products by name or category.
  * Search orders by order ID.

### What I Learned

Building **Petify** was a valuable hands-on experience that allowed me to deepen my understanding of full-stack development. Throughout the process, I learned how to:

* **Design and structure full-stack applications** with a clear separation of concerns across the frontend, backend, and admin interface.
* **Work with real-time technologies** like Socket.IO to build interactive features such as live chat.
* **Manage complex state** across different React apps using Redux.
* **Implement robust API endpoints** and connect them seamlessly to the frontend with secure data flow.
* **Build user-friendly and responsive UIs** using Tailwind CSS with a component-driven design approach.
* **Handle file uploads** and manage media using Formidable on the server side.
* **Containerize applications** with Docker and orchestrate services using docker-compose.
* **Deploy full-stack apps in production**, set up HTTPS, and manage routing using Caddy as a reverse proxy.

This project helped me strengthen both my frontend and backend skills, and gave me practical experience in DevOps and deployment — turning abstract knowledge into real, working solutions.

### Future Improvements

While **Petify** is already a functional and scalable e-commerce platform, there are several improvements and features I plan to implement in the future to enhance its performance, usability, and maintainability:

1. **Testing:** Introduce comprehensive unit, integration, and end-to-end tests to ensure code reliability and stability.
2. **Message Broker:** Implement a message broker (e.g., RabbitMQ or Kafka) to decouple services and improve communication between components.
3. **Load Balancing & Rate Limiting:** Add load balancing for distributing traffic efficiently and implement rate limiting to prevent abuse and ensure fair usage of the API.
4. **Redis Caching:** Integrate Redis for caching frequently accessed data, improving app performance and reducing database load.
5. **Database Optimization:** Apply indexing and other database optimizations to improve query performance, especially for large datasets.
6. **Internationalization (i18n):** Add multi-language support to cater to a global audience and improve accessibility.
7. **React Server-Side Rendering (SSR):** Implement server-side rendering with React to improve SEO and page load performance.
8. **CI/CD Pipeline:** Set up a continuous integration and continuous deployment (CI/CD) pipeline using Jenkins to automate testing, builds, and deployments.
9. **App Monitoring:** Integrate app monitoring tools like Prometheus or New Relic to track performance, identify bottlenecks, and ensure system health.
10. **Better Error Handling & Code Refactoring:** Improve error handling and refactor code for better readability, maintainability, and scalability.
11. **Additional Enhancements:** Implement features like product recommendation systems, advanced analytics for admins, and AI-driven chat support to further enhance user experience.

These improvements will ensure that **Petify** remains a modern, high-performing, and scalable solution as it evolves to meet growing user demands.

### Live Demo & Source Code