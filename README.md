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

</div>

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│  🌐  BROWSER   │   HTML + Bootstrap 5 + Vanilla JS                  │
│                │   fetch() calls REST endpoints — renders JSON       │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🔒  SPRING SECURITY  —  validates session · enforces roles         │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🎮  CONTROLLER LAYER   (@RestController)                            │
│      HomeController · AuthController · SellerController             │
│      AdminController · PageController                               │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  ⚙️  SERVICE LAYER   —  UserService · CarService                     │
│      Business logic · BCrypt encoding · file I/O                   │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🗃️  REPOSITORY LAYER   —  UserRepository · CarRepository           │
│      Spring Data JPA — auto-generates all SQL                       │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🗄️  MySQL  —  tables: users · cars                                  │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🧬 OOP Concepts

```
                    ┌───────────────────────────┐
                    │       User.java            │
                    │      〈 abstract 〉         │
                    │  id · name · email · pass  │
                    │  login() · logout()        │
                    │  getDashboard()  ◄── abstract
                    └──────────┬────────────────┘
                               │  extends
               ┌───────────────┴───────────────┐
               │                               │
   ┌───────────▼────────────┐   ┌──────────────▼──────────┐
   │     Seller.java        │   │      Admin.java          │
   │  List<Car> cars        │   │                          │
   │  getDashboard()        │   │  getDashboard()          │
   │  → "seller-dashboard"  │   │  → "admin-dashboard"     │
   └────────────────────────┘   └──────────────────────────┘
```

| Concept | Implementation |
|---|---|
| **Encapsulation** | All entity fields `private` — access via Lombok getters/setters only |
| **Inheritance** | `Seller` and `Admin` extend abstract `User`, inheriting shared fields |
| **Polymorphism** | `getDashboard()` overridden per subclass — correct version called at runtime |
| **Abstraction** | Abstract `User` · JPA repository interfaces · Service hides logic from controllers |
| **Information Hiding** | Passwords `@JsonProperty(WRITE_ONLY)` · each layer isolated from others |

---

## 🗄️ Database Schema

```
┌──────────────────────────────────┐       ┌──────────────────────────────────┐
│             users                │       │              cars                 │
├──────────────────────────────────┤       ├──────────────────────────────────┤
│  id        INT   PK  AUTO_INC    │◄──┐   │  id          INT  PK  AUTO_INC   │
│  name      VARCHAR(100)          │   │   │  brand       VARCHAR(100)         │
│  email     VARCHAR(150)  UNIQUE  │   │   │  model       VARCHAR(100)         │
│  password  VARCHAR(255)  BCRYPT  │   │   │  year        INT                  │
│  role      ENUM(SELLER, ADMIN)   │   │   │  price       DECIMAL(10,2)        │
│            ↑ discriminator col   │   │   │  mileage     INT                  │
└──────────────────────────────────┘   │   │  location    VARCHAR(150)         │
                                       │   │  image_path  VARCHAR(255)         │
  Single-Table Inheritance:            └───┤  seller_id   INT  FK              │
  SELLER + ADMIN share one table          └──────────────────────────────────┘
  differentiated by the role column
                                          Relationship:  One Seller → Many Cars
```

---

## 🔌 API Reference

**🌐 Public**

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cars` | All listings |
| `GET` | `/api/cars/{id}` | Single car |
| `GET` | `/api/cars/search?brand=&location=` | Search & filter |
| `POST` | `/api/auth/register` | Register seller |
| `POST` | `/api/auth/login` | Login |
| `GET` | `/api/auth/me` | Current user + role |

**🏪 Seller** *(role: `SELLER`)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/seller/cars` | Own listings |
| `POST` | `/api/seller/cars` | Add listing *(multipart)* |
| `PUT` | `/api/seller/cars/{id}` | Edit listing |
| `DELETE` | `/api/seller/cars/{id}` | Delete listing |

**🔑 Admin** *(role: `ADMIN`)*

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET / DELETE` | `/api/admin/cars/{id}` | Manage any listing |
| `GET / DELETE` | `/api/admin/sellers/{id}` | Manage any seller |

---

## ⚙️ Setup

**1 — Clone**
```bash
git clone https://github.com/IT25102440/AutoLane.git && cd AutoLane
```

**2 — Create the database**
```sql
CREATE DATABASE carplatform;
```

**3 — Configure** `src/main/resources/application.properties`
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/carplatform
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
```

**4 — Seed an admin** *(admins cannot self-register)*
```sql
INSERT INTO users (name, email, password, role)
VALUES ('Admin', 'admin@autolane.lk', '$2a$10$YOUR_BCRYPT_HASH', 'ADMIN');
```
> Generate a BCrypt hash → [bcrypt.online](https://bcrypt.online)

**5 — Run**
```bash
./mvnw spring-boot:run
```
Then open → **`http://localhost:8080`**

---

## 🖥️ Pages

| Page | Route | Access |
|------|--------|--------|
| 🏠 Home | `/` | Public |
| 🚗 Car Detail | `/car-detail?id=` | Public |
| 📝 Register | `/register` | Public |
| 🔐 Login | `/login` | Public |
| 📊 Seller Dashboard | `/seller-dashboard` | SELLER |
| ➕ Add / ✏️ Edit Car | `/add-car` · `/edit-car?id=` | SELLER |
| 🛡️ Admin Dashboard | `/admin-dashboard` | ADMIN |

---

## 👥 Team

| # | Name | Responsibility | Branch |
|---|------|---------------|--------|
| 1 | **Nethwindu** *(Lead)* | Models · Security · Home page | `main` |
| 2 | **Chanuki** | CarRepository · CarService · Car Detail | `feature/car-model-and-service` |
| 3 | **Resandu** | UserRepository · UserService · Register | `feature/user-repository-and-service` |
| 4 | **Nilanga** | Auth / Home / Page Controllers · Login | `feature/auth-and-home-controller` |
| 5 | **Chalinda** | SellerController · Seller / Add / Edit pages | `feature/seller-controller` |
| 6 | **Nadee** | AdminController · Admin Dashboard | `feature/admin-controller` |

---

<div align="center">

<img src="https://img.shields.io/badge/SE1020-Object_Oriented_Programming-a1b800?style=for-the-badge&labelColor=111111"/>

<br/><br/>
<sub>Built with ☕ Java · 🌱 Spring Boot · 🗄️ MySQL &nbsp;|&nbsp; Group 6</sub>

</div>
