### Project Overview

Linze.pro is a platform designed to showcase my portfolio projects and share technical articles with a wider audience. As the admin, I can create, edit, and manage blog posts, while users can browse the posts, view information about me, and explore my portfolio. The blog supports both English and Chinese, allowing users to filter posts by tags.

### Motivation

The purpose of this project was to build a versatile platform where I could not only display my personal and technical achievements but also engage with an audience by sharing valuable technical insights. I wanted to create an intuitive, multi-language blog with a clean, responsive UI and a robust backend to demonstrate my skills in full-stack development, including frontend, backend, and deployment.

### My Role

As the sole developer on this project, I designed and implemented both the frontend and backend. I utilized Vue.js and Tailwind CSS to create an intuitive and aesthetically pleasing user interface. On the backend, I used Golang with the chi framework to build a secure, scalable, and efficient API. I also integrated PostgreSQL as the database and took care of the server-side logic, including authentication, caching, and performance optimization.

### Development Process

Frontend Development: I started by designing a user-friendly frontend with Vue.js and Tailwind CSS, ensuring that the UI was responsive and supported both English and Chinese.

Backend Development: I used Golang and the chi framework for building the RESTful API. Key features like JWT authentication, CORS strategies, and rate-limiting middleware were integrated for security and scalability.

Database Design: PostgreSQL was chosen for its reliability and scalability. I structured the database to handle blog posts, tags, user statistics (likes, views), and multilingual content.

Deployment & DevOps: The app is dockerized for easy deployment using Docker and Docker Compose. Caddy is used as a reverse proxy and static file server. I set up GitHub actions for continuous integration and delivery.

Testing & Debugging: To ensure the app’s reliability, I implemented unit and integration tests, while logging and monitoring were set up to keep track of any issues.

### Tech Stack

Frontend: Vue.js, Tailwind CSS

Backend: Golang, chi

Database: PostgreSQL

Middleware & Libraries: JWT-based Authentication, Redis Caching, Rate Limiting, CORS, Logging

DevOps & Deployment: Docker, Docker Compose, GitHub Actions, Caddy (Static File Server & Reverse Proxy)

Documentation: API Swagger for Backend Documentation

### Key Features

Multilingual Support: The app supports both English and Chinese, making it accessible to a wider audience.

Tag-based Filtering: Users can filter posts by tags, allowing them to find content relevant to their interests.

Blog Post Statistics: Each post shows the number of views and likes, giving both readers and the admin insights into content popularity.

Admin Controls: The admin can create and edit blog posts, manage content, and view analytics.

### Challenges & Solutions

Multilingual Content Management: Handling different languages for the blog posts required careful database design and logic. I used a flexible model in PostgreSQL to support translations without complexity.

Security and Performance: Implementing secure JWT authentication and strict CORS policies, as well as optimizing performance with Redis caching, were critical for both security and the user experience.

### What I Learned

Full-Stack Development: I deepened my knowledge in building full-stack applications, especially with Golang as a backend technology.

Deployment Practices: I learned how to effectively use Docker for containerizing applications and setting up efficient deployment pipelines with GitHub Actions.

Multilingual App Design: Creating a multilingual platform helped me understand the complexities of handling multiple languages in both frontend and backend.

### Future Improvements

SEO Optimization: Improving search engine optimization (SEO) for better discoverability of blog posts.

### Summary

This Blog app serves as a platform to showcase my portfolio, share insights, and interact with readers. By using modern technologies like Vue.js, Golang, and Docker, I have built a secure, scalable, and feature-rich app. This project reflects my ability to manage both frontend and backend development while utilizing industry-standard tools and practices.
