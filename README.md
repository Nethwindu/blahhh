<img src="https://capsule-render.vercel.app/api?type=venom&height=300&color=a1b800&text=AutoLane&reversal=false&fontColor=FFFFFF&desc=SECOND%20HAND%20CAR%20SALES&descAlign=50&descAlignY=60&descSize=-2" width="100%"/>

<div align="center">

<p>
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge&logo=springboot&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Security-✓-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white"/>
  <img src="https://img.shields.io/badge/MySQL-8.x-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
</p>
<p>
  <img src="https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"/>
  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white"/>
  <img src="https://img.shields.io/badge/Maven-Build-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white"/>
  <img src="https://img.shields.io/badge/Lombok-Annotations-E91E63?style=for-the-badge&logo=java&logoColor=white"/>
</p>

<p>
  <img src="https://img.shields.io/badge/SE1020-OOP_Project-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Architecture-Layered_REST_API-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Auth-Session_Based-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Frontend-Vanilla_JS_+_Bootstrap_5-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/ORM-Spring_Data_JPA-a1b800?style=flat-square&labelColor=111111"/>
  <img src="https://img.shields.io/badge/Status-Active-a1b800?style=flat-square&labelColor=111111"/>
</p>

*A full-stack second-hand car marketplace built to demonstrate core OOP principles —*
*role-based access, image uploads, clean layered architecture, and a decoupled Vanilla JS frontend.*

</div>

---

<div align="center">

### `// navigate`

[`Team`](#team) &nbsp;`·`&nbsp; [`Setup`](#setup) &nbsp;`·`&nbsp; [`Architecture`](#architecture) &nbsp;`·`&nbsp; [`OOP Concepts`](#oop-concepts) &nbsp;`·`&nbsp; [`Database`](#database-schema) &nbsp;`·`&nbsp; [`API`](#api-reference) &nbsp;`·`&nbsp; [`Pages`](#pages)

</div>

---

<a name="team"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=200&lines=TEAM)](https://git.io/typing-svg)

> **6 engineers · 6 branches · one platform**

| `#` | Student ID | Name | Domain | Branch |
|:---:|:----------:|------|--------|--------|
| `01` | `IT25102440` | **Nethwindu** &nbsp;`LEAD` | Models · Security · Home | `main` |
| `02` | `IT25101445` | **Chanuki** | Car Repository · Service · Detail page | `feature/car-model-and-service` |
| `03` | `IT25101707` | **Resandu** | User Repository · Service · Register page | `feature/user-repository-and-service` |
| `04` | `IT25103697` | **Nilanga** | Auth · Home · Page Controllers · Login | `feature/auth-and-home-controller` |
| `05` | `IT25100476` | **Chalinda** | Seller Controller · Dashboard · Add · Edit | `feature/seller-controller` |
| `06` | `IT25200167` | **Nadee** | Admin Controller · Admin Dashboard | `feature/admin-controller` |

---

<a name="setup"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=200&lines=SETUP)](https://git.io/typing-svg)

```bash
# 01 — clone
git clone https://github.com/IT25102440/AutoLane.git && cd AutoLane

# 02 — create the database
mysql -u root -p -e "CREATE DATABASE carplatform;"

# 03 — run
./mvnw spring-boot:run
```

**Configure** `src/main/resources/application.properties`

```properties
spring.datasource.url       = jdbc:mysql://localhost:3306/carplatform
spring.datasource.username  = root
spring.datasource.password  = YOUR_PASSWORD
```

**Seed an admin account** — admins cannot self-register

```sql
INSERT INTO users (name, email, password, role)
VALUES ('Admin', 'admin@autolane.lk', '$2a$10$YOUR_BCRYPT_HASH', 'ADMIN');
```

> Generate hash → [bcrypt.online](https://bcrypt.online) &nbsp;·&nbsp; Tables auto-created by Hibernate on first boot

**Then open →** `http://localhost:8080`

---

<a name="architecture"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=300&lines=ARCHITECTURE)](https://git.io/typing-svg)

> Strict layered separation — each layer communicates only with its immediate neighbour.

<div align="center">
  <img src="docs/autolane_architecture.svg" width="60%"/>
</div>

---

<a name="oop-concepts"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=300&lines=OOP+CONCEPTS)](https://git.io/typing-svg)

> OOP is worth **20 marks** — every pillar is explicitly demonstrated in the codebase.

<div align="center">
  <img src="docs/autolane_oop_concepts.svg" width="70%"/>
</div>

| Pillar | Where it lives |
|--------|----------------|
| **Encapsulation** | All entity fields `private` — exposed only via Lombok `@Getter` / `@Setter` |
| **Inheritance** | `Seller` and `Admin` extend abstract `User`, inheriting `id · name · email · password` |
| **Polymorphism** | `getDashboard()` overridden per subclass — correct version invoked at runtime |
| **Abstraction** | Abstract `User` · JPA repository interfaces · Service layer hides all logic from controllers |
| **Information Hiding** | `password` marked `@JsonProperty(WRITE_ONLY)` · each layer sealed from the others |

---

<a name="database-schema"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=350&lines=DATABASE+SCHEMA)](https://git.io/typing-svg)

<div align="center">
  <img src="docs/autolane_database_schema.svg" width="70%"/>
</div>

---

<a name="api-reference"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=300&lines=API+REFERENCE)](https://git.io/typing-svg)

**`PUBLIC`** — no auth required

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/cars` | All listings |
| `GET` | `/api/cars/{id}` | Single car |
| `GET` | `/api/cars/search?brand=&location=` | Search & filter |
| `POST` | `/api/auth/register` | Register seller |
| `POST` | `/api/auth/login` | Login |
| `GET` | `/api/auth/me` | Current user + role |

**`SELLER`** — role: `SELLER` required

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/seller/cars` | Own listings |
| `POST` | `/api/seller/cars` | Add listing *(multipart/form-data)* |
| `PUT` | `/api/seller/cars/{id}` | Edit listing |
| `DELETE` | `/api/seller/cars/{id}` | Delete listing |

**`ADMIN`** — role: `ADMIN` required

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET · DELETE` | `/api/admin/cars/{id}` | Manage any listing |
| `GET · DELETE` | `/api/admin/sellers/{id}` | Manage any seller |

---

<a name="pages"></a>
[![](https://readme-typing-svg.demolab.com?font=Bebas+Neue&size=30&duration=1&pause=3000&color=a1b800&width=200&lines=PAGES)](https://git.io/typing-svg)

| Route | Page | Access |
|-------|------|:------:|
| `/` | Home — browse & search all listings | `PUBLIC` |
| `/car-detail?id=` | Car Detail — full listing with image | `PUBLIC` |
| `/register` | Register — create seller account | `PUBLIC` |
| `/login` | Login — auto-redirects by role on success | `PUBLIC` |
| `/seller-dashboard` | Seller Dashboard — listings, stats, actions | `SELLER` |
| `/add-car` · `/edit-car?id=` | Add / Edit Car — form + drag-and-drop image upload | `SELLER` |
| `/admin-dashboard` | Admin Dashboard — platform-wide management | `ADMIN` |

---

<div align="center">

<img src="https://img.shields.io/badge/SE1020-Object_Oriented_Programming-a1b800?style=for-the-badge&labelColor=111111"/>

<br/><br/>
<sub>Java &nbsp;·&nbsp; Spring Boot &nbsp;·&nbsp; MySQL &nbsp;·&nbsp; Vanilla JS &nbsp;·&nbsp; Group 149</sub>

</div>
