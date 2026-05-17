<img src="https://capsule-render.vercel.app/api?type=venom&height=300&color=a1b800&text=AutoLane&reversal=false&fontColor=FFFFFF&desc=Second-Hand%20Car%20Sales&descAlign=50&descAlignY=60" width="100%"/>

<div align="center">

<p>
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/MySQL-8.x-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Security-✓-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white"/>
</p>

<p>
  <img src="https://img.shields.io/badge/SE1020-OOP_Project-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Architecture-Layered_REST_API-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Auth-Session_Based-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Status-Active-a1b800?style=flat-square&labelColor=111111"/>
</p>

*A full-stack second-hand car marketplace built to demonstrate core OOP principles —*
*role-based access, image uploads, clean layered architecture, and a decoupled Vanilla JS frontend.*

<br/>

</div>

---

## `>_ ` Table of Contents

- [🏎️ Features](#️-features)
- [🏗️ System Architecture](#️-system-architecture)
- [🧬 OOP Concepts](#-oop-concepts)
- [🗄️ Database Schema](#️-database-schema)
- [🔌 API Reference](#-api-reference)
- [⚙️ Setup & Installation](#️-setup--installation)
- [🖥️ Frontend Pages](#️-frontend-pages)
- [👥 Team](#-team)

---

## 🏎️ Features

<table>
<tr>
<td width="50%">

**🌐 Public — Buyers**
- Browse all listings in a responsive card grid
- Search by **brand** · filter by **district**, **price**, **year**
- View full car detail with uploaded image

</td>
<td width="50%">

**🏪 Sellers** *(authenticated)*
- Personal dashboard with stats
- Create, edit, and delete own listings
- Drag-and-drop image upload per car

</td>
</tr>
<tr>
<td width="50%">

**🔑 Admins** *(privileged)*
- Platform-wide car listing oversight
- Remove any seller account
- Role-protected dashboard

</td>
<td width="50%">

**⚙️ Technical**
- Spring Security session-based auth
- Single-Table Inheritance for users
- Multipart file write → disk (`/uploads`)
- Decoupled REST API + Vanilla JS `fetch()`

</td>
</tr>
</table>

---

## 🏗️ System Architecture

AutoLane follows a strict **layered architecture** — each layer has one responsibility and communicates only with its neighbours.

```
┌─────────────────────────────────────────────────────────────────────┐
│  🌐  BROWSER   │   HTML + Bootstrap 5 + Vanilla JS                  │
│                │   fetch() calls REST endpoints — renders JSON       │
└────────────────────────────────┬────────────────────────────────────┘
                                 │  HTTP
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🔒  SPRING SECURITY                                                 │
│      Validates session · Enforces SELLER / ADMIN role rules         │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🎮  CONTROLLER LAYER   (@RestController)                            │
│      HomeController · AuthController · SellerController             │
│      AdminController · PageController                               │
│      Receives HTTP · delegates to services · returns JSON           │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  ⚙️  SERVICE LAYER   (@Service)                                      │
│      UserService  ·  CarService                                     │
│      Business logic · BCrypt encoding · file I/O                   │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🗃️  REPOSITORY LAYER   (JpaRepository interfaces)                   │
│      UserRepository  ·  CarRepository                               │
│      Spring Data JPA — auto-generates all SQL                       │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🗄️  MySQL DATABASE                                                  │
│      Tables:  users  ·  cars                                        │
└─────────────────────────────────────────────────────────────────────┘
```

### Request Lifecycle
```
① Browser sends  GET /api/cars
② Spring Security checks access rules
③ HomeController receives request
④ Calls CarService.getAllCars()
⑤ CarService calls CarRepository.findAll()
⑥ JPA queries MySQL → returns List<Car>
⑦ Controller serialises → JSON response
⑧ JavaScript renders cards in the browser
```

---

## 🧬 OOP Concepts

> OOP is worth **20 marks** — every concept is explicitly demonstrated below.

### Inheritance · Polymorphism · Abstraction

```
                    ┌───────────────────────────┐
                    │       User.java            │
                    │      〈 abstract 〉         │
                    │                           │
                    │  - id: Long               │
                    │  - name: String           │
                    │  - email: String          │
                    │  - password: String       │
                    │                           │
                    │  + login()                │
                    │  + logout()               │
                    │  + getDashboard() ◄───── abstract method
                    └──────────┬────────────────┘
                               │   extends
               ┌───────────────┴───────────────┐
               │                               │
   ┌───────────▼────────────┐   ┌──────────────▼──────────┐
   │     Seller.java        │   │      Admin.java          │
   │                        │   │                          │
   │  - List<Car> cars      │   │  (no extra fields)       │
   │                        │   │                          │
   │  + getDashboard()      │   │  + getDashboard()        │
   │  → "seller-dashboard"  │   │  → "admin-dashboard"     │
   └────────────────────────┘   └──────────────────────────┘
```

```java
// Polymorphism in action — AuthController.java
User user = userService.findByEmail(email);
String redirect = user.getDashboard(); // Calls Seller or Admin version at runtime
```

### Encapsulation

```java
// Car.java — all fields private, never directly exposed
@Entity
public class Car {
    private String brand;   // ← private
    private double price;   // ← private
    private String imagePath; // ← private
    // Access only via Lombok @Getter / @Setter
}
```

### Information Hiding

| Layer | What it hides |
|-------|--------------|
| `@JsonProperty(WRITE_ONLY)` on `password` | Password hash never appears in any API response |
| Controller → Service boundary | Controller has no idea how file I/O or BCrypt works |
| Service → Repository boundary | Service never writes a single line of SQL |
| Frontend → Backend boundary | JS never touches the database; everything goes through the REST API |

---

## 🗄️ Database Schema

```
┌──────────────────────────────────────┐         ┌──────────────────────────────────┐
│               users                  │         │              cars                 │
├──────────────────────────────────────┤         ├──────────────────────────────────┤
│  id          INT   PK  AUTO_INC      │◄──┐     │  id          INT  PK  AUTO_INC   │
│  name        VARCHAR(100)            │   │     │  brand       VARCHAR(100)         │
│  email       VARCHAR(150)  UNIQUE    │   │     │  model       VARCHAR(100)         │
│  password    VARCHAR(255)  BCRYPT    │   │     │  year        INT                  │
│  role        ENUM(SELLER, ADMIN)     │   │     │  price       DECIMAL(10,2)        │
│              ↑ discriminator column  │   │     │  mileage     INT                  │
└──────────────────────────────────────┘   │     │  location    VARCHAR(150)         │
                                           │     │  image_path  VARCHAR(255)         │
  Single-Table Inheritance:                └─────┤  seller_id   INT  FK              │
  SELLER and ADMIN rows live in                  └──────────────────────────────────┘
  the same table, differentiated
  by the `role` column.                    Relationship: One Seller → Many Cars (1 : N)
```

---

## 🔌 API Reference

### 🌐 Public *(no login required)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cars` | All listings |
| `GET` | `/api/cars/{id}` | Single car by ID |
| `GET` | `/api/cars/search?brand=&location=` | Filter listings |
| `POST` | `/api/auth/register` | Register new seller |
| `POST` | `/api/auth/login` | Login via Spring Security |
| `POST` | `/api/auth/logout` | End session |
| `GET` | `/api/auth/me` | Get current user + role |

### 🏪 Seller *(Role: `SELLER`)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/seller/cars` | Own listings only |
| `GET` | `/api/seller/cars/{id}` | Own car by ID |
| `POST` | `/api/seller/cars` | Add listing `multipart/form-data` |
| `PUT` | `/api/seller/cars/{id}` | Edit own listing |
| `DELETE` | `/api/seller/cars/{id}` | Delete own listing |

### 🔑 Admin *(Role: `ADMIN`)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/admin/cars` | All platform listings |
| `DELETE` | `/api/admin/cars/{id}` | Delete any listing |
| `GET` | `/api/admin/sellers` | All sellers + car counts |
| `DELETE` | `/api/admin/sellers/{id}` | Remove seller account |

### 📤 Image Upload Format
```
POST /api/seller/cars
Content-Type: multipart/form-data

├── car    →  JSON  { "brand", "model", "year", "price", "mileage", "location" }
└── image  →  file  JPG / PNG / WEBP  (max 10 MB)
```

---

## ⚙️ Setup & Installation

### Prerequisites

| Tool | Version |
|------|---------|
| JDK | 17+ |
| MySQL | 8.x |
| Maven | 3.8+ *(bundled with IntelliJ)* |

---

### 1 — Clone

```bash
git clone https://github.com/IT25102440/AutoLane.git
cd AutoLane
```

### 2 — Create the Database

```sql
CREATE DATABASE carplatform;
```
> Tables are auto-created by Hibernate on first run (`ddl-auto=update`).

### 3 — Configure

Edit `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/carplatform
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
```

### 4 — Seed an Admin Account

Admins cannot self-register — insert directly:

```sql
INSERT INTO users (name, email, password, role)
VALUES ('Admin', 'admin@autolane.lk', '$2a$10$YOUR_BCRYPT_HASH', 'ADMIN');
```
> Generate a hash → [bcrypt.online](https://bcrypt.online)

### 5 — Run

```bash
./mvnw spring-boot:run
```
or run `CarPlatformApplication.java` from IntelliJ (`▶`).

### 6 — Open

```
http://localhost:8080
```

---

## 🖥️ Frontend Pages

| Page | Route | Access | Description |
|------|--------|--------|-------------|
| 🏠 Home | `/` | Public | Browse & search all listings |
| 🚗 Car Detail | `/car-detail?id=` | Public | Full listing view with image |
| 📝 Register | `/register` | Public | Create seller account |
| 🔐 Login | `/login` | Public | Login — auto-redirects by role |
| 📊 Seller Dashboard | `/seller-dashboard` | SELLER | Own listings, stats, actions |
| ➕ Add Car | `/add-car` | SELLER | New listing + image upload |
| ✏️ Edit Car | `/edit-car?id=` | SELLER | Pre-filled edit form |
| 🛡️ Admin Dashboard | `/admin-dashboard` | ADMIN | Platform-wide management |

### Shared Frontend Utilities

```
static/
├── css/style.css       ─  shared stylesheet (CSS variables, navbar, cards, modals)
├── js/auth.js          ─  requireRole() auth guard · handleLogout()
├── js/navbar.js        ─  dynamic navbar injection by mode (public/seller/admin)
└── js/utils.js         ─  populateDistricts() — 25 Sri Lankan districts dropdown
```

---

## 📦 CRUD Map

| Operation | Trigger | Full Code Path |
|-----------|---------|----------------|
| **CREATE** car | Seller adds listing | `POST /api/seller/cars` → `SellerController` → `CarService.addCar()` → `CarRepository.save()` |
| **CREATE** seller | Registration form | `POST /api/auth/register` → `AuthController` → `UserService.registerSeller()` → `UserRepository.save()` |
| **READ** all cars | Home page load | `GET /api/cars` → `HomeController` → `CarService.getAllCars()` → `CarRepository.findAll()` |
| **READ** one car | Car detail page | `GET /api/cars/{id}` → `HomeController` → `CarService.getCarById()` |
| **READ** own cars | Seller dashboard | `GET /api/seller/cars` → `SellerController` → `CarService.getCarsBySeller()` |
| **UPDATE** car | Seller edits listing | `PUT /api/seller/cars/{id}` → `SellerController` → `CarService.updateCar()` → `Car.updateDetails()` |
| **DELETE** car | Seller / Admin | `DELETE /api/seller/cars/{id}` or `/api/admin/cars/{id}` → `CarService.deleteCar()` |
| **DELETE** seller | Admin removes user | `DELETE /api/admin/sellers/{id}` → `AdminController` → `UserService.deleteSeller()` |

---

## 👥 Team

| # | Name | Role | Files | Branch |
|---|------|------|-------|--------|
| 1 | **Nethwindu** *(Lead)* | Models · Security · Home | `User.java` `Seller.java` `Admin.java` `Car.java` `SecurityConfig.java` `WebConfig.java` `index.html` | `main` |
| 2 | **Chanuki** | Car layer | `CarRepository.java` `CarService.java` `CarDTO.java` `car-detail/index.html` | `feature/car-model-and-service` |
| 3 | **Resandu** | User layer | `UserRepository.java` `UserService.java` `UserDTO.java` `register/index.html` | `feature/user-repository-and-service` |
| 4 | **Nilanga** | Controllers · Login | `AuthController.java` `HomeController.java` `PageController.java` `login/index.html` | `feature/auth-and-home-controller` |
| 5 | **Chalinda** | Seller flow | `SellerController.java` `seller-dashboard/index.html` `add-car/index.html` `edit-car/index.html` | `feature/seller-controller` |
| 6 | **Nadee** | Admin flow | `AdminController.java` `admin-dashboard/index.html` | `feature/admin-controller` |

---

<div align="center">

<img src="https://img.shields.io/badge/SE1020-Object_Oriented_Programming-a1b800?style=for-the-badge&labelColor=111111"/>

<br/><br/>

<sub>Built with ☕ Java · 🌱 Spring Boot · 🗄️ MySQL &nbsp;|&nbsp; Group 6 &nbsp;|&nbsp; Deadline: May 22, 2025</sub>

</div>
