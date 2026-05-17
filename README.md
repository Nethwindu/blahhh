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

*A full-stack second-hand car marketplace built to demonstrate core OOP principles -*
*role-based access, image uploads, clean layered architecture, and a decoupled Vanilla JS frontend.*

</div>

---

<div align="center">

### `// navigate`

[`👥 Team`](#-team) &nbsp;`·`&nbsp; [`⚙️ Setup`](#️-setup) &nbsp;`·`&nbsp; [`🏗️ Architecture`](#️-architecture) &nbsp;`·`&nbsp; [`🧬 OOP`](#-oop-concepts) &nbsp;`·`&nbsp; [`🗄️ Database`](#️-database-schema) &nbsp;`·`&nbsp; [`🔌 API`](#-api-reference) &nbsp;`·`&nbsp; [`🖥️ Pages`](#️-pages)

</div>

---

## 👥 Team

> **6 engineers · 6 branches · one platform**

| `#` | Student ID | Name | Domain | Branch |
|:---:|:----------:|----------|--------|--------|
| `01` | `IT25102440` | **Nethwindu** &nbsp;`LEAD` | Models · Security · Home | `main` |
| `02` | `IT25101445` | **Chanuki** | Car Repository · Service · Detail page | `feature/car-model-and-service` |
| `03` | `IT25101707` | **Resandu** | User Repository · Service · Register page | `feature/user-repository-and-service` |
| `04` | `IT25103697` | **Nilanga** | Auth · Home · Page Controllers · Login | `feature/auth-and-home-controller` |
| `05` | `IT25100476` | **Chalinda** | Seller Controller · Dashboard · Add · Edit | `feature/seller-controller` |
| `06` | `IT25200167` | **Nadee** | Admin Controller · Admin Dashboard | `feature/admin-controller` |

---

## ⚙️ Setup

```bash
# 01 - clone
git clone https://github.com/IT25102440/AutoLane.git && cd AutoLane

# 02 - create the database
mysql -u root -p -e "CREATE DATABASE carplatform;"

# 03 - run
./mvnw spring-boot:run
```

**Configure** `src/main/resources/application.properties`

```properties
spring.datasource.url       = jdbc:mysql://localhost:3306/carplatform
spring.datasource.username  = root
spring.datasource.password  = YOUR_PASSWORD
```

**Seed an admin account** - admins cannot self-register

```sql
INSERT INTO users (name, email, password, role)
VALUES ('Admin', 'admin@autolane.lk', '$2a$10$YOUR_BCRYPT_HASH', 'ADMIN');
```

> Generate hash → [bcrypt.online](https://bcrypt.online) &nbsp;·&nbsp; Tables auto-created by Hibernate on first boot

**Then open →** `http://localhost:8080`

---

## 🏗️ Architecture

> Strict layered separation - each layer communicates only with its immediate neighbour.

```
┌─────────────────────────────────────────────────────────────────────┐
│  🌐  BROWSER   │   HTML + Bootstrap 5 + Vanilla JS                 │
│                │   fetch() calls REST endpoints - renders JSON      │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🔒  SPRING SECURITY  -  validates session · enforces roles         │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🎮  CONTROLLER LAYER   (@RestController)                           │
│      HomeController · AuthController · SellerController             │
│      AdminController · PageController                               │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  ⚙️  SERVICE LAYER   -  UserService · CarService                    │
│      Business logic · BCrypt encoding · file I/O                    │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🗃️  REPOSITORY LAYER   -  UserRepository · CarRepository           │
│      Spring Data JPA - auto-generates all SQL                       │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│  🗄️  MySQL  -  tables: users · cars                                 │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🧬 OOP Concepts

```
                    ┌───────────────────────────┐
                    │       User.java           │
                    │      〈 abstract 〉       │
                    │  id · name · email · pass │
                    │  login() · logout()       │
                    │  getDashboard()  ◄── abstract
                    └──────────┬────────────────┘
                               │  extends
               ┌───────────────┴───────────────┐
               │                               │
   ┌───────────▼────────────┐   ┌──────────────▼───────────┐
   │     Seller.java        │   │      Admin.java          │
   │  List<Car> cars        │   │                          │
   │  getDashboard()        │   │  getDashboard()          │
   │  → "seller-dashboard"  │   │  → "admin-dashboard"     │
   └────────────────────────┘   └──────────────────────────┘
```

| Pillar | Where it lives |
|--------|---------------|
| **Encapsulation** | All entity fields `private` - access via Lombok `@Getter` / `@Setter` only |
| **Inheritance** | `Seller` and `Admin` extend abstract `User`, inheriting id · name · email · password |
| **Polymorphism** | `getDashboard()` overridden per subclass - correct version invoked at runtime |
| **Abstraction** | Abstract `User` · JPA interfaces · Service layer hides all logic from controllers |
| **Information Hiding** | `password` is `@JsonProperty(WRITE_ONLY)` · each layer sealed from the others |

---

## 🗄️ Database Schema

```
┌──────────────────────────────────┐       ┌──────────────────────────────────┐
│             users                │       │              cars                │
├──────────────────────────────────┤       ├──────────────────────────────────┤
│  id        INT   PK  AUTO_INC    │◄──┐   │  id          INT  PK  AUTO_INC   │
│  name      VARCHAR(100)          │   │   │  brand       VARCHAR(100)        │
│  email     VARCHAR(150)  UNIQUE  │   │   │  model       VARCHAR(100)        │
│  password  VARCHAR(255)  BCRYPT  │   │   │  year        INT                 │
│  role      ENUM(SELLER, ADMIN)   │   │   │  price       DECIMAL(10,2)       │
│            ↑ discriminator col   │   │   │  mileage     INT                 │
└──────────────────────────────────┘   │   │  location    VARCHAR(150)        │
                                       │   │  image_path  VARCHAR(255)        │
  Single-Table Inheritance -           └───┤  seller_id   INT  FK             │
  SELLER + ADMIN share one table,          └──────────────────────────────────┘
  differentiated by the role column
                                          Cardinality:  One Seller → Many Cars
```

---

## 🔌 API Reference

**`PUBLIC`** - no auth required

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cars` | All listings |
| `GET` | `/api/cars/{id}` | Single car |
| `GET` | `/api/cars/search?brand=&location=` | Search & filter |
| `POST` | `/api/auth/register` | Register seller |
| `POST` | `/api/auth/login` | Login |
| `GET` | `/api/auth/me` | Current user + role |

**`SELLER`** - role: `SELLER` required

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/seller/cars` | Own listings |
| `POST` | `/api/seller/cars` | Add listing *(multipart)* |
| `PUT` | `/api/seller/cars/{id}` | Edit listing |
| `DELETE` | `/api/seller/cars/{id}` | Delete listing |

**`ADMIN`** - role: `ADMIN` required

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET · DELETE` | `/api/admin/cars/{id}` | Manage any listing |
| `GET · DELETE` | `/api/admin/sellers/{id}` | Manage any seller |

---

## 🖥️ Pages

| Route | Page | Access |
|-------|------|:------:|
| `/` | 🏠 Home - browse & search all listings | `PUBLIC` |
| `/car-detail?id=` | 🚗 Car Detail - full listing with image | `PUBLIC` |
| `/register` | 📝 Register - create seller account | `PUBLIC` |
| `/login` | 🔐 Login - auto-redirects by role on success | `PUBLIC` |
| `/seller-dashboard` | 📊 Seller Dashboard - listings, stats, actions | `SELLER` |
| `/add-car` · `/edit-car?id=` | ➕✏️ Add / Edit Car - listing form + image upload | `SELLER` |
| `/admin-dashboard` | 🛡️ Admin Dashboard - platform-wide management | `ADMIN` |

---

<div align="center">

<img src="https://img.shields.io/badge/SE1020-Object_Oriented_Programming-a1b800?style=for-the-badge&labelColor=111111"/>

<br/><br/>
<sub>☕ Java &nbsp;·&nbsp; 🌱 Spring Boot &nbsp;·&nbsp; 🗄️ MySQL &nbsp;·&nbsp; Group 149</sub>

</div>
