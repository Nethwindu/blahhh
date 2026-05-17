<div align="center">

<!-- Animated Banner -->
<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0f0f0f,50:1a1a2e,100:16213e&height=200&section=header&text=AutoLane&fontSize=80&fontAlignY=38&animation=fadeIn&fontColor=ffffff&desc=Second-Hand%20Car%20Sales%20%26%20Purchase%20Platform&descAlignY=60&descSize=20&descColor=a0a0b0"/>

<!-- Badges Row 1 -->
<p>
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/MySQL-8.x-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white"/>
</p>

<!-- Badges Row 2 -->
<p>
  <img src="https://img.shields.io/badge/Maven-Build-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Security-Auth-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white"/>
  <img src="https://img.shields.io/badge/Lombok-Boilerplate_Free-E91E63?style=for-the-badge&logo=java&logoColor=white"/>
  <img src="https://img.shields.io/badge/OOP-Concepts-0088CC?style=for-the-badge&logo=java&logoColor=white"/>
</p>

<!-- Status Badges -->
<p>
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square"/>
  <img src="https://img.shields.io/badge/Module-SE1020-blueviolet?style=flat-square"/>
  <img src="https://img.shields.io/badge/University_Project-Group_6-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-Academic-lightgrey?style=flat-square"/>
</p>

<br/>

> **A full-stack web application built for SE1020 – Object Oriented Programming.**  
> Demonstrates core OOP principles through a real-world car marketplace — featuring role-based authentication, image uploads, and a clean decoupled REST API architecture.

<br/>

</div>

---

## 📋 Table of Contents

- [✨ Features](#-features)
- [🏗️ Architecture Overview](#️-architecture-overview)
- [🧬 OOP Concepts Demonstrated](#-oop-concepts-demonstrated)
- [🗂️ Project Structure](#️-project-structure)
- [🗄️ Database Schema](#️-database-schema)
- [🔌 API Reference](#-api-reference)
- [⚙️ Setup & Installation](#️-setup--installation)
- [🖥️ Pages & Frontend](#️-pages--frontend)
- [👥 Team](#-team)
- [📄 License](#-license)

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🛒 For Buyers (Public)
- Browse all car listings on the home page
- Search by **brand** and filter by **district**
- Filter by **price range** and **year**
- View full car details with images

</td>
<td width="50%">

### 🏪 For Sellers (Authenticated)
- Secure registration and login
- Personal seller dashboard with stats
- Add, edit, and delete own car listings
- Upload car images with drag-and-drop

</td>
</tr>
<tr>
<td width="50%">

### 🔑 For Admins (Privileged)
- Platform-wide car listing management
- View and remove seller accounts
- Real-time stats across all listings
- Role-protected admin dashboard

</td>
<td width="50%">

### ⚙️ Technical Highlights
- Spring Security session-based auth
- Single-Table Inheritance for User hierarchy
- Multipart file upload to disk
- Decoupled REST API + Vanilla JS frontend

</td>
</tr>
</table>

---

## 🏗️ Architecture Overview

AutoLane follows a strict **layered architecture** — a core principle of OOP that enforces separation of concerns. Each layer has a single responsibility and communicates only with its adjacent layers.

```
┌─────────────────────────────────────────────────────────────────┐
│                    BROWSER  (HTML + JS)                         │
│          fetch() calls REST endpoints, renders JSON             │
└───────────────────────────┬─────────────────────────────────────┘
                            │  HTTP Request
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│              SPRING SECURITY  (SecurityConfig)                  │
│        Checks authentication and role before forwarding         │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                 CONTROLLER LAYER  (@RestController)             │
│   HomeController · AuthController · SellerController            │
│                  · AdminController · PageController             │
│       Receives HTTP, delegates to services, returns JSON        │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                  SERVICE LAYER  (@Service)                      │
│              UserService  ·  CarService                         │
│    Business logic, validation, file I/O, password encoding      │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│               REPOSITORY LAYER  (JpaRepository)                 │
│            UserRepository  ·  CarRepository                     │
│         Spring Data JPA interfaces — auto-generates SQL         │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                   MySQL Database  (carplatform)                 │
│                  Tables: users  ·  cars                         │
└─────────────────────────────────────────────────────────────────┘
```

### Request Flow

```
1. Browser  →  HTTP request (e.g. GET /api/cars)
2. Spring Security checks access rules
3. Controller receives request, extracts parameters
4. Controller calls Service method
5. Service applies business logic, calls Repository
6. Repository queries MySQL, returns entity
7. Service returns result to Controller
8. Controller returns JSON → Browser renders it
```

---

## 🧬 OOP Concepts Demonstrated

> These are the core evaluation criteria for SE1020 (20 marks).

### 1. 🔒 Encapsulation

All entity fields are `private`. Access is only through public getters and setters — provided by Lombok `@Getter` / `@Setter`.

```java
// Car.java — fields are private, never directly exposed
@Entity
public class Car {
    private String brand;  // ← private
    private double price;  // ← private
    // Accessed via getCar(), setBrand() etc.
}
```

The frontend never touches the database directly — it talks to the REST API, which controls exactly what data is exposed.

---

### 2. 🧬 Inheritance

`User` is an **abstract class**. `Seller` and `Admin` both **extend** `User`, inheriting shared fields (`id`, `name`, `email`, `password`) without duplicating code.

```
                    ┌──────────────┐
                    │  User.java   │  ← abstract class
                    │  (abstract)  │
                    │  id, name,   │
                    │  email, pass │
                    │  login()     │
                    │  logout()    │
                    │  getDashboard() ← abstract method
                    └──────┬───────┘
                           │
              ┌────────────┴────────────┐
              │                         │
     ┌────────▼────────┐     ┌──────────▼───────┐
     │  Seller.java    │     │   Admin.java      │
     │  extends User   │     │   extends User    │
     │  List<Car> cars │     │                   │
     │  getDashboard() │     │  getDashboard()   │
     │  → "seller-     │     │  → "admin-        │
     │    dashboard"   │     │    dashboard"     │
     └─────────────────┘     └───────────────────┘
```

---

### 3. 🔄 Polymorphism

`getDashboard()` is declared as `abstract` in `User` and **overridden** in both `Seller` and `Admin`. The correct version is called automatically at runtime depending on the actual object type.

```java
// User.java
public abstract String getDashboard();  // contract only

// Seller.java
@Override
public String getDashboard() { return "seller-dashboard"; }

// Admin.java
@Override
public String getDashboard() { return "admin-dashboard"; }

// AuthController.java — polymorphism in action:
User user = userService.findByEmail(email);
String dashboard = user.getDashboard();  // calls Seller or Admin version automatically
```

---

### 4. 🎭 Abstraction

Three levels of abstraction exist throughout the system:

| Level | What it hides |
|---|---|
| `abstract User` | Defines what a user IS, not how each type behaves |
| Repository interfaces | The developer calls `.save()` without writing any SQL |
| Service layer | The controller calls `carService.addCar()` without knowing how file I/O works |

---

### 5. 🛡️ Information Hiding

Each layer is isolated from the others:

- HTML/JS frontend never touches the database  
- Controllers never write SQL  
- Repositories contain zero business rules  
- Passwords are `@JsonProperty(access = WRITE_ONLY)` — never returned in API responses

---

## 🗂️ Project Structure

```
car-platform/
│
├── 📄 pom.xml                          # Maven dependencies
├── 📄 README.md                        # This file
│
├── 📁 uploads/                         # Car images saved here at runtime
│
└── 📁 src/
    ├── 📁 main/
    │   ├── 📁 java/com/carplatform/car_platform/
    │   │   │
    │   │   ├── 🚀 CarPlatformApplication.java     # Entry point
    │   │   │
    │   │   ├── 📁 config/
    │   │   │   ├── SecurityConfig.java             # Auth rules, roles, login/logout
    │   │   │   └── WebConfig.java                  # Static resource & path config
    │   │   │
    │   │   ├── 📁 model/                           # ← OOP lives here
    │   │   │   ├── User.java                       # Abstract base class
    │   │   │   ├── Seller.java                     # Extends User
    │   │   │   ├── Admin.java                      # Extends User
    │   │   │   └── Car.java                        # Car entity
    │   │   │
    │   │   ├── 📁 repository/
    │   │   │   ├── UserRepository.java             # JPA interface for users
    │   │   │   └── CarRepository.java              # JPA interface for cars
    │   │   │
    │   │   ├── 📁 service/
    │   │   │   ├── UserService.java                # Registration, lookup, deletion
    │   │   │   └── CarService.java                 # CRUD + file upload logic
    │   │   │
    │   │   ├── 📁 controller/
    │   │   │   ├── HomeController.java             # Public car endpoints
    │   │   │   ├── AuthController.java             # Register, login, /me
    │   │   │   ├── SellerController.java           # Seller-protected endpoints
    │   │   │   ├── AdminController.java            # Admin-protected endpoints
    │   │   │   └── PageController.java             # Page routing (SPA-style)
    │   │   │
    │   │   └── 📁 dto/
    │   │       ├── CarDTO.java                     # Car input transfer object
    │   │       └── UserDTO.java                    # User input transfer object
    │   │
    │   └── 📁 resources/
    │       ├── application.properties              # DB config, file upload settings
    │       │
    │       └── 📁 static/                          # Frontend (served by Spring)
    │           ├── index.html                      # Home page
    │           ├── 📁 css/style.css                # Shared stylesheet
    │           ├── 📁 js/
    │           │   ├── auth.js                     # requireRole(), handleLogout()
    │           │   ├── navbar.js                   # Dynamic navbar injection
    │           │   └── utils.js                    # populateDistricts()
    │           ├── 📁 login/
    │           ├── 📁 register/
    │           ├── 📁 car-detail/
    │           ├── 📁 seller-dashboard/
    │           ├── 📁 add-car/
    │           ├── 📁 edit-car/
    │           └── 📁 admin-dashboard/
    │
    └── 📁 test/
        └── CarPlatformApplicationTests.java
```

---

## 🗄️ Database Schema

### Entity Relationship

```
┌──────────────────────────────────┐          ┌──────────────────────────────────┐
│             users                │          │              cars                 │
├──────────────────────────────────┤          ├──────────────────────────────────┤
│ id          INT  PK  AUTO_INC    │◄────1──┐ │ id          INT  PK  AUTO_INC    │
│ name        VARCHAR(100)         │        │ │ brand       VARCHAR(100)          │
│ email       VARCHAR(150) UNIQUE  │        │ │ model       VARCHAR(100)          │
│ password    VARCHAR(255) BCRYPT  │        │ │ year        INT                   │
│ role        ENUM(SELLER, ADMIN)  │        │ │ price       DECIMAL(10,2)         │
└──────────────────────────────────┘        │ │ mileage     INT                   │
                                            │ │ location    VARCHAR(150)          │
                                            │ │ image_path  VARCHAR(255)          │
                                            └─┤ seller_id   INT  FK → users.id   │
                                              └──────────────────────────────────┘

Relationship: One Seller → Many Cars  (1:N)
Inheritance:  Single Table — users table stores both SELLER and ADMIN rows
              differentiated by the `role` discriminator column
```

---

## 🔌 API Reference

### 🌐 Public Endpoints (No Auth)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cars` | Get all car listings |
| `GET` | `/api/cars/{id}` | Get single car by ID |
| `GET` | `/api/cars/search?brand=&location=` | Search/filter cars |
| `POST` | `/api/auth/register` | Register new seller account |
| `POST` | `/api/auth/login` | Login (Spring Security) |
| `POST` | `/api/auth/logout` | Logout current session |
| `GET` | `/api/auth/me` | Get current logged-in user info |

---

### 🏪 Seller Endpoints (Role: SELLER required)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/seller/cars` | Get logged-in seller's listings |
| `GET` | `/api/seller/cars/{id}` | Get one of seller's cars |
| `POST` | `/api/seller/cars` | Add new car listing (multipart) |
| `PUT` | `/api/seller/cars/{id}` | Edit own car listing |
| `DELETE` | `/api/seller/cars/{id}` | Delete own car listing |

---

### 🔑 Admin Endpoints (Role: ADMIN required)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/admin/cars` | View all listings platform-wide |
| `DELETE` | `/api/admin/cars/{id}` | Delete any car listing |
| `GET` | `/api/admin/sellers` | View all registered sellers |
| `DELETE` | `/api/admin/sellers/{id}` | Remove a seller account |

---

### 📤 File Upload Format

Car listings with images use `multipart/form-data` with two parts:

```
POST /api/seller/cars
Content-Type: multipart/form-data

Part 1: car   → JSON blob { brand, model, year, price, mileage, location }
Part 2: image → binary file (JPG / PNG / WEBP, max 10MB)
```

---

## ⚙️ Setup & Installation

### Prerequisites

Make sure the following are installed before you begin:

| Tool | Version | Download |
|------|---------|----------|
| JDK | 17+ | [adoptium.net](https://adoptium.net) |
| Maven | 3.8+ | Bundled with IntelliJ IDEA |
| MySQL | 8.x | [mysql.com](https://dev.mysql.com/downloads/) |
| IntelliJ IDEA | Any | [jetbrains.com](https://www.jetbrains.com/idea/) |

---

### Step 1 — Clone the Repository

```bash
git clone https://github.com/YOUR_ORG/car-platform.git
cd car-platform
```

---

### Step 2 — Create the Database

Open **MySQL Workbench** (or any MySQL client) and run:

```sql
CREATE DATABASE carplatform;
```

> The tables (`users`, `cars`) are created **automatically** by Hibernate on first run.  
> `spring.jpa.hibernate.ddl-auto=update` handles this.

---

### Step 3 — Configure Application Properties

Open `src/main/resources/application.properties` and update your credentials:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/carplatform
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
```

---

### Step 4 — Seed an Admin Account

Admin accounts cannot be created through the UI — they must be inserted directly into the database:

```sql
INSERT INTO users (name, email, password, role)
VALUES (
  'Admin User',
  'admin@autolane.lk',
  '$2a$10$REPLACE_WITH_BCRYPT_HASH',
  'ADMIN'
);
```

> Generate a BCrypt hash at [bcrypt.online](https://bcrypt.online) or via Spring's `BCryptPasswordEncoder`.

---

### Step 5 — Run the Application

**In IntelliJ IDEA:**

1. Open the project
2. Wait for Maven to download dependencies
3. Run `CarPlatformApplication.java` (the green ▶ button)
4. Wait for `Started CarPlatformApplication` in the console

**Or via terminal:**

```bash
./mvnw spring-boot:run
```

---

### Step 6 — Open in Browser

```
http://localhost:8080
```

| Route | Page |
|-------|------|
| `/` | Home — browse all listings |
| `/register` | Create a seller account |
| `/login` | Login as seller or admin |
| `/seller-dashboard` | Seller private area |
| `/admin-dashboard` | Admin private area |

---

## 🖥️ Pages & Frontend

All pages are plain HTML with Bootstrap 5. JavaScript uses `fetch()` to call the REST API — no frameworks, no Thymeleaf.

| Page | Route | Access | Description |
|------|--------|--------|-------------|
| 🏠 Home | `/` | Public | Browse all listings, search by brand/district |
| 🚗 Car Detail | `/car-detail?id=` | Public | Full listing view with image |
| 📝 Register | `/register` | Public | Create a seller account |
| 🔐 Login | `/login` | Public | Login — redirects by role |
| 📊 Seller Dashboard | `/seller-dashboard` | SELLER | Own listings, stats, edit/delete |
| ➕ Add Car | `/add-car` | SELLER | New listing form with image upload |
| ✏️ Edit Car | `/edit-car?id=` | SELLER | Pre-filled edit form |
| 🛡️ Admin Dashboard | `/admin-dashboard` | ADMIN | Platform-wide management |

### Shared Frontend Utilities

| File | What it does |
|------|-------------|
| `css/style.css` | Shared stylesheet used by every page |
| `js/auth.js` | `requireRole()` guard + `handleLogout()` |
| `js/navbar.js` | Dynamic navbar injection by mode (public / seller / admin) |
| `js/utils.js` | `populateDistricts()` — 25 Sri Lankan districts dropdown |

---

## 📦 CRUD Operations Map

| Operation | Trigger | Code Path |
|-----------|---------|-----------|
| **CREATE** Car | Seller adds listing | `POST /api/seller/cars` → `SellerController` → `CarService.addCar()` → `CarRepository.save()` |
| **CREATE** Seller | Registration form | `POST /api/auth/register` → `AuthController` → `UserService.registerSeller()` → `UserRepository.save()` |
| **READ** All Cars | Home page load | `GET /api/cars` → `HomeController` → `CarService.getAllCars()` → `CarRepository.findAll()` |
| **READ** One Car | Car detail page | `GET /api/cars/{id}` → `HomeController` → `CarService.getCarById()` |
| **READ** Own Cars | Seller dashboard | `GET /api/seller/cars` → `SellerController` → `CarService.getCarsBySeller()` |
| **UPDATE** Car | Seller edits listing | `PUT /api/seller/cars/{id}` → `SellerController` → `CarService.updateCar()` → `Car.updateDetails()` |
| **DELETE** Car | Seller/Admin deletes | `DELETE /api/seller/cars/{id}` or `/api/admin/cars/{id}` → `CarService.deleteCar()` |
| **DELETE** Seller | Admin removes user | `DELETE /api/admin/sellers/{id}` → `AdminController` → `UserService.deleteSeller()` |

---

## 👥 Team

| # | Name | Role | Assigned Files |
|---|------|------|----------------|
| 1 | **Nethwindu** *(Lead)* | Models, Security, Home page | `User.java`, `Seller.java`, `Admin.java`, `Car.java`, `SecurityConfig.java`, `WebConfig.java`, `index.html` |
| 2 | **Chanuki** | Car Repository, Car Service, Car Detail page | `CarRepository.java`, `CarService.java`, `CarDTO.java`, `car-detail/index.html` |
| 3 | **Resandu** | User Repository, User Service, Register page | `UserRepository.java`, `UserService.java`, `UserDTO.java`, `register/index.html` |
| 4 | **Nilanga** | Auth, Home & Page Controllers, Login page | `AuthController.java`, `HomeController.java`, `PageController.java`, `login/index.html` |
| 5 | **Chalinda** | Seller Controller, Seller Dashboard, Add/Edit Car | `SellerController.java`, `seller-dashboard/index.html`, `add-car/index.html`, `edit-car/index.html` |
| 6 | **Nadee** | Admin Controller, Admin Dashboard | `AdminController.java`, `admin-dashboard/index.html` |

> **Branch convention:** `main` (Nethwindu, lead) · `feature/car-model-and-service` (Chanuki) · `feature/user-repository-and-service` (Resandu) · `feature/auth-and-home-controller` (Nilanga) · `feature/seller-controller` (Chalinda) · `feature/admin-controller` (Nadee)

---

## 📄 License

This project was developed as a university assignment for **SE1020 – Object Oriented Programming** at [Your University Name]. It is intended for academic evaluation purposes only.

---

<div align="center">

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:16213e,50:1a1a2e,100:0f0f0f&height=120&section=footer"/>

<p>
  <img src="https://img.shields.io/badge/Built_with-Java_17-ED8B00?style=flat-square&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Powered_by-Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/SE1020-OOP_Project-blueviolet?style=flat-square"/>
</p>

**AutoLane** — *Built for SE1020 · Group 6*

</div>
