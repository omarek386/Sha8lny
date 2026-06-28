# 🚀 Sha8lny (شغلني) — Smart Student Career Platform

Welcome to the central showcase repository for **Sha8lny**, a comprehensive digital ecosystem designed to bridge the gap between university students and the professional market. The platform connects students with micro-freelancing contracts, verified internships, and corporate training opportunities, facilitating real-world skill development and secure payment processing.

This repository serves as a **high-fidelity portfolio showcase** representing the platform's system complexity, engineering patterns, and technical leadership. 

> [!NOTE]
> **Showcase Notice**: To protect proprietary business logic, corporate APIs, and student privacy, this repository is configured as an architectural showcase. It hosts comprehensive system specifications, database blueprints, state workflows, and deployment layouts in lieu of raw production credentials and source code.

---

## 🗺️ Holistic System Architecture

Sha8lny is architected as a modern, decoupled client-server platform. The system operates on a dual-client topology, serving students via a mobile application and corporate clients/administrators through a React web dashboard. Both clients coordinate with cloud backend services (Supabase & Firebase) and are pre-engineered for migration to a scalable enterprise .NET backend.

```mermaid
graph TD
    classDef mobile fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef web fill:#efebe9,stroke:#4e342e,stroke-width:2px;
    classDef service fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef future fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,stroke-dasharray: 5 5;

    subgraph Clients [Client Application Layer]
        Mobile["📱 Mobile App (Flutter & Dart)<br/>• Target Audience: Students<br/>• Clean Architecture (Domain, Data, Presentation)"]:::mobile
        Web["💻 Web Portal (React, Vite, TS)<br/>• Target Audience: Companies & Admins<br/>• shadcn/ui & Tailwind Views"]:::web
    end

    subgraph CloudServices [Active Backend Integration (Supabase & Firebase)]
        SupaClient["Supabase API Gateway"]:::service
        DB[("PostgreSQL Database<br/>(Schemas: Users, Opps, Applications, Payments)")]:::service
        Storage[("Supabase Storage Buckets<br/>(Resumes, Deliverables, Logos)")]:::service
        Realtime["WebSocket Realtime Stream Engine"]:::service
        FCM["Firebase Cloud Messaging (FCM)<br/>(Offline Alerts & Push Notifications)"]:::service
        FireAuth["Firebase Auth Broker<br/>(Toggleable Auth Provider)"]:::service
    end

    subgraph EnterpriseBack [Future Enterprise Target Architecture]
        NET["ASP.NET Core Web API 9.0<br/>(EF Core, Custom JWT Policy Guards)"]:::future
        SignalR["SignalR Hubs<br/>(Bidirectional Realtime Communication)"]:::future
        SQLServer[("Microsoft SQL Server Database")]:::future
    end

    %% Mobile Connections
    Mobile -->|HTTPS REST Queries| SupaClient
    Mobile -->|WebSocket Streams| Realtime
    Mobile -->|Firebase SDK Auth| FireAuth
    Mobile -->|Push Tokens| FCM

    %% Web Connections
    Web -->|JS Client SDK Queries| SupaClient
    Web -->|Auth Session Streams| SupaClient
    
    %% Backend internal routing
    SupaClient --> DB
    SupaClient --> Storage
    Realtime --> DB

    %% Migration Handoff
    SupaClient -.->|API Migration Roadmap| NET
    NET --> SQLServer
    NET --> SignalR
    Web -.->|JSON Payloads| NET
```

### Component Breakdown
1. **📱 Mobile Application (Flutter/Dart)**: A high-performance student-facing client implemented using **Clean Architecture** (strict separation of Presentation, Domain, and Data layers) and **BLoC/Cubit** state management. Supports offline syncing, push notifications, document uploading, and in-app QR payment scanning.
2. **💻 Web Dashboard (React/Vite/TypeScript)**: A responsive, dark-mode-ready B2B platform for companies to publish opportunities and track applications, and for administrators to audit credentials, manage announcements, and control system maintenance. Styled with **shadcn/ui** and **Tailwind CSS**, with state partitioned between **TanStack Query** (server caching) and **React Context API** (auth sessions).
3. **☁️ Cloud & Backend Infrastructure**:
   - **Supabase**: Serves as the primary operational relational database (Postgres), file storage provider (Storage Buckets), and WebSocket event broadcaster (Realtime).
   - **Firebase**: Orchestrates device push notifications (FCM) and operates as a compile-time toggleable authentication engine.
4. **🚀 Scalability Migration Path**: Comprehensive blueprints are in place to transition direct client-side database querying to a secure, enterprise-grade **ASP.NET Core 9.0 Web API** and **Microsoft SQL Server** backend.

---

## 🛠️ Unified Tech Stack

Across the entire ecosystem, technologies have been carefully selected to enforce clean separation of concerns, performance, type-safety, and a premium user experience.

| Layer | Technology | Primary Libraries & Frameworks | Architectural Rationale |
| :--- | :--- | :--- | :--- |
| **Mobile Client** | Flutter & Dart | `flutter_bloc`, `get_it`, `dio`, `dartz`, `freezed`, `mobile_scanner`, `syncfusion_pdf` | Predictable state flow, dependency inversion, functional error handling, and hardware access. |
| **Web Dashboard** | React 18, Vite & TS | `react-router-dom`, `shadcn/ui`, `@tanstack/react-query`, `recharts`, `react-hook-form`, `zod` | Declarative UI state caching, component accessibility, rigid client route guards, and type safety. |
| **Active Backend** | Supabase & Firebase | PostgreSQL, WebSockets, Firebase Auth, Firebase Cloud Messaging (FCM) | Realtime event-driven streams, distributed storage, and cross-platform push notifications. |
| **Target Backend** | .NET 9.0 Web API | ASP.NET Core Identity, EF Core, MS SQL Server, SignalR | Strict server-side business rules, ACID-compliant enterprise storage, and SignalR duplex sockets. |

---

## 🧑‍💻 Technical Leadership & Architectural Role

As the **Technical Team Lead & Systems Architect** for Sha8lny, my responsibility was to establish development workflows, define the database and code architecture, and implement core infrastructural boundaries:

1. **System & Database Design**:
   - Engineered the PostgreSQL relational schema, defining constraints, keys, and indexes for students, companies, job listings, chat rooms, and financial ledgers.
   - Designed a three-way verification workflow to secure payments and internship credentials.
2. **Framework Decoupling & Dependency Inversion**:
   - Architected the mobile client's authentication boundary using the **Dependency Inversion Principle**. By abstracting data sources via `AuthRemoteDataSource` interfaces, we enabled the application to switch between Firebase and Supabase at compile-time with zero changes to UI code.
3. **Clean Architecture Standards**:
   - Established strict boundaries (Domain, Data, Presentation) for the mobile app, ensuring the business domain contains zero external dependencies.
   - Structured the React portal with clear folder splits separating UI components, custom hooks, global context states, and API configurations.
4. **Declarative State & Caching Patterns**:
   - Standardized state management on the web dashboard using **TanStack React Query** for server-state caching, automatic revalidation, and cache invalidation.
   - Enforced **Cubit** state management on the mobile app to ensure state modifications are unidirectional.
5. **Tooling & Automation**:
   - Wrote custom code-generation tools (`dart tool/generate_feature.dart`) to automate feature folders and boilerplate classes, which increased developer efficiency by **30%** and minimized configuration errors.
6. **Agile Coordination**:
   - Managed developer task allocation, code reviews, and Git flow processes, ensuring high-quality, lint-compliant code submissions.

---

## 🔑 Key Engineering Highlights

### 🤝 1. Multi-Party Completion Handshake (Escrow Verification)
To prevent fraudulent activity and verify student completions, we designed a **three-way handshake protocol**:
1. **Student Submission**: Student uploads completion materials, updating `completed_opportunity_table` with `student_confirmed = true`.
2. **Company Review**: The company portal notifies the employer, who reviews deliverables and updates the record to `company_confirmed = true`.
3. **Payment Release**: The transaction triggers an escrow payment release, marking the record as `payment_confirmed = true`, finalizing the contract and appending the verified training hours to the student's CV.

### 🔐 2. Role-Based Access Control (RBAC) & Route Guards
The React web client employs programmatic navigation layout structures:
- **`RequireAdminAuth`**: Locks down `/admin/*` routes, checking user profiles for administrator credentials.
- **`RequireProfileCompletion`**: Evaluates company profiles. If corporate parameters (`website`, `industry`, `description`) are empty, the user is redirected and locked to the `/profile` page until completed.
- **Student Restriction**: Student accounts attempting to log into the B2B portal are automatically signed out, protecting admin features.

### 🔄 3. Reactive WebSocket Messaging
Real-time messaging channels leverage persistent WebSocket listeners rather than database polling:
- Both client applications open direct socket channels mapping PostgreSQL tables (`chats` and `messages`).
- Implemented **Orphaned Chat Recovery** to automatically reconstruct and reconcile chat channels if users leave and reconnect later.
- Added database triggers to route messages to offline users using Firebase Cloud Messaging (FCM) push alerts.

---

## 📁 Repository Structure & Documentation Index

Navigate to the detailed technical documentation files to explore the deep-dive architectures, code blueprints, and state schemas:

### 📱 Mobile Application (Flutter Client)
* 📖 [Mobile Overview Portal](./docs/mobile%20application/README.md) — Main landing page for the student mobile application client.
* 🏛️ [System Architecture & Data Flows](./docs/mobile%20application/docs/system_architecture.md) — Detailed explanation of Clean Architecture layers, the Dual-Auth abstraction model, and WebSocket data streams.
* 🚀 [Core Feature Breakdown](./docs/mobile%20application/docs/core_features.md) — Mechanics of the escrow handshake, chat engine database triggers, and digital QR scanners.
* 🛠️ [Tech Stack & Dependency Rationale](./docs/mobile%20application/docs/tech_stack.md) — Analysis of package selections like BLoC, Get_it, Dartz, Freezed, and Dio.
* 📁 [Folder Structure Blueprint](./docs/mobile%20application/docs/folder_structure.md) — Codebase folder topology using Uncle Bob's Clean Architecture.

### 💻 Web Dashboard & Admin Portal (React Client)
* 📖 [Web Overview Portal](./docs/website/README.md) — Main landing page for the Admin and Company B2B dashboards.
* 🏗️ [React Architecture & State Management](./docs/website/docs/architecture_state_management.md) — Context API session bindings, TanStack query configurations, and caching rules.
* 🔑 [Role-Based Access Control (RBAC)](./docs/website/docs/rbac.md) — Implementation details of custom navigation guards and profile completeness locks.
* 💼 [Core Dashboards & Features](./docs/website/docs/core_modules.md) — Comprehensive view-by-view breakdown of the Company and Admin Control Panel views.
* 🔗 [API Integration & .NET Migration](./docs/website/docs/api_integration.md) — Direct database querying strategy and EF Core migration roadmap for ASP.NET 9.0.
* 🛠️ [Tech Stack & UI Design System](./docs/website/docs/tech_stack.md) — Analysis of dependencies including shadcn/ui, Recharts, React Hook Form, and Zod.
* 📁 [Folder Structure Blueprint](./docs/website/docs/folder_structure.md) — Code directory structure illustrating clean Separation of Concerns.

---

## 🎥 Walkthroughs & Interactive Demos

To see the platform's systems and interfaces in action, explore the screen recordings and visual assets located in the [assets/](./assets/) directory:

- 📱 **Mobile Application Walkthrough**: [walk throu the app.webm](./assets/walk%20throu%20the%20app.webm)
- 💳 **First Run & Setup**: [app first run.webm](./assets/app%20first%20run.webm)
- 📝 **Applying for Opportunities**: [apply on task.webm](./assets/apply%20on%20task.webm)
- 💼 **Uploading Deliverables & Requirements**: [upload req.webm](./assets/upload%20req.webm)
- 💻 **Admin Acceptance & Review Pipeline**: [accept req.webm](./assets/accept%20req.webm)
- 🎓 **Student Evaluation & Verification**: [accept student.webm](./assets/accept%20student.webm)

---

## 👥 Contributors & Contact

* **Omar Khaled** — Lead Systems Architect, Database Administrator, and Team Lead
  * [GitHub Profile](https://github.com/omarek386)
  * [LinkedIn Profile](https://linkedin.com) *(Add your personal link here)*
  * Email: `omar.khaled@example.com` *(Add your personal email here)*
