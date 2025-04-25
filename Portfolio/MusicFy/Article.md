## Project Overview

MusicFy is a full-stack mobile music streaming application built using bare React Native, with a backend powered by Node.js, Express, and MongoDB. Designed as a course project, this app was developed to deepen my understanding of mobile app development beyond the Expo framework.

MusicFy enables authenticated users to upload, manage, and stream music, offering features such as email verification, password reset, playlists, favorites, listening history, and user following. To enhance performance, the app utilizes the mobile device’s caching system to store audio files locally for smoother playback.

## Project Goals

The primary goal of MusicFy was to gain hands-on experience in building a full-stack mobile application from the ground up using bare React Native, beyond the convenience of Expo. This project served both as a practical learning experience and a portfolio-worthy app with real-world features.

### Key Objectives:

* Explore bare React Native to understand native module integration and app deployment processes.
* Implement secure user authentication, including email verification and password reset functionality.
* Enable music uploads and streaming, including support for audio files and cover images.
* Support playlist creation and management, favorites, and user-curated content.
* Build a simple recommendation system based on users’ listening history.
* Optimize app performance by using the device’s local cache for storing and streaming audio files.
* Design a scalable backend API using Node.js, Express, and MongoDB.

## Tech Stack

MusicFy was built with a robust and modern full-stack setup, combining essential tools for mobile development, backend services, state management, and deployment.

### Frontend (Mobile App)

- React Native  – for building cross-platform mobile apps without Expo

- React Navigation – for handling navigation between screens

- Redux – for managing global app state

- React Query – for efficient data fetching, caching, and syncing with the server

- Axios – for making HTTP requests to the backend API

- AsyncStorage – for local caching of audio files for performance

### Backend

* Node.js + Express – for building the RESTful API and handling server logic
* MongoDB + Mongoose – as the NoSQL database and ODM for storing and managing users, music entries, playlists, and more
* Formidable – for handling file uploads (music and cover images)
* Nodemailer – for sending email verification and password reset links
* JWT (JSON Web Tokens) – for authentication and secure session management
* Bcrypt – for password hashing

### Deployment & DevOps

* Docker – for containerizing the backend application for consistent and portable deployment
* Caddy – as a web server and reverse proxy with automatic HTTPS support for serving the backend API

## What I Learned

Building MusicFy was a hands-on journey that helped me grow both as a developer and as a problem solver. By stepping outside of Expo and using bare React Native, I gained deeper insight into how mobile apps interact with native components and system-level features.

### Key Takeaways:

* Bare React Native Experience: I learned how to configure native iOS and Android environments, link native modules manually, and handle platform-specific setups without relying on Expo.
* State Management Strategies: Working with Redux and React Query taught me how to manage global state and server state efficiently, especially in a complex app with authentication, playlists, and recommendations.
* Full-Stack Integration: I built a complete backend system with Node.js, Express, and MongoDB, which helped me understand how to structure APIs, handle file uploads, and manage user authentication securely.
* Authentication Flows: Implementing email verification and password reset mechanisms gave me real-world experience with user flows and secure token management.
* Audio Caching: Leveraging the device’s local file system improved performance and taught me how caching strategies can enhance the mobile user experience.
* DevOps & Deployment: Using Docker and Caddy introduced me to modern deployment practices, including containerization, reverse proxy configuration, and HTTPS setup.

This project strengthened my confidence in building scalable, real-world applications and deepened my understanding of the full development lifecycle—from design to deployment.

## Future Improvements

While MusicFy is fully functional on both Android and iOS devices, there are several enhancements and optimizations I plan to implement to improve both the user experience and the system architecture.

### Technical Enhancements

* Automated Testing
   Introduce unit, integration, and end-to-end testing to ensure stability and prevent regressions.
* CI/CD Pipeline
   Set up continuous integration and deployment to automate testing, builds, and releases.
* Redis / CDN Caching
   Use Redis or a Content Delivery Network to cache frequently accessed audio and metadata for better performance and reduced server load.
* Rate Limiting & Load Balancing
   Add security and scalability by implementing request rate limiting and using load balancers for high availability.

### User Experience Features

* Localization / Internationalization
   Add multi-language support to make the app accessible to users from different regions.
* Lyrics Support
   Allow users to view synced or static lyrics while listening to songs for a more immersive experience.
* Enhanced Recommendation Engine
   Improve the existing recommendation system with more advanced algorithms (e.g., collaborative filtering or ML-based suggestions).
* User Notifications
   Add push notifications for follows, playlist updates, or featured tracks.
* Social Sharing
   Let users share songs or playlists outside the app, enhancing discoverability.

These future additions will help make MusicFy more polished, scalable, and user-friendly across platforms.

## Live Demo & Source Code