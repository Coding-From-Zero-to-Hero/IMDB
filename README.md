# 🎬 IMDB-like Backend — Feature Plan & Architecture

**Tech stack:** C#, ASP.NET, PostgreSQL

**Architecture:** Modular Monolith

This document describes the architecture, domain modules, features, and API endpoints of an IMDB-like backend service built using a **Modular Monolith** approach.
Each module encapsulates its own domain logic, infrastructure, and HTTP API.

---

# 🧱 Architecture Overview

The system consists of multiple domain modules with strict internal boundaries:

* Domain entities
* Application services
* Infrastructure layer
* API endpoints

The database is shared, but business logic is isolated per domain.

### **Primary Modules**

* **Titles Module** — movies, series, games, episodes, genres, ratings
* **People Module** — actors, directors, professions, filmography
* **Search Module** — full-text search
* **Auth Module** — registration, login, JWT
* **Users Module** — watchlist, user profile

---

# 📦 Domain Modules & Features

---

# 1. 🎞 Titles Module (Movies / Series / Games)

Handles all content-related data.

### **Entities**

* `titles`
* `types`
* `genres`
* `title_genre`
* `episodes`
* `rating`

### **Features**

* CRUD for titles
* Attach genres to titles
* Fetch rating & vote count
* Episode listing for series
* Filtering, sorting, pagination

### **API Endpoints**

| Method | Endpoint                    | Description                           |
| ------ | --------------------------- | ------------------------------------- |
| GET    | `/api/titles`               | List titles with filters & pagination |
| GET    | `/api/titles/{id}`          | Get title details                     |
| POST   | `/api/titles`               | Create title (admin)                  |
| PUT    | `/api/titles/{id}`          | Update title (admin)                  |
| DELETE | `/api/titles/{id}`          | Delete title (admin)                  |

---

# 2. 👤 People Module (Actors / Directors / Filmography)

Manages individuals participating in titles.

### **Entities**

* `names`
* `professions`
* `name_profession`
* `name_title`

### **Features**

* Fetch person information
* Filmography generation
* Profession priority
* Filters: birth year, profession, active years

### **API Endpoints**

| Method | Endpoint                       | Description              |
| ------ | ------------------------------ | ------------------------ |
| GET    | `/api/people`                  | List people with filters |
| GET    | `/api/people/{id}`             | Person details           |
| GET    | `/api/people/{id}/filmography` | Filmography              |
| POST   | `/api/people`                  | Create person (admin)    |
| PUT    | `/api/people/{id}`             | Update person (admin)    |
| DELETE | `/api/people/{id}`             | Delete person (admin)    |

---

# 3. 🔍 Search Module (Full-text Search)

Provides global search functionality.

### **Features**

* Search across titles
* Search across people
* Match highlighting
* Relevance-based ranking

### **API Endpoints**

| Method | Endpoint             | Description                    |
| ------ | -------------------- | ------------------------------ |
| GET    | `/api/search`        | Combined title + people search |
| GET    | `/api/search/titles` | Title-only search              |
| GET    | `/api/search/people` | People-only search             |

### **Notes**

* Uses PostgreSQL Full-Text Search
* Indexed fields:

  * `primary_title`
  * `original_title`
  * `primary_name`

---

# 4. 🔐 Auth Module (JWT Authentication)

Handles account creation and authentication.

### **Features**

* Registration
* Login
* Logout (invalidate refresh token)
* Access & refresh token pair
* Role-based auth:

  * **User**
  * **Admin**

### **API Endpoints**

| Method | Endpoint             | Description              |
| ------ | -------------------- | ------------------------ |
| POST   | `/api/auth/register` | Register new user        |
| POST   | `/api/auth/login`    | Login                    |
| POST   | `/api/auth/refresh`  | Refresh JWT token        |
| POST   | `/api/auth/logout`   | Invalidate refresh token |

---

# 5. 👥 Users Module (Profile / Watchlist)

Features available to authenticated users.

### **Features**

* User profile
* Watchlist management
* Rating history *(optional)*
* Personal preferences

### **API Endpoints**

| Method | Endpoint                         | Description            |
| ------ | -------------------------------- | ---------------------- |
| GET    | `/api/users/me`                  | Current user profile   |
| PATCH  | `/api/users/me`                  | Update profile         |
| GET    | `/api/users/watchlist`           | Get watchlist          |
| POST   | `/api/users/watchlist/{titleId}` | Add title to watchlist |
| DELETE | `/api/users/watchlist/{titleId}` | Remove from watchlist  |

---

# 🗄 Database Structure Overview

Database schema is centered around **Titles**, **People**, and **Users**, connected through many-to-many relations.

### **Titles Domain**

* `titles`
* `types`
* `genres`
* `title_genre`
* `rating`
* `episodes`

### **People Domain**

* `names`
* `professions`
* `name_profession`
* `name_title`

### **Users Domain**

* `users`
* `user_watchlist`
* `refresh_tokens`

---

# 🚀 Future Improvements

* Redis caching layer
* GraphQL API
* Recommendation engine (collaborative filtering)
* Rate-limiting & throttling
* WebSockets for real-time updates
* Background jobs (ETL imports, recalculations)

---