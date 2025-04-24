# Short Explanation

This project was developed as part of my learning journey through [*The Ultimate React Course*](https://www.udemy.com/course/the-ultimate-react-course/) on Udemy, which explores modern technologies such as React, Next.js, and Redux. The primary goal was to deepen my full-stack development skills by implementing best practices in component-based architecture, state management, routing, and responsive design.

Beyond the course material, I extended the project significantly by introducing several custom implementations and enhancements:

* **Custom Backend:** Instead of using [Supabase](https://supabase.com/) as provided in the course, I rebuilt the entire backend from scratch using [PostgreSQL](https://www.postgresql.org/) for the database, [Prisma](https://www.prisma.io/) as the ORM, [Zod](https://zod.dev/) for schema validation, and a custom API built with [Node.js](https://nodejs.org/) and [Express](https://expressjs.com/).
* **UI Overhaul:** I also redesigned the frontend styling by replacing [styled-components](https://styled-components.com/) with a utility-first approach using [Tailwind CSS](https://tailwindcss.com/) and component abstractions from [shadcn/ui](https://ui.shadcn.com/).

# Features

**CabinFy** is a full-stack web application inspired by [Airbnb](https://www.airbnb.com), designed for booking and managing cabin rentals. It delivers a seamless experience for both guests and administrators, with a strong focus on performance, usability, and modern development practices. Key features include:

- **Multilingual Support:** Available in both **English** and **Chinese**, making the platform accessible to a broader audience.
- **Responsive Design & Accessibility:** The UI is fully optimized for desktop, tablet, and mobile devices, with built-in dark mode support and accessibility features ensuring a comfortable experience for all users regardless of device or viewing preferences.
- **Guest Booking Interface:** Users can browse available cabins, view detailed listings, and make bookings through an intuitive and modern interface.
- **Admin Dashboard:** Admins have access to a secure dashboard where they can:

  * View, manage, and process bookings

  * Submit new cabin listings or edit existing ones

  * Monitor booking activity and cabin availability
- **Advanced Data Management:** Supports **filtering**, **sorting**, and **pagination** for easy browsing and efficient data interaction
- **Authentication & Authorization:** Secure user authentication with access control to protect admin functionalities and private data.

# Tech Stack

**CabinFy** is built using a modern and scalable full-stack architecture, combining powerful libraries and tools on both the frontend and backend to ensure performance, maintainability, and a smooth developer experience.

#### Frontend

* [**React**](https://react.dev/) – Core library for building dynamic user interfaces
* [**React Router**](https://reactrouter.com/) – Handles client-side routing and navigation between pages
* [**Tailwind CSS**](https://tailwindcss.com/) – Utility-first CSS framework for building fast and responsive UIs
* [**shadcn/ui**](https://ui.shadcn.com/)– Component library for clean, accessible UI elements
* [**React Query**](https://tanstack.com/query/latest) – Powerful data-fetching and state management solution
* [**react-i18next**](https://react.i18next.com/) – Internationalization support for English and Chinese
* [**Axios**](https://axios-http.com/) – Promise-based HTTP client for API communication
* [**Recharts**](https://recharts.org/) – Elegant charting library used for visualizing sales and analytics
* **Makefile** – Automates the build process and deployment of frontend assets
* [**Caddy**](https://caddyserver.com/) – Lightweight static file server used to serve optimized frontend builds

#### Backend

* [**Node.js**](https://nodejs.org/) – JavaScript runtime used for the backend server
* [**Express**](https://expressjs.com/) – Lightweight web framework for handling routing and server logic
* [**Passport**](https://www.passportjs.org/) – Extremely flexible and modular authentication middleware for Node.js
* [**PostgreSQL**](https://www.postgresql.org/) – Relational database system for structured data storage
* [**Prisma**](https://www.prisma.io/) – Type-safe ORM for database management and queries
* [**Zod**](https://zod.dev/) – Runtime schema validation for data safety
* [**Docker**](https://www.docker.com/) – Containerization tool for consistent development and deployment environments
* **Makefile** – Streamlines repetitive tasks like database migration and seeding
* [**Caddy**](https://caddyserver.com/) – Reverse proxy server to avoid exposing backend servers

# What I Learned

Building **CabinFy** was a comprehensive learning experience that challenged me to apply both frontend and backend skills in a real-world, full-stack project. Here are some of the key takeaways:

* **End-to-End Development Workflow:** I gained hands-on experience managing an entire development lifecycle—from designing UI components to building a custom backend and deploying the full application using Docker and Caddy.
* **Custom Backend Architecture:** Instead of relying on prebuilt backend services like Supabase, I built a complete backend from scratch using **PostgreSQL**, **Prisma**, **Express**, and **Zod**, which taught me how to better handle data modeling, validation, and RESTful API design.
* **Authentication & Authorization:** Implementing secure login logic with **Passport** and **JWT** gave me a deeper understanding of access control and session management in web applications.
* **Frontend Architecture & Optimization:** Using **React** and **React Query** helped me understand efficient state management and data fetching strategies in large-scale apps.
* **Internationalization:** Integrating **react-i18next** gave me insight into structuring multilingual applications and managing locale-specific content.
* **DevOps & Automation:** I learned how to automate repetitive tasks using **Makefile**, and improved deployment workflows by using **Docker**. I created custom **Dockerfiles** and a **docker-compose** setup to containerize the backend and database, streamlining local development and deployment. Additionally, I used **Caddy** as both a reverse proxy and static file server to simplify routing and secure backend services.
* **Responsive Design & UX:** Implementing dark mode, responsive layouts, and device-friendly interfaces helped me build a polished and accessible user experience across platforms.

This project was not only a technical exercise but also a chance to think critically about architecture, performance, and maintainability—skills I’ll carry forward into future projects.

# Final Thoughts

**CabinFy** was more than just a portfolio piece—it was a deep dive into building a full-stack, production-ready web application from the ground up. Through this project, I explored modern development practices, honed my skills across the stack, and gained hands-on experience with everything from multilingual support and authentication to responsive design and containerized deployment.

This project reflects my ability to:

* Take initiative beyond course content
* Architect scalable, maintainable systems
* Embrace modern tooling across frontend, backend, and DevOps
* Prioritize user experience across languages and devices

Most importantly, **CabinFy** gave me a clearer vision of how real-world web applications are designed, built, and deployed—and the confidence to take on even more complex, user-centric projects in the future.