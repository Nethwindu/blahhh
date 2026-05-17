<div align="center">

<img src="https://capsule-render.vercel.app/api?type=blur&height=300&color=&color=a1b800&text=AutoLane&reversal=false&fontColor=FFFFFF&descAlign=50&descAlignY=100" width="100%"/>


<p>
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/MySQL-8.x-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Security-Auth-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white"/>
</p>

<p>
  <img src="https://img.shields.io/badge/Module-SE1020_OOP-blueviolet?style=flat-square"/>
  <img src="https://img.shields.io/badge/Stack-Java_%2B_Spring_Boot_%2B_MySQL-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/Frontend-Vanilla_JS_%2B_Bootstrap_5-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square"/>
</p>

> A full-stack university OOP project — a role-based car marketplace with REST API, Spring Security, image uploads, and a decoupled vanilla JS frontend.

</div>

---

## 🏗️ Architecture

```
Browser (HTML + Vanilla JS)
        │  fetch() REST calls
        ▼
Spring Security  ──  checks role & session
        │
        ▼
Controller Layer  ─  HomeController · AuthController · SellerController · AdminController
        │
        ▼
Service Layer     ─  UserService · CarService  (business logic + file I/O)
        │
        ▼
Repository Layer  ─  UserRepository · CarRepository  (Spring Data JPA)
        │
        ▼
MySQL Database    ─  tables: users · cars
```

---

## 🧬 OOP Concepts

| Concept | Where |
|---|---|
| **Encapsulation** | All entity fields are `private`, accessed only via Lombok getters/setters |
| **Inheritance** | `Seller` and `Admin` both extend the abstract `User` class |
| **Polymorphism** | `getDashboard()` is abstract in `User`, overridden in each subclass |
| **Abstraction** | Abstract `User` class · Repository interfaces · Service layer hides business logic from controllers |
| **Information Hiding** | Passwords never returned in API responses · each layer isolated from others |

```
         User (abstract)
        /               \
   Seller              Admin
getDashboard()      getDashboard()
→ "seller-dashboard" → "admin-dashboard"
```

---

## 🔌 API Reference

**Public**

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cars` | All listings |
| `GET` | `/api/cars/{id}` | Single car |
| `GET` | `/api/cars/search?brand=&location=` | Search |
| `POST` | `/api/auth/register` | Register seller |
| `GET` | `/api/auth/me` | Current user info |

**Seller** *(requires role: SELLER)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/seller/cars` | Own listings |
| `POST` | `/api/seller/cars` | Add listing (multipart) |
| `PUT` | `/api/seller/cars/{id}` | Edit listing |
| `DELETE` | `/api/seller/cars/{id}` | Delete listing |

**Admin** *(requires role: ADMIN)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET/DELETE` | `/api/admin/cars/{id}` | Manage any listing |
| `GET/DELETE` | `/api/admin/sellers/{id}` | Manage any seller |

---

## ⚙️ Setup

**1. Prerequisites:** JDK 17+, MySQL 8.x, Maven

**2. Create the database**
```sql
CREATE DATABASE carplatform;
```

**3. Configure** `src/main/resources/application.properties`
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/carplatform
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
```

**4. Seed an admin account** *(admins can't self-register)*
```sql
INSERT INTO users (name, email, password, role)
VALUES ('Admin', 'admin@autolane.lk', '$2a$10$YOUR_BCRYPT_HASH', 'ADMIN');
```
> Generate a BCrypt hash at [bcrypt.online](https://bcrypt.online)

**5. Run**
```bash
./mvnw spring-boot:run
# or run CarPlatformApplication.java from IntelliJ
```

**6. Open** → `http://localhost:8080`

---

## 🖥️ Pages

| Page | Route | Access |
|------|--------|--------|
| Home | `/` | Public |
| Car Detail | `/car-detail?id=` | Public |
| Register | `/register` | Public |
| Login | `/login` | Public |
| Seller Dashboard | `/seller-dashboard` | SELLER |
| Add / Edit Car | `/add-car` · `/edit-car?id=` | SELLER |
| Admin Dashboard | `/admin-dashboard` | ADMIN |

---

## 👥 Team

| # | Name | Responsibility | Branch |
|---|------|---------------|--------|
| 1 | **Nethwindu** *(Lead)* | Models, Security, Home page | `main` |
| 2 | **Chanuki** | CarRepository, CarService, Car Detail | `feature/car-model-and-service` |
| 3 | **Resandu** | UserRepository, UserService, Register | `feature/user-repository-and-service` |
| 4 | **Nilanga** | Auth/Home/Page Controllers, Login | `feature/auth-and-home-controller` |
| 5 | **Chalinda** | SellerController, Seller/Add/Edit pages | `feature/seller-controller` |
| 6 | **Nadee** | AdminController, Admin Dashboard | `feature/admin-controller` |

---

<div align="center">
<sub>SE1020 – Object Oriented Programming · Group 6</sub>
</div>
