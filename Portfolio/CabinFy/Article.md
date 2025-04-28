# Short Explanation

CabinFy is a full-featured accommodation booking platform that connects travelers with unique cabin rentals. It offers an intuitive experience for guests to discover and book cabins, while providing owners with powerful management tools through a secure admin dashboard. Though initially inspired by a React course, I completely rebuilt it with a custom Node.js/Express backend, PostgreSQL database, and modernized UI using Tailwind CSS—transforming it into a production-quality application with multilingual support and responsive design.

# Features

CabinFy is a full-stack web application inspired by Airbnb, designed for booking and managing cabin rentals. It delivers a seamless experience for both guests and administrators, with a strong focus on performance, usability, and modern development practices. Key features include:

- Multilingual Support: Available in both English and Chinese, making the platform accessible to a broader audience.
- Responsive Design & Accessibility: The UI is fully optimized for desktop, tablet, and mobile devices, with built-in dark mode support and accessibility features ensuring a comfortable experience for all users regardless of device or viewing preferences.
- Guest Booking Interface: Users can browse available cabins, view detailed listings, and make bookings through an intuitive and modern interface.
- Admin Dashboard: Admins have access to a secure dashboard where they can:

  * View, manage, and process bookings

  * Submit new cabin listings or edit existing ones

  * Monitor booking activity and cabin availability
- Advanced Data Management: Supports filtering, sorting, and pagination for easy browsing and efficient data interaction
- Authentication & Authorization: Secure user authentication with access control to protect admin functionalities and private data.

# Tech Stack

CabinFy is built using a modern and scalable full-stack architecture, designed with performance, maintainability, and developer experience in mind.

## Core Technologies

**Frontend**

* **React** – Core library for building dynamic user interfaces
* **Tailwind CSS** – Utility-first CSS framework for building fast and responsive UIs
* **React Query** – Selected over Redux for its optimized data fetching and caching capabilities

**Backend**

* **Node.js & Express** – Lightweight yet powerful server framework enabling rapid API development
* **PostgreSQL & Prisma** – Combination provides type-safe database operations without the need to write raw SQL
* **Zod** – Implements thorough runtime validation at API boundaries, preventing data corruption

## Technical Highlights

### Authentication System

Built a secure authentication flow combining Passport.js with custom middleware that integrates seamlessly with Prisma's user model, supporting both traditional and OAuth authentication methods.

### Deployment Infrastructure

* **Docker** – Containerized application components for consistent environments across development and production
* **Caddy** – Configured as both a static file server for the frontend and reverse proxy for backend services, simplifying the deployment architecture
* **Makefile** – Automated common development tasks and deployment procedures, reducing errors when typing Linux commands

# What I Learned

Building CabinFy was a comprehensive learning experience that challenged me to apply both frontend and backend skills in a real-world, full-stack project. Here are some of the key takeaways:

* End-to-End Development Workflow: I gained hands-on experience managing an entire development lifecycle—from designing UI components to building a custom backend and deploying the full application using Docker and Caddy.
* Custom Backend Architecture: Instead of relying on prebuilt backend services like Supabase, I built a complete backend from scratch using PostgreSQL, Prisma, Express, and Zod, which taught me how to better handle data modeling, validation, and RESTful API design.
* Authentication & Authorization: Implementing secure login logic with Passport and JWT gave me a deeper understanding of access control and session management in web applications.
* Frontend Architecture & Optimization: Using React and React Query helped me understand efficient state management and data fetching strategies in large-scale apps.
* Internationalization: Integrating react-i18next gave me insight into structuring multilingual applications and managing locale-specific content.
* DevOps & Automation: I learned how to automate repetitive tasks using Makefile, and improved deployment workflows by using Docker. I created custom Dockerfiles and a docker-compose setup to containerize the backend and database, streamlining local development and deployment. Additionally, I used Caddy as both a reverse proxy and static file server to simplify routing and secure backend services.
* Responsive Design & UX: Implementing dark mode, responsive layouts, and device-friendly interfaces helped me build a polished and accessible user experience across platforms.

This project was not only a technical exercise but also a chance to think critically about architecture, performance, and maintainability—skills I’ll carry forward into future projects.

# Final Thoughts

CabinFy was more than just a portfolio piece—it was a deep dive into building a full-stack, production-ready web application from the ground up. Through this project, I explored modern development practices, honed my skills across the stack, and gained hands-on experience with everything from multilingual support and authentication to responsive design and containerized deployment.

This project reflects my ability to:

* Take initiative beyond course content
* Architect scalable, maintainable systems
* Embrace modern tooling across frontend, backend, and DevOps
* Prioritize user experience across languages and devices

Most importantly, CabinFy gave me a clearer vision of how real-world web applications are designed, built, and deployed—and the confidence to take on even more complex, user-centric projects in the future.