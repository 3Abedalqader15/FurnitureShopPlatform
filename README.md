<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1C1917,100:451a03&height=220&section=header&text=Alasturu&fontSize=70&fontColor=FAFAF9&animation=fadeIn&fontAlignY=38&desc=Premium%20Furniture%20%E2%80%A2%20Microservices%20Platform&descAlignY=58&descSize=20&descColor=D6D3D1" width="100%"/>

<a href="#">
  <img src="https://readme-typing-svg.demolab.com?font=Playfair+Display&weight=600&size=24&duration=3000&pause=800&color=78716C&center=true&vCenter=true&width=650&lines=A+furniture+shop%2C+built+like+production+software.;Clean+Architecture+%C2%B7+Microservices+%C2%B7+Event-Driven;.NET+10+%E2%80%A2+PostgreSQL+%E2%80%A2+RabbitMQ+%E2%80%A2+Redis;Discovery+%E2%80%94%3E+Booking+%E2%80%94%3E+(future)+Marketplace" alt="Typing SVG" />
</a>

<br/>

![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Microservices-1C1917?style=for-the-badge&logo=serverless&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Development-fbbf24?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-78716C?style=for-the-badge)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Angular](https://img.shields.io/badge/Angular-DD0031?style=flat-square&logo=angular&logoColor=white)
![Serilog](https://img.shields.io/badge/Serilog%20%2B%20Seq-2C3E50?style=flat-square)
![GitHub Actions](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

</div>

<br/>

## 📖 About The Project

**Alasturu** started as a real-world problem: small and mid-size furniture workshops rely on physical showrooms, social media DMs, and manual follow-up. Prices get asked ten times, booking requests get lost in chat threads, and there's no clear state for a piece — *available, reserved, sold, hidden?* Nobody knows.

This project solves that with a proper digital catalog: **browse → search → inquire → book**, backed by an architecture built to production-grade standards — not a toy CRUD app.

It's deliberately built as **Single-Shop first**, with every architectural decision made so it can evolve into a **Multi-Vendor Marketplace** later, without a rewrite.

> 🎯 **Dual purpose:** a real product for a real furniture shop, and a serious portfolio piece demonstrating production-level .NET engineering — Microservices, Event-Driven Architecture, Observability, and full-spectrum testing (QA is learned and applied *alongside* every phase of this build, not bolted on at the end).

<br/>

## 🧩 Core Features

<table>
<tr>
<td width="50%" valign="top">

**🛋️ For Customers**
- Browse a full product catalog with rich filtering
- PostgreSQL Full-Text Search (relevance-ranked)
- Favorites, product inquiries, reviews
- Structured booking flow with live status
- Email / WhatsApp notifications

</td>
<td width="50%" valign="top">

**🏪 For Shop Owners**
- Manage products, images, and inventory state
- Accept / reject / complete bookings
- Respond to customer inquiries
- View analytics: views, favorites, conversion
- Full shop profile management

</td>
</tr>
</table>

<br/>

## 🏗️ Architecture

Built as **disciplined Microservices** — enough separation to show real engineering judgment, without over-fragmenting a portfolio-scale system.

```mermaid
flowchart TB
    Client["🖥️ Angular Web App"]
    GW["🚪 API Gateway (YARP)"]

    subgraph Services["Microservices — each owns its own PostgreSQL schema"]
        ID["🔐 Identity Service<br/>Users · Auth · JWT"]
        SHOP["🏪 Shop Service<br/>Shop Profile"]
        CAT["📦 Catalog Service<br/>Products · Search · Favorites"]
        BOOK["📅 Booking Service<br/>Reservations · State Machine"]
        NOTIF["🔔 Notification Service<br/>Email · WhatsApp"]
    end

    MQ["🐰 RabbitMQ + MassTransit<br/>(Outbox Pattern)"]
    REDIS[("⚡ Redis Cache")]
    PG[("🐘 PostgreSQL")]
    SEQ["📊 Serilog → Seq"]

    Client -->|HTTPS| GW
    GW --> ID
    GW --> SHOP
    GW --> CAT
    GW --> BOOK

    BOOK -->|BookingCreated| MQ
    CAT -->|InquirySubmitted| MQ
    MQ -->|consume| NOTIF

    ID --- PG
    SHOP --- PG
    CAT --- PG
    BOOK --- PG

    CAT -.cache-aside.- REDIS
    SHOP -.cache-aside.- REDIS

    Services -.structured logs.- SEQ

    style Client fill:#FAFAF9,stroke:#1C1917,color:#1C1917
    style GW fill:#1C1917,stroke:#1C1917,color:#FAFAF9
    style MQ fill:#FF6600,stroke:#451a03,color:#fff
    style REDIS fill:#DC382D,stroke:#451a03,color:#fff
    style PG fill:#4169E1,stroke:#451a03,color:#fff
```

**Every service follows Clean Architecture internally:**

```
Domain  →  Application  →  Infrastructure  →  Api
(rules)    (use cases)     (EF Core, etc.)   (endpoints)
```

The dependency arrows only ever point **inward**. The Domain layer never knows PostgreSQL, RabbitMQ, or HTTP exist — swap any of them out, the business rules don't change.

<br/>

## 🛠️ Tech Stack

<div align="center">

<img src="https://skillicons.dev/icons?i=cs,dotnet,angular,ts,postgres,redis,rabbitmq,docker,githubactions,git&theme=light" />

</div>

| Layer | Technology | Why |
|---|---|---|
| **Backend** | ASP.NET Core (.NET 10), EF Core | Web API per service, Clean Architecture |
| **Database** | PostgreSQL | One schema per service + Full-Text Search (no Elasticsearch needed yet) |
| **Caching** | Redis | Cache-Aside pattern for hot reads (categories, product details) |
| **Messaging** | RabbitMQ + MassTransit | Async events, Outbox Pattern, Dead-Letter handling |
| **Background Jobs** | Hangfire | Booking expiration, notification retries |
| **Observability** | Serilog + Seq | Structured logs, Correlation ID across services |
| **Gateway** | YARP | Single entry point, routing, no business logic |
| **Frontend** | Angular + TypeScript | Reactive Forms, signals-first state |
| **Testing** | xUnit, FluentAssertions, Testcontainers, Playwright | Unit → Integration → E2E |
| **DevOps** | Docker, GitHub Actions | Reproducible local + CI/CD pipeline |

<br/>

## 🗺️ Roadmap — 20 Phases

<details>
<summary><b>Click to expand the full delivery roadmap</b></summary>
<br/>

- [x] **Phase 0** — Discovery & Final Planning
- [ ] **Phase 1** — Repository & Engineering Foundation 🚧 *(in progress)*
- [ ] **Phase 2** — Cross-Cutting Building Blocks
- [ ] **Phase 3** — Identity Service
- [ ] **Phase 4** — API Gateway
- [ ] **Phase 5** — Shop Service
- [ ] **Phase 6** — Catalog Foundation
- [ ] **Phase 7** — PostgreSQL Full-Text Search
- [ ] **Phase 8** — Favorites, Reviews & Views
- [ ] **Phase 9** — Inquiry Flow
- [ ] **Phase 10** — Booking Service *(state machine — deep QA territory)*
- [ ] **Phase 11** — RabbitMQ & MassTransit
- [ ] **Phase 12** — Notification Service
- [ ] **Phase 13** — Hangfire Jobs
- [ ] **Phase 14** — Admin Dashboard
- [ ] **Phase 15** — Observability & Hardening
- [ ] **Phase 16** — Full QA & Security Hardening
- [ ] **Phase 17** — Containerization & Staging
- [ ] **Phase 18** — CI/CD Completion
- [ ] **Phase 19** — Production Launch
- [ ] **Phase 20** — Post-Production Operations

</details>

<br/>

## 🧪 Testing Philosophy

This project is also a **QA learning ground** — every phase ships with both:

- **Manual QA** — written test plans, exploratory testing, structured bug reports per feature
- **Automated Testing** — Unit tests (business rules), Integration tests (real PostgreSQL/Redis via Testcontainers), and End-to-End flows (Playwright)

The **Booking state machine** (`Pending → Confirmed → Completed`, with `Rejected` / `Cancelled` / `Expired` branches) is treated as the flagship QA case study — it's where concurrency, idempotency, and edge-case thinking really get exercised.

<br/>

## 📂 Project Structure

```
FurnitureShopPlatform/
├── src/
│   ├── Services/
│   │   ├── Identity/      → Identity.Domain / .Application / .Infrastructure / .Api
│   │   ├── Catalog/       → (same 4-layer pattern)
│   │   ├── Shop/
│   │   ├── Booking/
│   │   └── Notification/
│   ├── Gateway/
│   │   └── ApiGateway/    → YARP routing, no business logic
│   └── BuildingBlocks/
│       └── BuildingBlocks.Common/  → shared cross-cutting code
├── tests/
├── global.json             → pins .NET SDK version
└── FurnitureShopPlatform.slnx
```

<br/>

## 🚀 Getting Started

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/FurnitureShopPlatform.git
cd FurnitureShopPlatform

# 2. Restore & build
dotnet restore
dotnet build

# 3. Spin up infrastructure (PostgreSQL, Redis, RabbitMQ, Seq)
docker compose up -d

# 4. Run a service
dotnet run --project src/Services/Identity/Identity.Api
```

<br/>

## 📊 Project Status

<div align="center">

![Progress](https://img.shields.io/badge/Overall%20Progress-Phase%201%20%2F%2020-orange?style=for-the-badge)

</div>

<br/>

## 🤝 Connect

<div align="center">

<a href="#"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/></a>
<a href="#"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white"/></a>

*Built by Abedalqader — backend .NET developer, Irbid, Jordan.*

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:451a03,100:1C1917&height=120&section=footer" width="100%"/>
